---
title: "MacBook 的死与生 2011"
author: "bcalvino"
date: 2017-11-07T23:04:12-03:00
keywords: macbook pro, dGPU, arch linux, efi vars, force iGPU, hack of the year, 特梅尔之外
---

第三篇博文：我的勇敢的 _macbook pro 15' early 2011_ 坏了。令人伤心的一天。我在2011年12月从国家的Apple Store购买了它。它经历了很多事情，包括一次摔倒导致LCD显示屏更换。2013年，我的MBP首次遭遇了那个臭名昭著的AMD独立GPU（dGPU）问题。由于没有任何售后服务覆盖（我没有购买延长保修，这是一个错误，而且当时Apple还没有启动召回计划），而且避开了价格高得离谱的带Retina显示屏的新款MacBook（那时候不可能购买一台新的），我别无选择，只能花费相当于800美元购买一个新的逻辑板。

我找到了售后服务（多年来我一直选择同一个，我完全信任他们），等待了将近一个月的漫长时间才收到逻辑板，然后很快又回到了我战斗中的MBP身边。但这次不同。将近一个月前，dGPU问题又再次出现了。四年没有出现任何问题，这个时间比大多数人在更换逻辑板后报告的时间要长得多。但最终还是发生了。我将它带到了授权的Apple维修中心（之前一直去同一个地方），但机器甚至没有顺利通过门口。他们告诉我，由于被苹果视为“过时”，2011年的型号不再提供支持。下一个尝试：我将它带到了一个“非授权”的维修中心（由授权中心的人员推荐）。几天后，我收到了电子邮件确认，说这是dGPU的问题，但“他们无法解决，因为他们的供应商不再提供这块逻辑板的备件”。苹果是不是已经找到了一种彻底强制性过时化的方法？最终，在四天前，我收到了我的macbook pro，现在只是一块非常昂贵的废纸...

我的下一个决定是：既然没有人愿意解决，我自己来解决！我将购买二手配件并自己更换。即使不确定是否会得到一个寿命已经缩短的配件，也不知道自己的“自己动手”能力是否足够，我还是下定决心。第一步是在Google上搜索。由于我曾经更改过MBP的操作系统并尝试安装Linux系统，所以我搜索了在MBP上使用Linux发行版的替代

方案。令我惊讶的是，我进入了MacRumors论坛的[这个](https://forums.macrumors.com/threads/force-2011-macbook-pro-8-2-with-failed-amd-gpu-to-always-use-intel-integrated-gpu-efi-variable-fix.2037591/)讨论。由@AppleMacFinder于今年3月开始，他展示了一种可以完全恢复具有缺陷的MBP 2011的**100%有效解决方案**。这位作者，显然是来自俄罗斯，展示了一种简单而有效的方法，可以强制MBP隔离dGPU，并仅使用集成到英特尔CPU（iGPU）的图形芯片。我没有再等待，立即尝试了这个方法。

方法如下：

1- 我制作了一张 Arch Linux 的启动盘（CD/USB），选择它是因为它没有图形界面。我在一台 Windows 10 笔记本上完成了这个步骤。

2- 我通过按住 *Option* 键启动了 MBP，并在随后出现的屏幕上选择了磁盘（_EFI boot_）来启动 Arch Linux。当启动开始时，我按下 *E* 键访问 GRUB 并编辑了出现在下方的命令行，最后添加了 `nomodeset`。然后，我按下 *Enter* 键继续进行。

3- 操作系统启动后，显示了 Linux 的命令行界面。由于默认挂载了 _efivarfs_ 文件系统，可以编辑 EFI 变量，首先重新挂载驱动器以进行写入（这一步是由用户 @totoe_84 提供的）：

```bash
# cd /
# umount /sys/firmware/efi/efivars/
# mount -t efivarfs rw /sys/firmware/efi/efivars/
# cd /sys/firmware/efi/efivars/
```

4- 检查文件夹 `/sys/firmware/efi/efivars`，如果存在名为 `gpu-power-prefs-<UUID>` 的文件，则需要删除它。在我的情况下，没有这样的文件。

5- 我跳过了一些与显示问题或可能遇到的其他配置文件的建议（请仔细查看原始讨论），然后采取行动，使用任意 UUID 创建了一个新的配置（@AppleMacFinder 用户从物理卡中获取了 UUID）：

```bash
# printf "\x07\x00\x00\x00\x01\x00\x00\x00" > /sys/firmware/efi/efivars/gpu-power-prefs-fa4ce28d-b62f-4c99-9cc3-6815686e30f9
# chattr +i "/sys/firmware/efi/efivars/gpu-power-prefs-fa4ce28d-b62f-4c99-9cc3-6815686e30f9"
# cd /
# umount /sys/firmware/efi/efivars/
# reboot
```

在讨论中还介绍了其他附加方法，但我最初只做了这些。*就像魔术一样，我亲爱的 MBP 复活了！*由于在将其送到授权服务中心之前，在多次自我诊断问题的尝试中删除了操作系统，因此我使用组合键 *Option + Command + R* 启动了 Apple 的网络恢复功能。该过程顺利进行（以前在中途会卡住），下载了最新的操作系统（High Sierra），并安装并重新启动了系统。一切都很完美！或者几乎完美。在 High Sierra 启动过程中，当计算机在最后重新启动时，它又再次卡住了。在阅读论坛上同一讨论的更多内容后，我发现需要隔离一个尝试使用 AMD 的 dGPU 的配置文件。为此，我使用了用户 @MikeyN 的[指南](https://forums.macrumors.com/threads/force-2011-macbook-pro-8-2-with-failed-amd-gpu-to-always-use-intel-integrated-gpu-efi-variable-fix.2037591/page-35#post-24956091)，它与我最初使用的方法非常相似，但通过了另一条路线：

1- 通过同时按下 *Option + Control + Shift左 + Power* 来重置 SMC 和 NVRAM，然后松开并使用 *Option + Command + P + R* 启动。

2- 听到两次启动声音后，按下 *Command + R + S* 以进入 _恢复模式_。然后禁用 SIP 和 dGPU：

```bash
# csrutil disable
# nvram fa4ce28d-b62f-4c99-9cc3-6815686e30f9:gpu-power-prefs=%01%00%00%00
# nvram boot-args="-v"
# reboot
```

3- 通过按下 *Command + S* 进入单用户模式，然后挂载 _root_ 分区并将 _infrator_ 文件移动到专门创建的文件夹。

```bash
# /sbin/mount -uw /
# mkdir -p /System/Library/Extensions-off
# mv /System/Library/Extensions/AMDRadeonX3000.kext /System/Library/Extensions-off/
# touch /System/Library/Extensions/
# nvram boot-args=""
# reboot
```

还有一些额外的步骤可以防止一些用户在能源管理和其他子系统方面遇到的问题。但到目前为止，我没有需要对我的 MBP 添加其他 _hack_ 的必要。完成！我的 MBP 现在已经更新到 High Sierra，并且可以正常使用了！我已经安装了 VirtualBox、Docker、Ruby、Bundler、多个 Gems，下载了我的 Git 存储库，管理了占用大量空间的 _dicom_ 文件，一切都很顺利。感谢 @AppleMacFinder、@totoe_84、@MikeyN 和所有在 MacRumors 论坛上为创建这个巧妙的解决方案做出贡献的用户。

更新（11/12/17）：我安装了 High Sierra 的 10.13.2 更新，这似乎撤销了 _hack_ 的修改。安装在一半时停止，并导致 MacBook 进入循环重启。我只需再次按照上述描述的第二部分的所有步骤进行操作。安装继续进行，MacBook 恢复了正常运行。每次系统更新都存在再次破坏 _hack_ 的风险，也许这种方法已经无法解决。因此，我将不再进行任何更新。但是，我建议大家更新到 [High Sierra 10.13.2](https://support.apple.com/zh-cn/HT208331)，因为它修复了于 2017 年 11 月 28 日发现的一个严重的安全漏洞，该漏洞允许任何人在不需要密码的情况下访问 root 权限（o.O）。

更新（07/02/18）：我安装了 High Sierra 的 10.13.3 更新，该更新被推荐用于保护系统免受 Spectre 漏洞的攻击。同样的问题再次发生，我不得不重新应用 _hack_，并且再次成功。我没有计划进行更多的更新，因为这可能会彻底损坏系统。
