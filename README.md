This is a very early version of the MAP OpenWRT feed.
This feed adds the support for the MAP transition technology to OpenWRT with
DHCPv6-based provisioning. The option number is taken arbitrarily and will
definitely change in the future - so use this just for your own testing!

Here are the steps to add the MAP support to your OpenWRT build:

1. Open the feeds.conf.default in the editor
2. Add the line: "src-git openwrt_map https://github.com/ayourtch/openwrt-map-cernet5.git"
3. ./scripts/feeds update -a
4. ./scripts/feeds install -a -p openwrt_map

Then to build the OpenWRT with MAP support:

1. make menuconfig
2. Find the "Network" menu and select "map-mdpc" - it will autoselect odhcp6c and map-cernet items as well
3. exit the menu and "make"

Runtime configuration:

1. Configure a given interface as "DHCPv6 client"
2. Edit the /etc/config/network and add the line "option reqopts '48879'" under the interface that is configured for DHCPv6 client
3. Change the firewall configuration for IPv6 default inbound forwarding from "REJECT" to "ACCEPT" (to be investigated if a more granular setting is doable)

After the DHCP lease happens, the log is in the /tmp/map.log on the OpenWRT CPE.

