# netboot-fritzbox

A monolithic docker-compose file designed to run your own [netboot.xyz](https://github.com/netbootxyz/docker-netbootxyz) instance alongside an existing external DHCP server, such as the one built into the FRITZ!Box.

## Huh?

Usually, the DHCP server provides the required PXE boot options. Since this can't be configured on most consumer routers, we use `dnsmasq` in its ProxyDHCP mode to listen for DHCP broadcasts from PXE/iPXE clients. It then responds with the appropriate boot file location for netboot.xyz, without interfering with the DHCP server's IP address assignments.

## Install

1. Clone the repository and navigate into it:
   ```bash
   git clone https://github.com/optinux/netboot-fritzbox
   cd netboot-fritzbox
   ```
2. Edit `dnsmasq.conf` as shown inside the file
3. Start the services:
   ```bash
   docker compose up -d
   ```

## Troubleshooting

* `dnsmasq` may fail to start if port 67 is being used. Check what uses it by running `ss -ulpn | grep :67`
* Your servers firewall might be blocking port `4011` and `69`. Open these or it wont work.


## Usage

Head over to `http://<your-server-ip>:8901` and fetch the latest netboot menus. Here you can also edited the menu entries and locally store images.
If needed, hosted TFTP/web assets can be accessed directly via NGINX at `http://<your-server-ip>:8902`.

No more setup should be necessary! Except if you want to netboot Windows, this requires more [configuration](https://netboot.xyz/docs/kb/pxe/windows/). 

Since there are technically now two *competing* DHCP-Servers running in your network, loading netboot might take up to 10s. Also, the client sometimes fails to recieve its DHCP-Lease, thus netboot will prompt you to manually enter a static IP. I still havent really figured out why this happens, nothing ive tried fully fixed it. Open up a PR if you have any ideas! Still, netboot loads and is fully usable!
