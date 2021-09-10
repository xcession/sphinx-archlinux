=================
Post Installation
=================

Microcode
=========

Processor manufacturers release stability and security updates to the processor microcode. These updates provide bug fixes can be critical to the stability of your system.

All users with an AMD or Intel CPU should install the microcode updates to ensure system stability.

Installation
------------

For AMD processors:

.. code-block:: bash

   $ sudo pacman -S amd-ucode

For Intel processors:

.. code-block:: bash

   $ sudo pacman -S intel-ucode

Early Microcode Loading (GRUB)
------------------------------

``grub-mkconfig`` will automatically detect the microcode update and configure GRUB appropriately.

After install the microcode package, regenerate the GRUB config to activate loading the microcode update:

.. code-block:: bash

   # grub-mkconfig -o /boot/grub/grub.cfg

Display Driver
==============

Identify the Graphic Card
-------------------------

To identify the graphic card:

.. code-block:: bash

   $ lspci -v | grep -A1 -e VGA -e 3D

.. code-block::

   00:02.0 VGA compatible controller: VMware SVGA II Adapter (prog-f 00 [VGA controller])
       Subsystem: VMware SVGA II Adapter
       Flags: fast devsel

.. note::

   The *Subsystem* output shows the specific model.

Install the Video Driver
------------------------

For a complete list of open-source video drivers:

.. code-block:: bash

   $ sudo pacman -Ss xf86-video

Intel:

.. code-block:: bash

   $ sudo pacman -S xf86-video-intel mesa vulkan-intel

AMD:

.. code-block:: bash

   $ sudo pacman -S xf86-video-amdgpu mesa vulkan-radeon

Nvidia:

.. code-block:: bash

   $ sudo pacman -S nvidia nvidia-utils

.. note::

   Xorg will use ``xf86-video-fbdev`` and ``xf86-video-vesa`` as a fallback driver.

Audio
=====

ALSA
----

The **Advanced Linux Sound Architecture** (ALSA) provides kernel driven sound card drivers. It replaces the original Open Sound System (OSS).

ALSA is a set of built-in Linux kernel modules. Therefore, manual installation is not necessary.

ALSA Utilities
~~~~~~~~~~~~~~

This contains (among other utilities) the ``alsamixer`` and ``amixer`` utilities.

``amixer`` is a shell command to change audio settings, while ``alsamixer`` provides a more intuitive ncurses based interface for audio device configuration.

Unmute the Channels
~~~~~~~~~~~~~~~~~~~

By default ALSA has all channels muted. Those have to be unmuted manually.

Unmute with ``amixer``:

.. code-block:: bash

   $ amixer sset Master unmute
   $ amixer sset Speaker unmute
   $ amixer sset Headphone unmute

Unmute with ``alsamixer``:

.. code-block:: bash

   $ alsamixer

.. note::

   The **MM** label below a channel indicates that the channel is muted, and **00** indicates that it is open.

Scroll to **Master** and **PCM** channels with the ``arrow`` keys and unmute them by pressing the ``m`` key.

Display Server
==============

Install Xorg
------------

To install Xorg (Full):

.. code-block:: bash

   $ sudo pacman -S xorg

To install Xorg (Minimal)

.. code-block:: bash

   $ sudo pacman -S xorg-server

Install Xinit
-------------

To install Xinit:

.. code-block:: bash

   $ sudo pacman -S xorg-xinit

Copy the Xinit sample configuration file to home directory:

.. code-block:: bash

   $ cp /etc/X11/xinit/xinitrc ~/.xinitrc

Install WM or DE
================

Window Manager (WM)
-------------------

https://wiki.archlinux.org/index.php/Window_manager

The window manager is responsible for the placement of the windows, how they are arranged on the screen, how they interact with each other, and their appearance.

There are many windows manager out there, for example:

- i3
- DWM
- BSPWM

Desktop Environment (DE)
------------------------

https://wiki.archlinux.org/index.php/Desktop_environment

Building the window manager, the desktop environment comes with a suite of apps. A text editor, calendar, process viewer, file manager and other essential tools.

Some of the most popular full-blown desktop environment in Linux are:

- GNOME
- KDE
- XFCE

Display Manager
===============

https://wiki.archlinux.org/index.php/Display_manager

A display manager, or login manager, is typically a graphical user interface that displayed at the end of the boot process in place of the default shell.

Z Shell (Zsh)
=============

Identify the Current Shell
--------------------------

Identify current shell by ``echo`` the ``$SHELL`` variable:

.. code-block:: bash

   $ echo $SHELL

.. code-block::

   /bin/bash

Or by the ``$0`` variable:

.. code-block:: bash

   $ echo $0

.. code-block::

   -bash

Install the Z Shell
-------------------

To install Zsh:

.. code-block:: bash

   $ sudo pacman -S zsh

Set Default Shell
-----------------

To list all installed shells:

.. code-block:: bash

   $ chsh -l

.. code-block::

   /bin/sh
   /bin/bash

To set current user's default shell:

.. code-block:: bash

   $ chsh -s <full-path-to-shell>

.. code-block::

   Changing shell for <username>.
   Password:
   Shell changed.

To set Zsh as default shell:

.. code-block:: bash

   $ chsh -s /bin/zsh

OpenSSH
=======

Install OpenSSH
---------------

To install OpenSSH:

.. code-block:: bash

   $ sudo pacman -S openssh

Disable Root Login Over SSH
---------------------------

Edit ``/etc/ssh/sshd_config`` to disable root login over SSH:

.. code-block::

   PermitRootLogin no

.. tip::

   *(Optional)* Uncomment the *BANNER* option and edit ``/etc/issue`` for a nice MOTD message.

Enable and Start the SSH Daemon
-------------------------------

To start the SSH daemon:

.. code-block:: bash

   $ sudo systemctl start sshd.service

To auto start the SSH daemon:

.. code-block:: bash

   $ sudo systemctl enable sshd.service
