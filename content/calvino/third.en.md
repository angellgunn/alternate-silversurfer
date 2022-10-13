---
title: "Early 2011 MacBook death and hack"
author: "bcalvino"
date: 2017-11-07T23:04:12-03:00
keywords: macbook pro, dGPU, arch linux, efi vars, force iGPU, hack of the year, outTemer
---

Third post: my brave _macbook pro 15 'early 2011_ has died. Sad day. I bought it from the Apple Store in December 2011. It had already endured a lot, including a fall to the floor that earned it a change of LCD display. In 2013, the notorious AMD graphics card problem (_discrete GPU, dGPU_) hit my MBP for the first time. Without any technical support coverage (I didn't do the extended coverage, a mistake, and at the time Apple's _recall_ program had not started), and shying away from the stratospheric prices charged by the new retina screen Macbooks (impossible to buy a new one at that time), I had no choice but to pay the equivalent of $800.00 for a new logic board.

I sought the usual technical assistance (I've been their client for years, I trust them 100%), waited for the endless period of almost 1 month to receive the board and was soon back with my ol' good MBP. But not this time. Almost 1 month ago, the dGPU problem came back again. Four years without any problems, it was a much longer period than most report after the change of the logic board. But, after all, it's over. I took it to Apple's authorized service (the usual), but the machine didn't even make it through the door. The 2011 models are no longer supported because they are considered _obsolete_ by Apple, I was informed. Next attempt: I took in an "unauthorized" service (indicated by authorized personnel). A few days later, I received email confirmation that this was the dGPU issue and that _they couldn't fix it because their supplier no longer sent replacement parts for this card_. Had Apple arranged a definitive way to force scheduled obsolescence? Anyway, 4 days ago, then, I got back my macbook pro, now a very expensive paperweight ...

My next determination was: since nobody wants to solve it, I will solve it myself! I will buy the second hand piece and change it myself. Even with the uncertainty of maybe getting a part with a shortened life and not sure if my ability to do it myself would be sufficient, I was decided. The first step was to search Google. As I had tweaked the MBP operating system and tried to install a linux OS, I looked for alternatives that would use a linux distribution in MBP. To my surprise, I fell upon [this] (https://forums.macrumors.com/threads/force-2011-macbook-pro-8-2-with-failed-amd-gpu-to-always-use-intel-integrated -gpu-efi-variable-fix.2037591 /) MacRumors forum discussion. Started by @AppleMacFinder in March this year, it showed a *100% functional* solution to resurrect a 2011 MBP with the faulty graphics card. The author, apparently based in Russia, has shown a simple and effective way to force MBP to isolate dGPU and _work only with the Intel CPU integrated graphics card (iGPU)_. I didn't wait any longer and tried right away.

The method:

1- I created an Arch Linux boot disk (CD/pendrive), chosen because it has no graphical interface. I did it on a Windows 10 notebook.

2 - I started MBP from Arch Linux, holding the *Option* key while starting MBP and choosing the disk (_EFI boot_) on the screen that followed. When the boot started, I hit the *E* key to access GRUB and edit the command line that appeared below, adding `nomodeset` at the end. Then I continued the process by pressing *Enter*.

3- The OS started and presented a linux CLI. Since the _efivarfs_ file system is mounted by default, you can edit the EFI variables by first reassembling the drive to allow writing (this step was credited to user @totoe_84):

```bash
# CD /
# umount /sys/firmware/efi/efivars/
# mount -t efivarfs rw /sys/firmware/efi/efivars/
# cd /sys/firmware/efi/efivars/
```

4- Checked the `/sys/firmware/efi/efivars` folder, if there were any `gpu-power-prefs-<UUID>` file, it had to be removed. In my case, there was no such file.

5- I skipped some recommendations in case of display problems or in case of finding other configuration files (it is good to check the original discussion carefully) and set off for it, creating a new configuration using any UUID (@AppleMacFinder took from a physical board):

```bash
# printf "\x07\x00\x00\x00\x00\x00\x00\x00\x00"> /sys/firmware/efi/efivars/gpu-power-prefs-fa4ce28d-b62f-4c99-9cc3-6815686e30f9
# chattr + i "/sys/firmware/efi/efivars/gpu-power-prefs-fa4ce28d-b62f-4c99-9cc3-6815686e30f9"
# CD /
# umount /sys/firmware/efi/efivars/
# reboot
```
Other additional methods were introduced in the discussion, but initially I did just that. _Like a charm, my late MBP came back to life!_ As I had erased the OS in countless attempts to self-diagnose the issue before getting it to the authorized service, I reboot by pressing the *Option + Command + R* keys to trigger _Internet Recovery_ from Apple. The procedure worked perfectly (before freezing midway) and downloaded the newer OS (High Sierra) which was installed and rebooted. Everything perfect! Or almost. While going through the High Sierra startup steps, when the computer was restarted at the end, it froze again. After reading more of the same forum discussion, I found that it would be necessary to isolate a configuration file that was trying to use AMD's dGPU. To do this, I used the [guide](https://forums.macrumors.com/threads/force-2011-macbook-pro-8-2-with-failed-amd-gpu-to-always-use-intel-integrated -gpu-efi-variable-fix.2037591 / page-35 # post-24956091) by user @MikeyN, which does something very similar to the method I used first, but in another way:

1- I reset SMC and NVRAM by pressing *Option + Control + Left Shift + Power*, releasing and then starting with *Option + Command + P + R* pressed.

2- After hearing the startup sound twice, I pressed *Command + R + S* to start in _Recovery_ mode. Then I disabled SIP and dGPU:

```bash
# csrutil disable
# nvram fa4ce28d-b62f-4c99-9cc3-6815686e30f9: gpu-power-prefs =% 01% 00% 00% 00
# nvram boot-args = "- v"
# reboot
```

3- I restarted in single user mode by pressing *Command + S*. I then mounted the _root_ partition and moved the _offending_ file to a newly created folder.

```bash
# /sbin/mount -uw /
# mkdir -p /System/Library/Extensions-off
# mv /System/Library/Extensions/AMDRadeonX3000.kext /System/Library/Extensions-off/
# touch /System/Library/Extensions/
# nvram boot-args=""
# reboot
```

Again, there were a few more steps to prevent problems that some users have already observed with power management and other subsystems. But so far I didn't need to add any more _hack_ to my MBP. And ready! There was my MBP, upgraded to High Sierra and ready to go! I've already installed VirtualBox, Docker, Ruby, Bundler, several Gems, downloaded my Git repositories, managed my mega-space _dicom_ files, and so far so good. Thanks, @AppleMacFinder, @totoe_84, @MikeyN and all the MacRumors forum users who contributed to creating this ingenious solution that allows you to continue using a MBP with a defective graphics card.

Update (12/11/17): I installed High Sierra update 10.13.2 and this seems to have reversed the _hack_ changes. The installation halted midway and the macbook went into a restart loop. I just followed all the steps in the second part of the above description again. The installation could continue and macbook operation returned to normal. Each system update will risk breaking the _hack_ again, perhaps in ways that can no longer be solved with the method described. This way, I won't make any further updates. I urge everyone, however, to upgrade to [High Sierra 10.13.2](https://support.apple.com/en-US/HT208331) as it fixes a serious security bug detected on 11/28/17, which allowed root access to anyone without requiring a password (oO).

Update (07/02/18): I installed High Sierra update 10.13.3, which is recommended to protect the system from the Specter vulnerability. The same problem occurred and I had to apply the _hack_ again, once more succeeding. I do not plan to do any more upgrades at the risk of compromising the system altogether.
