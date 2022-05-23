# Collectd-mod-routeros-in-LuCi

**Using collectd-mod-routeros in LuCi openwrt**

[Collectd-mod-routeros](https://openwrt.org/packages/pkgdata_owrt19_7/collectd-mod-routeros) is an openwrt package which based on "[Plugin routeros](https://collectd.org/wiki/index.php/Plugin:RouterOS)" from [Collected](https://collectd.org/documentation/manpages/collectd.conf.5.shtml#plugin_routeros). The routeros plugin connects to a device running RouterOS, the Linux-based operating system for routers by MikroTik. The plugin uses librouteros to connect and reads information about the interfaces and wireless connections of the device.

Since there is no GUI support for Collectd-mod-routeros in LuCi and openwrt out of the box, I wrote this guid.

1-[Install luci-app-statistics](https://openwrt.org/docs/guide-user/luci/luci_app_statistics) and config it.

2-Install** collectd-mod-routeros package:

```
opkg update
opkg install collectd-mod-routeros
```

3-Login LuCi and make sure in "Collectd Settings" from Statistic menu, "Directory for sub-configurations" submenu is enabled with this path:

```
/etc/collectd/conf.d/*.conf
```

![](https://user-images.githubusercontent.com/22772774/169720327-7d9630a7-4884-4a79-a33a-e497dee364e8.png)

4-Create this path on your router in terminal (SSH):

```
/etc/collectd/conf.d/
```
Do not try to edit /etc/collectd.conf. LuCi will rewrite it after every reboot.

5-Make plugin config file with this name: "routeros.conf"

6-Make sure all directory and config file have 775 permision.

```
chmode 775
```

7-Put config in the "routeros.conf", base on the luci-app-statistics pattern. Something like this:

```
LoadPlugin routeros
<Plugin routeros>
    <Router>
      Host "Mikrotik IP"
      User "admin"
      Password "mikrotik password"
      CollectInterface true
    </Router>
    <Router>
      Host "Mikrotik IP"
      User "admin"
      Password "mikrotik password"
      CollectRegistrationTable true
    </Router>
</Plugin>
```
8-Restart collected service:
```
/etc/init.d/collectd restart
```
9-New files which they belong to your remote router will be appear in "RRD" folder:

![Screen Shot 2022-05-23 at 4 01 29 AM](https://user-images.githubusercontent.com/22772774/169721950-f1026902-73d4-46ed-8daf-521f6a9e2fb5.png)

![Screen Shot 2022-05-23 at 4 06 07 AM](https://user-images.githubusercontent.com/22772774/169721968-7847e49a-526f-4360-9143-0ed840cfaf03.png)


10-Make some empty folder in your router folder in RRD path with preferd name which you want apeare in graph page:

<img width="375" alt="Screen Shot 2022-05-23 at 4 09 40 AM" src="https://user-images.githubusercontent.com/22772774/169721979-02f6cf38-b0d0-486d-b585-195aeff774ce.png">


11-Symlink files in this folders to files in remote folder:
```
ln -s /overlay/rrd/192.168.88.1/routeros/if_dropped-ether1.rrd /overlay/rrd/YOUR-ROUTER-FOLDER/interface-Mikrotik-ether1/if_dropped.rrd
ln -s /overlay/rrd/192.168.88.1/routeros/if_dropped-ether2.rrd /overlay/rrd/YOUR-ROUTER-FOLDER/interface-Mikrotik-ether2/if_dropped.rrd
ln -s /overlay/rrd/192.168.88.1/routeros/if_dropped-ether3.rrd /overlay/rrd/YOUR-ROUTER-FOLDER/interface-Mikrotik-ether3/if_dropped.rrd
ln -s /overlay/rrd/192.168.88.1/routeros/if_dropped-wlan1.rrd /overlay/rrd/YOUR-ROUTER-FOLDER/interface-Mikrotik-wlan1/if_dropped.rrd
ln -s /overlay/rrd/192.168.88.1/routeros/if_dropped-wlan2.rrd /overlay/rrd/YOUR-ROUTER-FOLDER/interface-Mikrotik-wlan2/if_dropped.rrd
ln -s /overlay/rrd/192.168.88.1/routeros/if_errors-bridge.rrd /overlay/rrd/YOUR-ROUTER-FOLDER/interface-Mikrotik-bridge/if_errors.rrd
ln -s /overlay/rrd/192.168.88.1/routeros/if_errors-ether1.rrd /overlay/rrd/YOUR-ROUTER-FOLDER/interface-Mikrotik-ether1/if_errors.rrd
ln -s /overlay/rrd/192.168.88.1/routeros/if_errors-ether2.rrd /overlay/rrd/YOUR-ROUTER-FOLDER/interface-Mikrotik-ether2/if_errors.rrd
ln -s /overlay/rrd/192.168.88.1/routeros/if_errors-ether3.rrd /overlay/rrd/YOUR-ROUTER-FOLDER/interface-Mikrotik-ether3/if_errors.rrd
ln -s /overlay/rrd/192.168.88.1/routeros/if_errors-wlan1.rrd /overlay/rrd/YOUR-ROUTER-FOLDER/interface-Mikrotik-wlan1/if_errors.rrd
ln -s /overlay/rrd/192.168.88.1/routeros/if_errors-wlan2.rrd /overlay/rrd/YOUR-ROUTER-FOLDER/interface-Mikrotik-wlan2/if_errors.rrd
ln -s /overlay/rrd/192.168.88.1/routeros/if_octets-bridge.rrd /overlay/rrd/YOUR-ROUTER-FOLDER/interface-Mikrotik-bridge/if_octets.rrd
ln -s /overlay/rrd/192.168.88.1/routeros/if_octets-ether1.rrd /overlay/rrd/YOUR-ROUTER-FOLDER/interface-Mikrotik-ether1/if_octets.rrd
ln -s /overlay/rrd/192.168.88.1/routeros/if_octets-ether2.rrd /overlay/rrd/YOUR-ROUTER-FOLDER/interface-Mikrotik-ether2/if_octets.rrd
ln -s /overlay/rrd/192.168.88.1/routeros/if_octets-ether3.rrd /overlay/rrd/YOUR-ROUTER-FOLDER/interface-Mikrotik-ether3/if_octets.rrd
ln -s /overlay/rrd/192.168.88.1/routeros/if_octets-wlan1.rrd /overlay/rrd/YOUR-ROUTER-FOLDER/interface-Mikrotik-wlan1/if_octets.rrd
ln -s /overlay/rrd/192.168.88.1/routeros/if_octets-wlan2.rrd /overlay/rrd/YOUR-ROUTER-FOLDER/interface-Mikrotik-wlan2/if_octets.rrd
ln -s /overlay/rrd/192.168.88.1/routeros/if_packets-bridge.rrd /overlay/rrd/YOUR-ROUTER-FOLDER/interface-Mikrotik-bridge/if_packets.rrd
ln -s /overlay/rrd/192.168.88.1/routeros/if_packets-ether1.rrd /overlay/rrd/YOUR-ROUTER-FOLDER/interface-Mikrotik-ether1/if_packets.rrd
ln -s /overlay/rrd/192.168.88.1/routeros/if_packets-ether2.rrd /overlay/rrd/YOUR-ROUTER-FOLDER/interface-Mikrotik-ether2/if_packets.rrd
ln -s /overlay/rrd/192.168.88.1/routeros/if_packets-ether3.rrd /overlay/rrd/YOUR-ROUTER-FOLDER/interface-Mikrotik-ether3/if_packets.rrd
ln -s /overlay/rrd/192.168.88.1/routeros/if_packets-wlan1.rrd /overlay/rrd/YOUR-ROUTER-FOLDER/interface-Mikrotik-wlan1/if_packets.rrd
ln -s /overlay/rrd/192.168.88.1/routeros/if_packets-wlan2.rrd /overlay/rrd/YOUR-ROUTER-FOLDER/interface-Mikrotik-wlan2/if_packets.rrd
```
12-Restart collected service:
```
/etc/init.d/collectd restart
```
13-Graph are now avelable on graph page on interface tab.

14-You are Done.

Ps: Special thanks to "Hannu Nyman" (<hannu.nyman@iki.fi>) from collcted team, for helpful hints which put me in the right path.
