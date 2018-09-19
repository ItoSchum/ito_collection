---
date: 2017-07-26T01:34:00+08:00
description: ""
featured_image: ""
tags: []
title: "iWork 版本管理文件清理（HFS+ Only）"
featured_image: '/images/log_03/iwork_00.jpg'
---

13/06/2018 更新  
&emsp;&emsp;如今在 APFS 下，Apple 似乎已经弃用了这套版本管理逻辑，所以版本管理也不再臃肿。  

&emsp;&emsp;今天打开 About This Mac > Storage > Manage > Review Files > File Browser, 突然发现了 ~/Library/Application Support/CloudDocs/session/r 目录下大量的 iWork 版本管理文件，而且一看容量有 13.5GB（如下图）。  

{{% figure src ="/images/log_03/iwork_01.jpg" %}}


&emsp;&emsp;这样来看 iWork 文件华丽的版本管理背后并不光彩，Apple 没有选择增量备份，而是简单粗暴地用副本来完成类似 Time Machine 的版本管理（如封面图）。

&emsp;&emsp;之后不论是 File Browser 还是 Finder 都无法删除这些 History Edition 文件（如下图）。

{{% figure src ="/images/log_03/iwork_02.jpg" %}}

&emsp;&emsp;无奈只好用 rm 命令再把文件拖入 Terminal 强删（如下图）。

{{% figure src ="/images/log_03/iwork_03.jpg" %}}

&emsp;&emsp;另外不知为什么此目录下的文件虽然不是隐藏文件，但在 Terminal 中是不可见的，需 ls -a / -al（如下图）。  

{{% figure src ="/images/log_03/iwork_04.jpg" %}}

&emsp;&emsp;remove 完较大的 Keynote History Version 文件之后，算是清理出了不少硬盘空间，可能这就是所谓的 Purgeable 文件吧（如下图）。  

{{% figure src ="/images/log_03/iwork_05.jpg" %}}

&emsp;&emsp;最后再附上清理后的效果图（如下图）。

{{% figure src ="/images/log_03/iwork_06.jpg" %}}

