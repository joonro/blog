.. title: Lenovo Y50 UHD Laptop: Driver Recommendations and Troubleshooting Information
.. slug: lenovo-y50-uhd-laptop-driver-troubleshooting
.. date: 2015/12/27 00:00
.. updated: 2015/12/27 17:00
.. tags: lenovo, y50, UHD, GTX 860M, drivers, setting, troubleshooting, windows 10
.. link: 
.. description: org file for my blog
.. type: text
.. author: Joon Ro
.. category: Hardware

Lenovo Y50 UHD is a laptop with good specs (especially for the price), as it
has a high-end quad-core processor (i7-4700HQ, passmark `7750+ <http://www.cpubenchmark.net/cpu.php?cpu=Intel+Core+i7-4700HQ+%40+2.40GHz>`_ ), 16GB Ram,
256GB SSD, UHD display (3840x2160), Nvidia Geforce GTX 860M GPU, etc. The
problem is that it suffers from many driver issues, making it unnecessarily
frustrating to use. At first I had several problems, but now my machine is
pretty reliable and stable, and I want to document driver recommendations and
troubleshooting information. I'm using Windows 10.

I feel the most important things to take care of are the SSD lock-up issue and
the Nvidia driver. I will keep this post updated as we get more driver updates.

SSD Lock-up
-----------

The system becomes not responsive for 30~ seconds, Task Manager shows 100% disk
usage, while no process is using the disk.

The solution is using ``Standard SATA AHCI Controller`` instead of intel's for
``IDE ATA/ATAPI controllers`` in ``Device Manager``.

1. Right click on the ``Intel(R) 8 Series Chipset Family SATA AHCI Controller`` and click on  ``Update Driver Software ..``

2. Select ``Browse my computer for driver software``

3. Select ``Let me pick from a list of ...``

4. Choose and install ``Standard SATA AHCI Controller``

5. Reboot

A major Windows 10 update will revert the driver back to the Intel's, so you
have to manually do this again if it happens.

Nvidia Driver for GTX 860M
--------------------------

It seems GTX 860M suffers from driver issues more than other desktop GPU's. If
you install a wrong version of the driver, you will suffer from performance
issues, crashes, and even BOSDs. Please do not update Nvidia driver without
checking the web and make sure it does not have any problems.

- Current (2015/12/27) recommended driver version is `GeForce Hot Fix driver 359.12 <http://nvidia.custhelp.com/app/answers/detail/a_id/3812/~/geforce-hot-fix-driver-359.12>`_. 
  It is for GTX 860M, but it seems it fixes problems for 960M as
  well. You can read about it at the following Nvidia forum thread: 
  `GeForce Hotfix driver 359.12 for GeForce 860M notebook GPUs (Released 12/2/15) <https://forums.geforce.com/default/topic/900924/geforce-drivers/geforce-hotfix-driver-359-12-for-geforce-860m-notebook-gpus-released-12-2-15->`_.

- The current version 361.43 seems to have less number of problems, but
  `people seem to have BOSDs <https://forums.geforce.com/default/topic/904579/geforce-drivers/official-361-43-game-ready-whql-display-driver-feedback-thread-12-21-15-/6/>`_ with ``Kernal_Security_Check_Failure`` error.

- The previously working version was 355.98. All drivers in between (358.50,
  358.87, 358.91) will cause problems.

Touchpad loses gestures after waking up
---------------------------------------

Sometimes after waking up, the Synaptics touchpad will lose gestures such as
three-finger swipe. Albeit not a permanent fix, running the following batch file will
reset the touchpad and bring back gestures. (I saved it as
``synaptics_touchpad_reset.bat``) and run it whenever it doesn't work.

.. code-block:: bat

    taskkill -f -im syntpenh.exe
    cd C:\Program Files\Synaptics\SynTP
    start "" "syntpenh.exe"
    exit

Enabling Touchpad 2- and 3-finger tap
-------------------------------------

If you want to enable touchpad 2- and 3-finger tap - for example, 2-finger tap
for the right-click and 3-finger tap for the middle-click, you can follow the
instructions at `this thread <https://forums.lenovo.com/t5/Lenovo-P-Y-and-Z-series/Y580-touchpad-two-finger-tap-for-right-click-not-working/m-p/1025407#M86254>`_. 

In sum, what you need to do is, hit windows key + x, select ``run``, enter
``regedit`` then hit enter, and on the left pane browse to the following
location:

``HKEY_USERS\<SID name for your current user>\Software\Synaptics\SynTP\TouchPadPS2``

Only issue for me was that I had two entries, ``TouchPadSMB2cTM2334`` and
``TouchPadPS2TM2334``, and ``TouchPadSMB2cTM2334`` was the one that worked.

Then, in the right pane, change values of according to the following:

2FingerTapAction
    ``2`` for right click, ``4`` for middle click.

2FingerTapPluginID
    clear any value it may have

3FingerTapAction
    ``2`` for right click, ``4`` for middle click.

3FingerTapPluginID
    clear any value it may have

3FingerTapPluginActionID
    ``0``

Reboot and the 2- and 3-finger taps will work.
