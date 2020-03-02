.. title: Windows 10: How to Enable Dark mode in PowerShell
.. slug: windows-10-enable-dark-mode-posh
.. date: 2016/02/10 16:00
.. tags: PowerShell, windows, settings, posh, theme, dark
.. link: 
.. description: How to enable dark mode in PowerShell
.. type: text
.. author: Joon Ro
.. category: Windows

With `Windows 10 Anniversary Update <https://blogs.windows.com/windowsexperience/2016/08/02/new-video-series-this-week-on-windows-highlights-windows-10-anniversary-update/>`_, one can choose between the `light and dark
modes <https://blogs.windows.com/windowsexperience/2016/08/08/windows-10-tip-personalize-your-pc-by-enabling-the-dark-theme/>`_. Here is a set of PowerShell commands to set this in case you want to automate
it. These are slightly modified commands from this `reddit <https://www.reddit.com/r/windows/comments/3f0n2u/windows_10_enable_dark_mode/>`_ post.

To use the dark mode:

.. code-block:: PowerShell
    :number-lines: 0

    Set-ItemProperty -Path HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\Themes\Personalize -Name AppsUseLightTheme -Value 0

To use the light mode:

.. code-block:: PowerShell
    :number-lines: 0

    Set-ItemProperty -Path HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\Themes\Personalize -Name AppsUseLightTheme -Value 1
