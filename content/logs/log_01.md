---
date: 2017-02-28T23:20:00+08:00
description: ""
featured_image: ""
tags: []
title: "Unlock the Mac With the Apple Watch"
featured_image: '/images/log_01/unlock_mac_over_applewatch_00.jpg'
---

&emsp;&emsp;本人在北京时间 9 月 21 日更新了 watchOS 3 与 macOS 10.12 ，尝试使用 Apple Watch 解锁Mac的新特性，遇到了一些问题，在此记录。

Sept. 21st, I updated my devices to watchOS 3 and macOS 10.12, trying Apple Watch's new feature unlocking the Mac. And there were some matters occurred, which was the reason why I logged them here.

&emsp;&emsp;“系统偏好设置”内“安全性与隐私”设置中可以找到“允许 Apple Watch 解锁 Mac ”的勾选项，但仅在满足 watchOS 3 + macOS 10.12 + 开启“双重认证”／“两步验证”的情况下会显示。

We can find the option "Allow your Apple Watch to unlock your Mac" in "System Preference", but the option only displays when "watchOS 3 + macOS 10.12 + Two-Factor Authentication/Two-Step Authentication" is available.

&emsp;&emsp;待勾选操作进行后，若之前开启了 Apple ID 的“两步验证”，系统会提示开启“双重认证”，且之前需关闭“两步验证”（因开启此项新特性需打开 Apple ID 的“双重认证”，且“双重认证”与“二步验证”只能择其一开启）

After checking the option, the system will remind you of turning on "Two-Step Authentication" with "Two-Step Authentication" off if you have already turned on your Apple ID's "Two-Step Authentication". (The new feature demands Apple ID's "Two-Step Authentication" on, which cannot be turned on when "Two-Step Authentication" is on.)

&emsp;&emsp;在关闭“两步验证”后，设置内的“允许 Apple Watch 解锁 Mac ”选项会消失，即使是开启“双重认证”后，也可能因为延迟，暂时无法进行勾选操作，虽然在“系统偏好设置”内的搜索栏内可以搜索到此项目，但选择后仍不可见。

After turning off "Two-Step Authentication", the option "Allow your Apple Watch to unlock your Mac" will disappear for a while, even though you've turned on "Two-Factor Authentication", because there might be something delayed. You can find the option in "Search", bur it still invisible after you click the searching result.

&emsp;&emsp;选项出现后的配对操作很快，解锁时仅需 Apple Watch 靠近 Mac 即可，距离较远时可能出现解锁失败需输入密码的情况，可将 Mac 睡眠后重试。（此操作类似于 Touch ID，开机首次进入用户界面时需手动输入密码）

The process of pairing your Apple Watch with your Mac is very fast. And it just demands your Apple Watch to get close to your Mac. It's possible for your Apple Watch to fail to unlock your Mack and to be required the password. You can try for one more time after sleeping your Mac.(This unlocking act is kind of Touch ID's unlocking act, which still need you to type your password for the first time entering your account after powering on your device.)

&emsp;&emsp;然而之后，若重启 Mac，那么配对的选项虽然依然在，但是 Apple Watch 的解锁特性失效。本人最初怀疑为 Apple Watch 的设置问题，于是尝试了多次对 Apple Watch 进行设置与内容抹除与恢复（不同时间段的备份与“设置为新 Apple Watch”），其中只有一次“设置为新 Apple Watch”的尝试重现了选项的消失与出现过程，再次能够成功解锁 Mac，但是仍然在重启后失效。但之后再次尝试，无果。（解锁功能失效后可以在“系统偏好设置”内解除勾选，但再次勾选会在长时间的配对后提示“ Mac 无法与 Apple Watch 正常通信”）

Then, if you try to restart your Mac, the Apple Watch's unlocking feature will be failed though the option is still checked. Firstly, I suspected there's something trouble with the Apple Watch's Setting. Consequently, I tried to Erase all of the Apple Watch's settings and content and restore for many times(I've tried both "Restore from Backup" and "Setting as New Apple Watch"). And there is only one time when I "Set it as New Apple Watch" reappeared the original process of setting the option and unlocked the Mac successfully again. Unfortunately, the unlocking feature still get failed after the restarting. And the later trying got no effect.(It's accessible to unchecking the option after it is not working, but rechecking act will fail and remind you "Mac can't communicate with the Apple Watch")

&emsp;&emsp;9 月 25 日凌晨，本人对配对 iPhone 进行了 10.0.2 的更新，更新内容包括修复了一些蓝牙连接的问题。在 OTA 更新后，Apple Watch 再次成功与 Mac 通信，解锁特性恢复。

Early morning on Sept. 25th, after updating my iPhone OTA to iOS 10.0.2, which includes fixing some Bluetooth connecting problems, the Apple Watch successfully communicated with the Mac, and the unlocking feature worked again.

&emsp;&emsp;之后本人尝试了重启 Mac 与对 iPhone 开启飞行模式的的操作，并不影响 Apple Watch 的解锁操作。这样看，似乎 Apple Watch 与 Mac 的通信并非基于 iPhone 的健全，在 Wi-Fi 环境下，且无需处于同一 WLAN，无需广域网接入，只要经过了配对验证即可。（在无 Wi-Fi 环境下，解锁操作无法进行）

After that, I had tried restarting the Mac and turned on Airplane Mode on the iPhone, which didn't have any impact on Apple Watch's unlocking feature. As you can see, there is no relevance between the iPhone and the communication between the Apple Watch and Mac after pairing, when your devices get Wi-Fi connection, even though there is no Internet connection and they are in different WLAN.(The unlocking feature is not available without Wi-Fi)

&emsp;&emsp;多日后，本人再次遭遇 Apple Watch 解锁失败的情况，在解除 iPhone 与 Apple Watch 断开连接（即关闭 iPhone 的蓝牙连接）后，Apple Watch 再次成功解锁 Mac。基于此案例，结合之前iPhone的固件更新后使得 Apple Watch 成功解锁 Mac 的案例，本人猜测，在 iPhone 与 Apple Watch 配对时，解锁操作需要通过 iPhone 健全，反之，在未配对时，不需要。

A few days later, the unlocking failure recurred. But it worked after I unconnected the Apple Watch and the iPhone(Turning off the iPhone's Bluetooth). Based on this example and the successful unlocking operation after the iPhone's software update, I guess the iPhone's verifying or etc. is required when the Apple Watch and the iPhone are connected, but when they are unconnected, it doesn't.