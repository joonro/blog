.. title: How to set up inverse and forward search in LyX for Windows
.. slug: inverse-and-forward-search-lyx-windows
.. date: 2015/10/10 00:00
.. updated: 2015/10/12 00:00
.. tags: howto, setting, windows, lyx, latex, autohotkey
.. link: 
.. description: org file for my blog
.. type: text
.. author: Joon Ro
.. category: LyX/LaTeX

I describe how to set up `inverse <https://en.wikipedia.org/wiki/Inverse_search>`_ and forward search in LyX for the Windows
environment, with `SumatraPDF <http://www.sumatrapdfreader.org/free-pdf-reader.html>`_ as the pdf viewer. Inverse search lets you
quickly move to the corresponding part of a LyX (or LaTeX) source from its pdf
output, typically by clicking on the text in the pdf viewer. Forward search
works in the opposite direction; from the LyX (or LaTeX) source, you can
invoke forward search to make the pdf viewer scroll through the pdf document
so the output counterpart of the source is shown. They are very useful
especially when you are editing a long document. Most of the instructions
overlap with those found at `http://wiki.lyx.org/LyX/SyncTeX#toc3 <http://wiki.lyx.org/LyX/SyncTeX#toc3>`_.

.. TEASER_END: click to read the rest of the article

LyX
---

1. Set ``Tools`` > ``Preferences`` > ``Paths`` > ``LyxServer pipe`` to
   ``\\.\pipe\lyxpipe``.

2. In ``Document`` > ``Settings`` > ``Output``, check ``Synchronize with output``. (this is checked by default)

Scripts
-------

Create a batch file named ``lyxeditor.cmd`` with the following contents and save
it to one of the locations in your ``PATH`` Windows environmental variable so
the pdf editor can call it:

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

The above cmd works well, but it shows an annoying black cmd window when you
invoke the script. With a simple `AutoHotkey <http://ahkscript.org>`_ script, one can not only suppress
this window but also activate the LyX window after the inverse search.

Create an AutoHotkey script named ``lyx-inverse-search.ahk`` with the following
code and save it to the same location at ``lyxeditor.cmd`` and compile it with
the AutoHotkey to generate ``lyx-inverse-search.exe``:

.. code-block:: ahk

    SetTitleMatchMode, RegEx
    Run, lyxeditor.cmd "%1%" "%2%",, Hide
    WinActivate, ^LyX:,,,

If you don't have AutoHotkey installed, you can also download the pre-compiled
`exe <https://dl.dropboxusercontent.com/u/561594/lyx-inverse-search.zip>`_.

SumatraPDF
----------

SumatraPDF is good because you can recompile your LaTeX document while the pdf
is open with SumatraPDF and the viewer will automatically refresh the pdf
file. Some pdf viewers will lock the pdf file and will not let you
re-generate the pdf file unless you close the file first. It also supports
inverse and forward search nicely.

1. Download and install `Sumatra PDF <http://blog.kowalczyk.info/software/sumatrapdf/download-free-pdf-viewer.html>`_.

2. In LyX, go to ``Tools`` > ``Preferences`` > ``Paths`` and append the install location
   of SumatraPDF to ``PATH prefix``. For example, ``C:\Program Files\SumatraPDF``.

Inverse Search
~~~~~~~~~~~~~~

In ``Tools`` > ``Preferences`` > ``File Handling`` > ``File Formats``, select ``PDF
(pdflatex)`` from the list ``Format`` and modify ``Viewer`` to the following
(including the quotes). If you have the compiled AutoHotkey script from above,
use the following instead (recommended):

.. code-block:: bat

    SumatraPDF -inverse-search "lyx-inverse-search.exe \"%f\" \"%l\""

Otherwise, use:

.. code-block:: bat

    SumatraPDF -inverse-search "lyxeditor.cmd \"%f\" \"%l\""

Now, once you do to ``Document`` -> ``View [PDF (pdflatex)]`` or ``View Master Document`` in
LyX to compile your ``.lyx`` file and invoke SumatraPDF to view the resulting
pdf, you can double-click on any text on the body of the pdf document to go to the
corresponding text in LyX.

Forward Search
~~~~~~~~~~~~~~

In LyX, go to ``Tools`` > ``Preferences`` > ``Output``, and enter the following in
``PDF command`` under ``Forward Search`` section:

.. code-block:: bat

    SumatraPDF -reuse-instance $$o -forward-search $$t $$n

Now, if you click on ``Forward Search`` in the ``Navigate`` menu in LyX, SumatraPDF
will scroll the pdf document to show the text corresponding to the text in LyX
where your cursor is and highlight the line.
