# KDE-Kubuntu---Setup-Fixes-and-Installation-Settings-and-Tweaks---Vote-Ranked

A collection of common improvements to KDE / Kubuntu
* Vote ranked by GitHub's "reactions" (likes, or thumbs) on each Issue
* List your single configuration item as a separate Issue and vote
* **To vote for an Issue, open it, click on the smiley and add a Thumbs Up**
* Serves as a repository of crowdsourced improvements for common issues

.

This should show the Issues sorted by most 'Thumbs Up' reactions:

https://github.com/auwsom/KDE-Kubuntu---Setup-Fixes-and-Installation-Settings-and-Tweaks---Vote-Ranked/issues?utf8=%E2%9C%93&q=is%3Aissue+sort%3Areactions-%2B1-desc+

.
Quick story, I've installed Linux distros many times over the years, but have always leaned back to Windows because of many of these very issues, despite wanting to support the alternatives and not being subject to Windows OS restrictions (etc). Recently, with the mammoth size of Windows 10 and all the default bloatware, I finally resolved to switch to using VMs from a Linux based, mostly to make backup and restore more simple (a single VM file) after countless times of having to patiently wade throught the painful reinstallation process and the even more painful re-setup process. I've spent many hours searching and learning how and designing a logical and stable boot process and VM setup with backups to the VM image file(s) as well as the running user profiles. I'm creating a blog about this setup design, but for the interested (and frustrated) here is a breif outline:

- Use AIO Boot on a USB drive and drop smallish Linux ISOs in the /AIO/Files/ directory. They are automatically added to the menu upon boot. Newbs: you may have to adjust the boot order by entering the BIOS pressing F2, F8 or F12 (etc) while booting. You may also need to disable Secure Boot at later stages for "Unsigned" OSes. You should add BIOS and Hard Drive password protection if possible (after setup is standardized). You also may need to enable 'Legacy BIOS mode' (versus UEFI boot mode which uses GPT instead of MPT) to let some OSes or boot tools work. I think AIO Boot works in Legacy from USB and in UEFI from a partition. Either way it also gives you an option to boot into Windows (and probably Mac OSX if installed). The other great thing is it includes Refind tool which will search and present most installations Linux, Windows, Mac to boot without any explicit boot entry. Now you have a key back in, in case boot mechanism gets messed up on the hardrive. You can add custom Grub entries to /AIO/Main/Main.cfg.

- Boot into a Live Linux distro. I prefer Kubuntu (though larger) coming from windows, though plain Ubuntu, Lubuntu or Xubuntu (both latter two being lighter weight) work well, along with others Puppy, DSL, PortableLinux, (all the fancier flavors), etc. Access GParted which should be included standard (may not be on Ubuntu). Or install using 'sudo apt install gparted'. The great thing about Linux is most distros come with their own device 'drivers', which means most devices (the network for internet access) work right 'out of the box'. If not you might want to go buy a well known brand network device rather then hassle with getting drivers and installing from separate media.

- Create a new partition (carefully, after backing up anything irreplaceable. GParted is very stable, but just in case), usually make it NTFS format if you want to share with Windows, and copy either the partition or the files from the AIOBoot USB drive. You may have to mount the partition to copy files using 'Disk Utility' (usually standard, program is 'gnome-disks' and repo listing is 'sudo apt search udisks2'. Then you have either to go into the BIOS again to point it to /EFI/boot/grubx64.efi for AIOBoot or others in /EFI. Or use efibootmgr from linux to add an entry (if it doesn't automatically find it) and change the boot order.
https://www.rodsbooks.com/efi-bootloaders/fallback.html
https://www.rodsbooks.com/efi-bootloaders/installation.html
efibootmgr --create --disk /dev/sda --part 4 --loader \\EFI\\boot\\grubx64.efi --label AIOBOOT-sda4
efibootmgr; efibootmgr --order 0,1,2,3; efibootmgr --help

- Now you can add custom entries to Main.cfg for the AIO Boot menu. They should be in Grub format pointing to a partition or UUID. The topmost entry is default.

- Now the computer can easily boot into any OS, install your preferred flavor of Linux, and add an entry for it to the AIOBoot menu. There should already be a partition with 'boot,esp' flags in GParted (EFI System Partition). This is where the installation should add it's kernel (initrc and vmlinuz) to be mounted at /boot/efi by the OS. The kernel then hands off process 1 ususally to Systemd which runs everything else: (on KDE for example, Kernel shared memory: ksmserver; window system: X11; display servers: Xorg; desktop manager: sddm; gui toolkit for kde: qt, has gtk; window manager: KWin; desktop = plasma)  

- If the installation replaces AIOBoot, go back into the BIOS or efibootmgr to change it back. Before updating you should consider marking grub for manual update so it doesn't replace AIOBoot on every update using 'sudo apt-mark hold grub-efi-amd64 grub-efi-amd64-signed'.

- I would suggest updating and making very basic setup changes and then copy this partition to a space next to it. You'll probably want to change the UUID in GParted to make a unique Grub entry, but you may be able to load it with device id number (position). 

- Here I would install Timeshift





