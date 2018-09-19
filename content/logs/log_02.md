---
date: 2017-03-26T21:00:00+08:00
description: ""
featured_image: ""
tags: []
title: "AirPort Extreme (5th gen.) Powered by Battery"
featured_image: '/images/log_02/airport_00.jpg'
---

&emsp;&emsp;为应对寝室的 “熄灯制度”，本人长期使用一款自带锂电池的便携式无线路由器 iShare，但是受限于体积与成本，路由器本身 WLAN 的速度限制较大（大概不到 20Mbps，远不及寝室宽带的 100Mbps），因此网络体验较差。而日间使用的 AirPort Time Capsule（2013 款）由于变压器内置，只能外接交流电源供电，这使得在夜间使用几乎是不可能的。

&emsp;&emsp;为了改善寝室夜间的网络环境，结合以上情况综合考虑后，本人购入了一款二手 AirPort Extreme (5th gen.) (Model: A1408)，以下简称 A1408（如文章封面图）。A1408 的电源适配器外置，只需输入 12V 直流电即可，原装电源适配器上标示的额定输入是 12 V，1.8 A。因此，为了保证 A1408 能够整晚（23:00 - 次日 6:30）提供 WLAN，本人另购入一块 8450mah 的 12V 电池（由 18650 电芯组成）。

&emsp;&emsp;同时因为同为 AirPort，也便于本人通过 Apple 原生 App: AirPort Utility 进行管理（如图：Time Capsule 开启 Bridge 模式）。

{{% figure src ="/images/log_02/airport_01.jpg" %}}

&emsp;&emsp;之后的几天内，本人和室友在使用 A1408 的 WLAN 的过程中发现间歇性地出现网络不稳定的问题，发现在通过 SpeedTest 测速时，A1408 的测速结果不仅延迟比 Time Capsule 更大，且上下行速度都大打折扣。如图：第一项测速结果为 Time Capsule，其余项为 A1408。

{{% figure src ="/images/log_02/airport_02.jpg" %}}

&emsp;&emsp;之后偶然将 A1408 的电源输入从电池改为电源适配器，再对 A1408 进行速度测试，恢复正常（如图）。

{{% figure src ="/images/log_02/airport_03.jpg" %}}

&emsp;&emsp;总结：  
&emsp;&emsp;本想图方便，将电池的公头接入 A1408，将电池的母头接入电源，可以省下早晚间更换 A1408 电源输入的操作。然而，这一做法却导致了 A1408 电源输入不足，致使其信号强度不足，测速时会间歇性地出现带宽陡减的情况，严重影响了使用体验。

&emsp;&emsp;希望本文能对其他的 AirPort 用户提供一些参考。
