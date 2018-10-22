---
date: 2018-10-19T05:44:00+08:00
description: ""
featured_image: ""
tags: []
title: "CloudKit Keychain's Database Crash"
featured_image: '/images/log_04/CKKS_crash_00.png'
---

## 现象描述
> 包括但不限于以下表现

### 异常流量
- iOS 出现大量异常流量，如果是在 Cellular 环境下，表现为 Setting > Cellular 中，Cellular Data 栏目出现大量的 System Service 流量，表现在 Documents & Sync 栏目中，如图。

{{% figure src ="/images/log_04/CKKS_crash_01.jpg" %}} 

- 若安装了网络代理软件如 Surge，可发现异常流量主要是在与 `icloud-content.com` 通信产生的 HTTPS 下行流量，如图。

{{% figure src ="/images/log_04/CKKS_crash_02.jpg" %}}  
{{% figure src ="/images/log_04/CKKS_crash_03.png" %}}

### 设备运行异常且大量发热
- 设备可能会出现极其严重的卡顿，以至于进行难以操作，同时大量发热。

### 大量第三方应用无法打开
- 大量第三方应用可能会出现无法打开的情况，具体表现为：在 Splash Screen 卡顿，直至 10 秒后被系统强制退出的现象。

> 目前发现「腾讯系」与「阿里系」应用似乎影响较小，可能 QQ、WeChat、AliPay 等应用仍然可以打开

### iCloud Keychain 数据库大小异常
- 在 Mac 的 `~/Library/Keychains/[UUID]/keychain-2.db` 可见 `keychain-2.db` 大小异常，可能为数百兆字节甚至超过 1 吉字节（正常大小应为几十兆字节）。
- 使用 Sqlite 或相关 Sqlite 的 GUI 软件可见 `keychain-2.db` 中表 `ckmirror` 的数据项可能多达近 30000，正常情况应为几千条，正常情况如图（图为本人恢复后的截图）。

{{% figure src ="/images/log_04/CKKS_crash_04.png" %}}

> 更详细的现象描述与分析可参考：[iOS异常流量消耗及大范围应用闪退问题的分析](https://blog.nyan.im/posts/3467.html)

## 解决方案
> 本人测试有效的方案，但无法考证其通用性，也无法考证其中做法是否有冗余操作

> 虽然之前参考 [iOS异常流量消耗及大范围应用闪退问题的分析](https://blog.nyan.im/posts/3467.html) 备份了 Health 数据，本人在使用本方案恢复 CloudKit 后， Health 数据并未出现丢失，但仍然建议先对 Health 数据进行备份

1. 首先，确认在 Mac 端的 `~/Library/Keychains/[UUID]/keychain-2.db` 查看 `keychain-2.db` 大小是否异常，可用命令 `ls -ahl | sort -k5 -hr` 查看该目录下各项目大小
2. 若发现数据库大小有问题，先运行 `/usr/sbin/ckksctl` 保证自己的各项服务都处于 `logged in` 状态，否则以下操作可能无效<br>{{% figure src ="/images/log_04/CKKS_crash_04.png" %}}
3. 确认自己为 `logged in` 状态后，可使用命令 `/usr/sbin/ckksctl reset` 重置本地数据
3. 再之后用命令 `/usr/sbin/ckksctl reset-cloudkit` 重置 CloudKit 数据
4. 登出所有受影响设备的 iCloud
5. 登入一台 Mac 设备，重新登入 iCloud，并运行命令 `/usr/sbin/ckksctl reset-cloudkit` 重置 CloudKit 数据
6. 之后再陆续将其他设备的 iCloud 登入，此时 `keychain-2.db` 可能会由最初的 10MB 或更小增大，但基本维持在几十兆字节左右。

> Command Reference:
 
> `/usr/sbin/ckksctl reset`: All local data will be wiped, and data refetched from CloudKit<br>
> `/usr/sbin/ckksctl reset-cloudkit`: All data in CloudKit will be removed and replaced with what's local<br>

> 操作命令参考：[Reset Connections To ApplePay and Health With ckksctl](http://krypted.com/cloud/reset-connections-applepay-health-ckksctl/)

## Reference
> 以下为本人最初找到的有关此问题的分析与解决方案的讨论

> 在此表示感谢

- [iOS异常流量消耗及大范围应用闪退问题的分析](https://blog.nyan.im/posts/3467.html)
- [Reset Connections To ApplePay and Health With ckksctl](http://krypted.com/cloud/reset-connections-applepay-health-ckksctl/)