.. title: Syntax highlighting in LaTeX export in org-mode: specifying outputdir option for minted package
.. slug: org-mode-outputdir-minted-latex-export
.. date: 2017/02/17 18:00
.. tags: org-mode, latex, minted, export, syntax highlighting
.. link: 
.. description: Specifying outputdir for minted package syntax highlighting in LaTeX export using in org-mode
.. type: text
.. author: Joon Ro
.. category: Emacs, org-mode

When you do LaTeX export in org-mode, you can get syntax highlighting in pdf
output using ``minted`` package, which uses ``pygments``. You can add the following 
in your ``init`` file:

.. code:: common-lisp

    (require 'ox-latex)
    (add-to-list 'org-latex-packages-alist '("" "minted"))
    (setq org-latex-listings 'minted)

    (setq org-latex-pdf-process
          '("pdflatex -shell-escape -interaction nonstopmode -output-directory %o %f"
            "pdflatex -shell-escape -interaction nonstopmode -output-directory %o %f"
            "pdflatex -shell-escape -interaction nonstopmode -output-directory %o %f"))

The problem with this is that if you want to export the document to a output
directory, (e.g., using ``:EXPORT_FILE_NAME: ./Output/File_Name`` property),
then you will get the following error:

.. code:: tex

    ! Package minted Error: Missing Pygments output; \inputminted was
    probably given a file that does not exist--otherwise, you may need 
    the outputdir package option, or may be using an incompatible build tool.

because ``minted`` does not know where to find the intermediate file. 

My solution is first commenting out the ``minted`` package part:

.. code:: common-lisp

    (require 'ox-latex)
    ;(add-to-list 'org-latex-packages-alist '("" "minted"))
    (setq org-latex-listings 'minted)

    (setq org-latex-pdf-process
          '("pdflatex -shell-escape -interaction nonstopmode -output-directory %o %f"
            "pdflatex -shell-escape -interaction nonstopmode -output-directory %o %f"
            "pdflatex -shell-escape -interaction nonstopmode -output-directory %o %f"))

and then manually load ``minted`` package with specifying ``outputdir`` option in the org file:

.. code:: text

    * Test
    :PROPERTIES:
    :EXPORT_FILE_NAME: Output/File_Name
    :EXPORT_LATEX_HEADER+: \usepackage[outputdir=Output]{minted}
    :END:

and it should work.

Last, if you want to highlight ``ipython`` block, you can add the following to
your ``init`` file:

.. code:: common-lisp

    (add-to-list 'org-latex-minted-langs '(ipython "python"))
