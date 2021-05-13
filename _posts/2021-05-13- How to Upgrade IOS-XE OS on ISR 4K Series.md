---
layout:     post
title:      How to Upgrade IOS-XE on ISR 4K Series
subtitle:   How to Upgrade IOS-XE on ISR 4K Series
date:       2021-05-13
author:     李广雄
header-img: img/post-bg-hacker.jpg
catalog: true
tags:
    - NETWORK
    - IOS-XE
---

# How to Upgrade IOS-XE OS on ISR 4K Series

## 1. Router Software

### 1.1 Router Software Component

* ROMMOM + IOS_XE = Router Software

* ROMMOM + IOS_XR = Router Software

### 1.2 Upgrade router software

1. List the reason to upgrade router software
   1. EOL/EOS
2. Check the prerequisite 
   1. Router Device
   2. License
   3. Check If the ROMMON is upgraded.
   4. Check If the ISO-XE is upgraded.
3. Update Object
   1. What's IOS-XE Version to update
4. How to upgrade
   1. Find official documents to update the Router Software
   2. Prepare the Router for update
   3. Download Router Software
   4. Upgrading Router Software
   5. Configure Router
   6. Reload

## 2. How to upgrade

### 2.1 Find official documents

**ROMMON Document**

Search google with keyword **isr 4k rommon upgrade** to find the software download site

[Upgrading Field-Programmable Hardware Devices for Cisco  4000 Series ISRs](https://www.cisco.com/c/en/us/td/docs/routers/access/4400/cpld/isr4400_hwfp.html)

**IOS-XE Upgrade**	

Search google with keyword **isr 4k ios-xe upgrade** to find the software download site

[Managing and Configuring a Router to Run Using a Consolidated Package](https://www.cisco.com/c/en/us/td/docs/routers/access/4400/software/configuration/xe-16-6/isr4400swcfg-xe-16-6-book/install.html#concept_5C163A0EE9564D83ADADF9FF29163E9A)



### 2.2 Router Software Download

[4431 Integrated Services Router](https://software.cisco.com/download/home/284358776/type) for ISR 4431 Router Software

[IOS XE Software - Fuji-16.9.7(MD)](https://software.cisco.com/download/home/284358776/type/282046477/release/Fuji-16.9.7)

![image-20210513101206319](img/image-20210513101206319.png)

 

[IOS XE ROMMON Software - 16.12(2r)](https://software.cisco.com/download/home/284358776/type/282046486/release/16.12(2r))



### 2.3 Upgrade the Router Software

#### 2.3.1 ROMMON Upgrade

1. To display the current ROMMON version at the IOS prompt

   ```shell
   Router#show rom-monitor R0        
   
   System Bootstrap, Version 16.12(2r), RELEASE SOFTWARE
   Copyright (c) 1994-2019  by cisco Systems, Inc.
   ```

2. Transfer Router Software ROMMON package file to Router.

   There is two ways to transfer ROMMON package file .

   * USB

     Copy the ROMMON ROMMON package R0 file to a USB flash drive.

   * TFTP

     1. Get information about TFTP

        ![image-20210513100713054](img/image-20210513100713054.png)

     2. Using ftp client to copy the package to tftp server

        ![image-20210513105013874](img/image-20210513105013874.png)  

     3. Copy

        ```shell
        ##
        Router# copy tftp: bootflash:
        
        ## Checking
        Router#dir
        Directory of bootflash:/
        
        ```

     4. 

3. Use the verify /md5 <filesystem>:<pkg filename> command to verify the MD5 checksum of the ROMMON package file

   Compare with software md5 in Calculated and expected value

   ```shell
   Router#verify /md5 isr4400_rommon_167_5r_SPA.pkg      
   ...............................Done!
   verify /md5 (bootflash:isr4400_rommon_167_5r_SPA.pkg) = 7b852c2927f74ac9cbe5c713eb2c98fa
   ```

   ![image-20210513130919587](img/image-20210513130919587.png)  

4. Run the upgrade rom-monitor command to begin the ROMMON upgrade process

   ```shell
   Router# upgrade rom-monitor filename bootflash:isr4400_rommon_169_1r_SPA.pkg R0.
   ROMMON upgraade complete.
   To make the new ROMMON permanent, you must restart the RP. Router
   ```

5. Reload

   ```shell
   Router# reload
   Proceed with reload? [confirm]
   (The ROMMON boots twice; on the second boot, the upgrade ROMMON starts) 
   ```

6. Confirm

   ```shell
   Router>enable
   Router#show rom-monitor R0
   ```

#### 2.3.2 IOS-XE Upgrade

1. To display the current IOS-XE version at the IOS prompt

   ```shell
   Router>enable 
   Router#show version 
   Cisco IOS XE Software, Version 03.16.06.S - Extended Support Release
   Cisco IOS Software, ISR Software (X86_64_LINUX_IOSD-UNIVERSALK9-M), Version 15.5(3)S6, RELEASE SOFTWARE (fc3)
   ```

2. Transfer Router Software ROMMON package file to Router.

   There is two ways to transfer ROMMON package file .

   * USB

     Copy the ROMMON ROMMON package R0 file to a USB flash drive.

   * TFTP

     1. Get information about TFTP

        ![image-20210513100713054](img/image-20210513100713054.png)

     2. Using ftp client to copy the package to tftp server

        ![image-20210513105013874](img/image-20210513105013874.png)  

     3. Copy

        ```shell
        Router#copy tftp: bootflash:
        Address or name of remote host []? 10.76.76.160
        Source filename []? isr4400-universalk9.16.09.07.SPA.bin  
        Destination filename [isr4400-universalk9.16.09.07.SPA.bin]? 
        Accessing tftp://10.76.76.160/isr4400-universalk9.16.09.07.SPA.bin...
        
        
        Router# dir bootflash:
        ```

3. Use the verify /md5 <filesystem>:<pkg filename> command to verify the MD5 checksum of the IOS-XE package file

   ```shell
   Router#verify /md5 isr4400-universalk9.16.09.06.SPA.bin
   ......................................Done!
   verify /md5 (bootflash:isr4400-universalk9.16.09.06.SPA.bin) = 717625455dcba3677a36767567cfd33c
   ```

   ![image-20210512160936882](img/image-20210512160936882.png)

4. Run the upgrade rom-monitor command to begin the IOS-XE upgrade process

   ```shell
   Router# conf t
   Enter configuration commands, one per line. End with CNTL/Z.
   
   Router(config)# no boot system flash bootflash:isr4400-universalk9.03.16.06.S.155-3.S6-ext.SPA.bin
   Router(config)# boot system flash bootflash:isr4400-universalk9.16.09.06.SPA.bin
   Router(config)# exit
   
   ##
   Router# show run | include boot
   boot-start-marker
   boot system flash bootflash:isr4400-universalk9.16.09.07.SPA.bin
   boot-end-marker
   Router# copy run start
   Destination filename [startup-config]?
   Building configuration...
   [OK]
   Router# reload
   ```

5. Reload

   ```shell
   Router# reload
   ```

6. Confirm

   ```shell
   
   ```

## 3. LAB

1. Upgrade IOS-XE Version 03.16.06.S to 16.09.06
2. Upgrade IOS-XE Version 16.09.06 to 16.10.1a
3. Upgrade ROMMON 16.2 to ROMMON 16.12(2r)

## 4. FAQ

1. Why update the ROMMON?
2. Why update the IOS-XE version?
3. How to upgrade the Router Software

## 5. Reference

ROMMON Upgrade:

[Upgrading Field-Programmable Hardware Devices for Cisco 4000 Series ISRs](https://www.cisco.com/c/en/us/td/docs/routers/access/4400/cpld/isr4400_hwfp.html#pgfId-1086477)

IOS-XE Upgrade:

[Cisco 4000 Series ISRs Software Configuration Guide, Cisco IOS XE Everest 16.6](https://www.cisco.com/c/en/us/td/docs/routers/access/4400/software/configuration/xe-16-6/isr4400swcfg-xe-16-6-book/install.html#concept_5C163A0EE9564D83ADADF9FF29163E9A) 

License:

[Activating Boost Performance License in CSL Mode](https://www.cisco.com/c/en/us/td/docs/routers/access/4400/software/configuration/xe-16-6/isr4400swcfg-xe-16-6-book/install.html#concept_x2p_pkj_ndb) 

