---
layout:     post
title:      Cisco Router Connection
subtitle:   How to set the connection with the Cisco Router 
date:       2021-05-13
author:     李广雄
header-img: img/post-bg-hacker.jpg
catalog: true
tags:
    -  Cisco
    -  Network
---

# Cisco Router Conf

## 1. User Management

### 1.1 User

1. Add  normal user

   ```shell
   R2(config)#username stone password cisco
   ```

2. Add admin user

   ```shell
   R2(config)#username admin privilege 15 secret cisco
   ```

### 1.2 password

1. password
2. secret

## 2. Connection  Manager

3个终端

* console
* aux
* vty

### 2.1 telnet

telnet Conf

```shell
R2>enable 
R2#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
R2(config)#lin vty 0 4
R2(config-line)#transport input telnet 
R2(config-line)#login local 
R2(config-line)#end 
```



### 2.2 SSH

## Reference

[Configure a Cisco router with Username and Password](https://community.cisco.com/t5/switching/configure-a-cisco-router-with-username-and-password/td-p/2607351)

https://www.learncisco.net/courses/icnd-1/network-environment-management/remote-devices.html