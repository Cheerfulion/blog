---
title: Ubuntu 矫正系统时间
description: 暂无描述！
tags:
  - 运维
  - Linux
abbrlink: 9d877af2
date: 2021-01-08 23:43:28
---



参考：https://blog.csdn.net/qq_19734597/article/details/102881741



当前ubuntu版本 16.04



1. 安装ntpdate，同步标准时间

   ```bash
   sudo apt install ntpdate
   ```

2. 修改时区

   ```bash
   root@wen80:~/ionluo/scripts# tzselect
   Please identify a location so that time zone rules can be set correctly.
   Please select a continent, ocean, "coord", or "TZ".
    1) Africa
    2) Americas
    3) Antarctica
    4) Asia
    5) Atlantic Ocean
    6) Australia
    7) Europe
    8) Indian Ocean
    9) Pacific Ocean
   10) coord - I want to use geographical coordinates.
   11) TZ - I want to specify the time zone using the Posix TZ format.
   #? 5
   Please select a country whose clocks agree with yours.
   1) Bermuda
   2) Cape Verde
   3) Falkland Islands
   4) Faroe Islands
   5) Iceland
   6) Portugal
   7) South Georgia & the South Sandwich Islands
   8) Spain
   #?
   (web) root@wen80:~/ionluo/scripts# tzselect
   Please identify a location so that time zone rules can be set correctly.
   Please select a continent, ocean, "coord", or "TZ".
    1) Africa
    2) Americas
    3) Antarctica
    4) Asia
    5) Atlantic Ocean
    6) Australia
    7) Europe
    8) Indian Ocean
    9) Pacific Ocean
   10) coord - I want to use geographical coordinates.
   11) TZ - I want to specify the time zone using the Posix TZ format.
   #? 4
   Please select a country whose clocks agree with yours.
    1) Afghanistan           18) Israel                35) Palestine
    2) Armenia               19) Japan                 36) Philippines
    3) Azerbaijan            20) Jordan                37) Qatar
    4) Bahrain               21) Kazakhstan            38) Russia
    5) Bangladesh            22) Korea (North)         39) Saudi Arabia
    6) Bhutan                23) Korea (South)         40) Singapore
    7) Brunei                24) Kuwait                41) Sri Lanka
    8) Cambodia              25) Kyrgyzstan            42) Syria
    9) China                 26) Laos                  43) Taiwan
   10) Cyprus                27) Lebanon               44) Tajikistan
   11) East Timor            28) Macau                 45) Thailand
   12) Georgia               29) Malaysia              46) Turkmenistan
   13) Hong Kong             30) Mongolia              47) United Arab Emirates
   14) India                 31) Myanmar (Burma)       48) Uzbekistan
   15) Indonesia             32) Nepal                 49) Vietnam
   16) Iran                  33) Oman                  50) Yemen
   17) Iraq                  34) Pakistan
   #? 9
   Please select one of the following time zone regions.
   1) Beijing Time
   2) Xinjiang Time
   #? 1
   
   The following information has been given:
   
           China
           Beijing Time
   
   Therefore TZ='Asia/Shanghai' will be used.
   Selected time is now:   Mon Dec 28 17:02:16 CST 2020.
   Universal Time is now:  Mon Dec 28 09:02:16 UTC 2020.
   Is the above information OK?
   1) Yes
   2) No
   #? 1
   
   You can make this change permanent for yourself by appending the line
           TZ='Asia/Shanghai'; export TZ
   to the file '.profile' in your home directory; then log out and log in again.
   
   Here is that TZ value again, this time on standard output so that you
   can use the /usr/bin/tzselect command in shell scripts:
   Asia/Shanghai
   ```

3. 保存时区文件

   ```bash
   cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
   ```

4. 保存到系统底层

   ```bash
   hwclock --systohc
   ```

5. 查看系统时间

   ```bash
   date
   ```

   