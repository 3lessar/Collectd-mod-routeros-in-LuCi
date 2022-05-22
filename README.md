# Collectd-mod-routeros-in-LuCi

**Using collectd-mod-routeros in LuCi openwrt**

[Collectd-mod-routeros](https://openwrt.org/packages/pkgdata_owrt19_7/collectd-mod-routeros) is an openwrt package which based on "[Plugin routeros](https://collectd.org/wiki/index.php/Plugin:RouterOS)" from [Collected](https://collectd.org/documentation/manpages/collectd.conf.5.shtml#plugin_routeros). The routeros plugin connects to a device running RouterOS, the Linux-based operating system for routers by MikroTik. The plugin uses librouteros to connect and reads information about the interfaces and wireless connections of the device.

**Since there is no GUI support for Collectd-mod-routeros in LuCi and openwrt,  I wrote this guid.**

**1-**[**Install luci-app-statistics**](https://openwrt.org/docs/guide-user/luci/luci_app_statistics) **and config it.**

**2-Install** collectd-mod-routeros package:

```
opkg update
opkg install collectd-mod-routeros
```

3-Login in LuCi and make sure in "Collectd Settings" in Statistic menu, "Directory for sub-configurations" submenu is enabled with this path:

```
/etc/collectd/conf.d/*.conf
```

![](https://user-images.githubusercontent.com/22772774/169720327-7d9630a7-4884-4a79-a33a-e497dee364e8.png)

4-Create this path on your router.

```
/etc/collectd/conf.d/
```

5-Make plugin config file wqith this name: "routeros.conf"

6-Make sure all directory and config file have 775 permision.

```
chmode 775
```

7-
