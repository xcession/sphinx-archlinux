============
Installation
============

.. note::

   The ``#`` symbol in the shell prompt, indicate that you are execute commands as *root* user.
   While ``$`` symbol indicate that you are execute commands as *normal* user.

Preparing
=========

Verify Internet Connection
--------------------------

Verify internet connection with the ``ping`` command:

.. code-block:: bash

   # ping -c 3 archlinux.prg

.. code-block::

   PING archlinux.org (138.201.81.199): 56 data bytes
   64 bytes from 138.201.81.199: icmp_seq=0 ttl=47 time=224.669 ms
   64 bytes from 138.201.81.199: icmp_seq=1 ttl=47 time=226.400 ms
   64 bytes from 138.201.81.199: icmp_seq=2 ttl=47 time=224.150 ms

   --- archlinux.org ping statistics ---
   3 packets transmitted, 3 packets received, 0.0% packet loss
   round-trip min/avg/max/stddev = 224.150/225.073/226.400/0.962 ms

iwctl
-----

In live session, you can connect to Wi-Fi using the ``iwctl`` command.

To get an interactive prompt:

.. code-block:: bash

  $ iwctl

.. tip::

  * In the ``iwctl`` prompt you can auto-complete commands and device names by pressing ``Tab``.
  * To exit the interactive prompt, send EOF by pressing ``Ctrl+d``.
  * You can use all commands as command line arguments without entering an interactive prompt. For example: ``iwctl device wlan0 show``.

To list all available commands:

.. code-block::

   [iwd]# help

Connect to a network
~~~~~~~~~~~~~~~~~~~~

List all Wi-Fi devices:

.. code-block::

   [iwd]# device list

Scan for networks:

.. code-block::

   [iwd]# station <device> scan

List all available networks:

.. code-block::

   [iwd]# station <device> get-networks

Connect to a network:

.. code-block::

   [iwd]# station <device> connect <SSID>

If a passphrase is required, you will prompt to enter it. Alternatively, you can supply it as a command line argument:

.. code-block:: bash

   $ iwctl --passphrase <passphrase> station <device> connect <SSID>

Set Keyboard Layout
-------------------

List available keyboard layouts:

.. code-block:: bash

   # ls -R /usr/share/kbd/keymaps

To set keyboard layout:

.. code-block:: bash

   # loadkeys uk

.. note::

   The default console keymap is ``us``

Verify the current Boot Mode
----------------------------

To verify the current boot mode, list the *efivars* directory:

.. code-block:: bash

   # ls /sys/firmware/efi/efivars

.. note::

   If the command shows the directory without error, then the system is booted in **UEFI** mode. If the directory does not exist, the system maybe booted in **BIOS** (or **Legacy**) mode.

Update the System Clock
-----------------------

To ensure the system clock is accurate:

.. code-block:: bash

   # timedatectl set-ntp true

.. note::

   To check the service status, use ``timedatectl status``.

Partition the Disk
==================

List All Available Storage Devices
----------------------------------

To list all available storage devices by use ``fdisk`` or ``lsblk`` command.

1. List all available storage devices using the ``fdisk`` command:

.. code-block:: bash

   # fdisk -l

.. code-block::

   Disk /dev/sda: 16 GiB, 17179869184 bytes, 33554432 sectors
   Disk model: Virtual Disk
   Units: sectors of 1 * 512 = 512 bytes
   Sector size (logical/physical): 512 bytes / 4096 bytes
   I/O size (minimum/optimal): 4096 bytes / 4096 bytes
   Disklabel type: dos
   Disk identifier: 0x6d265be0

   Device    Boot  Start      End  Sectors  Size Id Type
   /dev/sda1        2048   616447   614400  300M 83 Linux

2. List all available storage devices using the ``lsblk`` command:

.. code-block:: bash

   # lsblk

.. code-block::

   NAME  MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
   loop0   7:0    0 549.2M  1 loop /run/archiso/sfs/airootfs
   sda     8:0    0    16G  0 disk
    L sda1 8:1    0   300M  0 part
   sr0    11:0    1   671M  0 rom  /run/archiso/bootmnt

Create Partition Table
----------------------

..
   TODO: Need more details.

.. warning::

   This will wipe everything!

Partion the disk using ``fdisk`` command:

.. code-block:: bash

   # fdisk /dev/sdX

.. code-block::

   Create a new label
    g   create a new empty GPT partition table (for UEFI systems)
    o   create a new empty DOS partition table (for Legacy systems)
   Save & Exit
    w   write table to disk and exit
    q   quit without saving changes

Partition the disk using ``cfdisk`` command:

.. code-block:: bash

   # cfdisk /dev/sdX

Partition Scheme
----------------

.. code-block::

   Label type = dos (for Legacy systems)
   Label type = gpt (for UEFI systems)
   swap (/dev/sdX1)
     L Size = RAM amount (8GB)
     L Partition type = Linux swap
   /boot (/dev/sdX2)
     L Size = 300MB
     L Partition type = Linux filesystem (EXT2) (for Legacy systems)
     L Partition type = EFI system (FAT32) (for UEFI systems)
     L Set Bootable
   / (dev/sdX3)
     L Size = 32GB
     L Partition type = Linux filesystem (EXT4)
   /home (dev/sdX4)
     L Size = (Available disk space)
     L Partition type = Linux filesystem (EXT4)

Disk Format and Mount Filesystem
================================

Format the Partitions
---------------------

Swap Partition
~~~~~~~~~~~~~~

Format swap partition:

.. code-block:: bash

   # mkswap -L SWAP /dev/sdX1

Boot Partition
~~~~~~~~~~~~~~

For **Legacy systems** (**EXT2** filesystem):

.. code-block:: bash

   # mkfs.ext2 -L BOOT /dev/sdX2

For **UEFI systems** (**FAT32** filesystem):

.. code-block:: bash

   # mkfs.fat -F 32 -n BOOT /dev/sdX2

.. important::

   Boot partition on the UEFI system has to be **FAT32**.

Root Partition
~~~~~~~~~~~~~~

Format root partition (**EXT4** filesystem):

.. code-block:: bash

   # mkfs.ext4 -L ROOT /dev/sdX3

Home Partition
~~~~~~~~~~~~~~

Format home partition (**EXT4** filesystem):

.. code-block:: bash

   # mkfs.ext4 -L HOME /dev/sdX4

Mount the Filesystem
--------------------

Mount swap partition:

.. code-block:: bash

   # swapon /dev/sdX1

Mount root partition:

.. code-block:: bash

   # mount /dev/sdX3 /mnt

Create boot directory and mount boot partition:

.. code-block:: bash

   # mkdir /mnt/boot
   # mount /dev/sdX2 /mnt/boot

Create home directory and mount home partition:

.. code-block:: bash

   # mkdir /mnt/home
   # mount /dev/sdX4 /mnt/home

Installing
==========

Install the Base Packages
-------------------------

Install base packages and developer tools:

.. code-block:: bash

   # pacstrap /mnt base base-devel

Install a Kernel
----------------

.. code-block:: bash

   # pacstrap /mnt linux linux-firmware

Fstab
-----

..
   TODO: Need more details.

.. code-block:: bash

   # genfstab -U /mnt >> /mnt/etc/fstab

Chroot
------

..
   TODO: Need more details.

.. code-block:: bash

   # arch-chroot /mnt


Install Vim
-----------

.. code-block:: bash

   # pacman -S vim

Configure the System
====================

Time Zone
---------

Set the time zone:

.. code-block:: bash

   # ls -sf /usr/share/zoneinfo/Asia/Bangkok /etc/localtime

Run ``hwclock`` to generate ``/etc/adjtime``:

.. code-block:: bash

   # hwclock --systohc

Localization
------------

Edit ``/etc/locale.gen`` and uncomment ``en_US.UTF-8``, ``UTF-8`` and other needed locales:

.. code-block:: bash

   # vim /etc/locale.gen

Generate locale:

.. code-block:: bash

   # locale-gen

.. code-block::

   Generating locales...
     en_US.UTF-8... done
     en_US.ISO-8859-1... done
   Generation complete.

Create ``/etc/locale.conf``:

.. code-block:: bash

   # vim /etc/locale.conf

And set the ``LANG`` variable:

.. code-block::

   LANG=en_US.UTF-8

*(Optional)* Set persistent keyboard layout by create or edit ``/etc/vconsole.conf``:

.. code-block:: bash

   # vim /etc/vconsole.conf

Then add the following:

.. code-block::

   KEYMAP=uk

Network Configuration
---------------------

Hostname
~~~~~~~~

Set the *hostname* (computer name) by edit ``/etc/hostname``:

.. code-block:: bash

   # vim /etc/hostname

Then add *hostname*:

.. code-block::

   myhostname

Edit the *hosts* file:

.. code-block:: bash

   # vim /etc/hosts

Then add the following:

.. code-block::

   127.0.0.1    localhost
   ::1          localhost
   127.0.1.1    myhostname.localdomain    myhostname

.. important::

   Make sure ``myhostname`` is the same name that specified in ``/etc/hostname``.

.. note::

   If the system has a permanent IP address, it should be used instead of ``127.0.1.1``.

NetworkManager
~~~~~~~~~~~~~~

Install NetworkManager:

.. code-block:: bash

   # pacman -S networkmanager

Enable the NetworkManager system service:

.. code-block:: bash

   # systemctl enable NetworkManager.service

Install the DHCP client:

.. code-block:: bash

   # pacman -S dhcpcd

Enable the DHCP client system service:

.. code-block:: bash

   # systemctl enable dhcpcd.service

Initramfs
=========

.. note::

   Creating a new initramfs usually not required, because ``mkinitcpio`` was run on installation of the kernel package with ``basestrap``.

To re-create the initramfs image:

.. code-block:: bash

   # mkinitcpio -P

Install the Bootloader (GRUB)
=================================

For Legacy Systems
------------------

Install ``grub``:

.. code-block:: bash

   # pacman -S grub os-prober

Install bootloader:

.. code-block:: bash

   # grub-install --target=i386-pc /dev/sdX

Generate ``grub`` config file:

.. code-block:: bash

   # grub-mkconfig -o /boot/grub/grub.cfg

For UEFI Systems
----------------

Install ``grub``:

.. code-block:: bash

   # pacman -S grub os-prober efibootmgr

Install bootloader:

.. code-block:: bash

   # grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB

Generate ``grub`` config file:

.. code-block:: bash

   # grub-mkconfig -o /boot/grub/grub.cfg

Set the Root Password
=====================

To set root password:

.. code-block:: bash

   # passwd

Create a New User
=================

To create a new user:

.. code-block:: bash

   # useradd -m <newuser>

Set password of the new user:

.. code-block:: bash

   # passwd <newuser>

Add new user group(s):

.. code-block:: bash

   # usermod -aG wheel,audio,video,optical,storage <newuser>

To verify user's group(s):

.. code-block:: bash

   # groups <newuser>

.. note::

   Replace ``<newuser>`` with your user name.

Install sudo
============

.. code-block:: bash

   # pacman -S sudo vi

Enable *wheel* group for ``sudo``:

.. code-block:: bash

   # visudo

.. important::

   In order to execute the ``visudo`` command, make sure ``vi`` is installed.

Uncomment the following line:

.. code-block::

   %wheel ALL=(ALL) ALL

Reboot
======

Exit the ``chroot`` environment:

.. code-block:: bash

   # exit

Restart the machine:

.. code-block:: bash

   # reboot

