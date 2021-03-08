title: linux挂载U盘失败
author: weizhao
date: 2019-11-24 14:51:47
tags:
---
# 问题描述：
* linux系统在未完全完成U盘的写操作时就将U盘拔掉，造成再次插入U盘时出现无法挂载的问题：

Error mounting /dev/sdb1 at /media/xxx/xx: Command-line`mount -t "ntfs" -o"uhelper=udisks2,nodev,nosuid,uid=1000,gid=1000,dmask=0077,fmask=0177""/dev/sdb1" "/media/eden/文檔"' exited with non-zero exit status14: The disk contains an unclean file system (0, 0).
Metadata kept in Windows cache, refused to mount.
Failed to mount '/dev/sdb1': Operation not permitted
The NTFS partition is in an unsafe state. Please resume andshutdown
Windows fully (no hibernation or fast restarting), or mount thevolume
read-only with the 'ro' mount option.

# 解决：

* 从错误信息中可以看到U盘的分区位于/dev/sdb1，我们使用命令：<font color="#dd0000">sudo ntfsfix /dev/sdb1</font> 来修复这个错误。
* ntfsfix 实用程序可修复一些常见的 NTFS 问题，它将修复一些基本的 NTFS 不一致性，重置 NTFS 日志文件并在下次重新引导操作系统时安排进行 NTFS 一致性检查。