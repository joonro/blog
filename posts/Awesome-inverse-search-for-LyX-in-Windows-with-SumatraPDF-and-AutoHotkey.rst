.. title: Awesome inverse search for LyX in Windows with SumatraPDF and AutoHotkey
.. slug: Awesome-inverse-search-for-LyX-in-Windows-with-SumatraPDF-and-AutoHotkey
.. date: 2015/10/10 00:00
.. tags: howto, setting, windows, lyx, latex, autohotkey
.. link: 
.. description: org file for my blog
.. type: text
.. author: Joon RoI describe how to setup inverse search between LyX and SumatraPDF and/or
Okular. Most of the instructions overlap with the ones at
`http://wiki.lyx.org/LyX/SyncTeX#toc3 <http://wiki.lyx.org/LyX/SyncTeX#toc3>`_.

LyX Settings
------------

1. Set ``Tools`` > ``Preferences`` > ``Paths`` > ``LyxServer pipe`` to
   ``\\.\pipe\lyxpipe``.

2. In ``Document`` > ``Settings`` > ``Output``, check ``Synchronize with
      output``. (checked by default)

3. Create a batch-file named ``lyxeditor.cmd`` with the following contents and
   save it to one of the locations in your ``PATH`` Windows environmental
   variable, so the pdf editor can call it:

   .. code-block:: bat

       @echo off
       SETLOCAL enabledelayedexpansion
       set file=%1
       set row=%2
       REM remove quotes from variables 
       set file=!file:"=!
       set row=!row:"=!
       %comspec% /q /c (@echo LYXCMD:revdvi:server-goto-file-row:%file% %row%)> \\.\pipe\lyxpipe.in&&type \\.\pipe\lyxpipe.out
       endlocal

AutoHotkey Script
-----------------

The above cmd works well, but it shows an annoying black cmd window everytime
when you invoke the script. With a simple AutoHotkey script, one can not only
suppress this window, but also activate the LyX window after the inverse
search.

Create a AutoHotkey script named ``lyx-inverse-search.ahk`` with the following
code and save it to the same location to ``lyxeditor.cmd`` and compile it with
AutoHotkey to generate ``lyx-inverse-search.exe``:

.. code-block:: ahk

    SetTitleMatchMode, RegEx
    Run, lyxeditor.cmd "%1%" "%2%",, Hide
    WinActivate, ^LyX:,,,

If you don't have AutoHotkey installed, you can also download the pre-compiled
`exe <https://dl.dropboxusercontent.com/u/561594/lyx-inverse-search.zip>`_.

SumatraPDF
----------

`http://wiki.lyx.org/Windows/LyXWinTips#toc6 <http://wiki.lyx.org/Windows/LyXWinTips#toc6>`_

1. Download and install `Sumatra PDF <http://blog.kowalczyk.info/software/sumatrapdf/download-free-pdf-viewer.html>`_.

2. In LyX, ``Tools`` > ``Preferences`` > ``Paths`` and append the install location
   of SumatraPDF to ``PATH prefix``. For example, ``C:\Program Files\SumatraPDF``.

3. In ``Tools`` > ``Preferences`` > ``File Handling`` > ``File Formats``
   select ``PDF (pdflatex)`` from the list ``Format`` and modify ``Viewer`` to
   the following (including quotes):

   .. code-block:: bat

       SumatraPDF -inverse-search "lyxeditor.cmd \"%f\" \"%l\""

   If you have the compiled AutoHotkey script from above, use the following
   instead (recommended): 

   .. code-block:: bat

       SumatraPDF -inverse-search "lyx-inverse-search.exe \"%f\" \"%l\""

Now once you ``Document`` -> ``View (PDF (pdflatex))`` or ``View Master Documen`` in
LyX to compile your ``.lyx`` file and invoke SumatraPDF to view the resulting
pdf, you can double-click on any text on the body of the document to go to the
corresponding text in LyX.
