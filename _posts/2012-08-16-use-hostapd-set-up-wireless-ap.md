---
layout: post
title: Linux下的无线AP
tags: linux,wireless,ap,无线
---

{{ page.title }}
================

在Windows 7下建立无线AP只需要两行命令：

	netsh wlan set hostednetwork mode=allow ssid=yourssid key=password
	netsh wlan start/stop/show hostednetwork
	# 需要外部网络的话只需在有网络的连接上建立共享即可

下面是在Linux下的方法：

0.	下载[hostapd][]软件，然后配置hostapd.conf，文件参考`/usr/share/doc/hostapd/examples/`。

		# /etc/hostapd/hostapd.conf，参考examples/hostapd.conf.gz
		interface=wlan0 # ath0 for madwifi
		driver=nl80211
		ssid=x5310 # 这里没有使用加密，仅用MAC限制
		ignore_broadcast_ssid=0
		hw_mode=g
		channel=10
		macaddr_acl=1 #  # deny unless in accept list
		accept_mac_file=/etc/hostapd/hostapd.accept

	hostapd.accept:

		# /etc/hostapd/hostapd.accept
		00:00:00:00:00:00
		11:11:11:11:11:11

	初始化hostpad服务的文件:

		# /etc/default/hostapd
		DAEMON_CONF="/etc/hostapd/hostapd.conf"

1.	设置无线参数，启动hostapd。

		ifconfig wlan0 10.1.1.1 netmask 255.255.255.0
		service hostapd start

2.	设置iptables，启用内核转发。

		# 我的iptables本就是空的
		iptables -F
		iptables -F -t nat
		iptables -t nat -A POSTROUTING -o ppp0 -j MASQUERADE

		# 内核转发，修改/etc/sysctl.conf
		net.ipv4.ip_forward=1
		# 或者直接修改proc，即时生效
		echo 1 > /proc/sys/net/ipv4/ip_forward

我只有另外的两个设备需要此AP，所以未使用dhcpd服务。

参考：

* [Linux下用hostapd架无线AP](http://ihacklog.com/post/use-hostapd-to-setup-wireless-access-point-under-linux.html)

{% include references.md %}
