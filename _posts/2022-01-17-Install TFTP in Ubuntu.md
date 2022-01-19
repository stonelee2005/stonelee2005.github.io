---
layout:     post
title:      Use TFTP in Cisco 
subtitle:   Install TFTP in Ubuntu
date:       2022-01-17
author:     Stone
header-img: img/post-bg-hacker.jpg
catalog: true
tags:
    -  Cisco
    -  tftp	
---

# Install TFTP in Ubuntu

![image-20220115215858426](2022-01-17-Install TFTP in Ubuntu.assets/image-20220115215858426.png) 

## 1. Ubuntu Install tftpd-ha

1. ubuntu18.04 install tftpd

   ```shell
   root@ubuntu:~# sudo  apt-get install tftpd-hpa tftp-hpa
   ```

2. Configurate tftp

   ```shell
   root@ubuntu:~# vim /etc/default/tftpd-hpa
   
   TFTP_USERNAME="tftp"
   TFTP_DIRECTORY="/tftp"   
   TFTP_ADDRESS=":69"
   TFTP_OPTIONS="-l -c -s"
   ```

   ```shell
   root@ubuntu:~# mkdir /tftp
   root@ubuntu:~# chmod 777 /tftp
   root@ubuntu:~# systemctl start tftpd-hpa
   ```
   
   

3. verify tftp

   ```shell
   root@ubuntu:~# ss -ntlup
   Netid  State      Recv-Q Send-Q                         Local Address:Port                                        Peer Address:Port              
   udp    UNCONN     0      0                                          *:68                                                     *:*                   users:(("dhclient",pid=1071,fd=6))
   udp    UNCONN     0      0                                          *:69                                                     *:*                   users:(("in.tftpd",pid=1222,fd=4))
   udp    UNCONN     0      0                                         :::69                                                    :::*                   users:(("in.tftpd",pid=1222,fd=5))
   tcp    LISTEN     0      128                                        *:22                                                     *:*                   users:(("sshd",pid=1133,fd=3))
   
   root@ubuntu:~# ifconfig eth0
   eth0      Link encap:Ethernet  HWaddr 00:50:00:00:04:00  
             inet addr:192.168.75.141  Bcast:192.168.75.255  Mask:255.255.255.0
             inet6 addr: fe80::250:ff:fe00:400/64 Scope:Link
             UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
             RX packets:385 errors:0 dropped:0 overruns:0 frame:0
             TX packets:265 errors:0 dropped:0 overruns:0 carrier:0
             collisions:0 txqueuelen:1000 
             RX bytes:41405 (41.4 KB)  TX bytes:46789 (46.7 KB)
   ```
   
   

## 2.Copy Test

```shell
R3#copy running-config tftp:
Address or name of remote host []? 192.168.75.141
Destination filename [r3-confg]? test
!!
1319 bytes copied in 0.059 secs (22356 bytes/sec)
```

 
