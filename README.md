# LaTeX template for academic papers at IU International University

This repository contains a LaTeX org-mode template for academic papers at [IU International University](https://www.iu.de/). The template is tailored to the requirements of IU International University and takes into account all formalities known to me that apply to the submission of academic papers at IU International University.

## Disclaimer

This template is provided without guarantee or warranty. The author assumes no responsibility for the accuracy or reliability of the information contained in this template or for the consequences of its use. Please use it at your own risk and responsibility.

**Note**: This is an open source project created by IU International University students. There is no official affiliation with or endorsement by IU International University.

## Usage

To use this template, simply clone or download this repository and start writing your scientific paper. Please make sure you keep the original credits in the source code. Don't worry, this will not be visible in any of your submissions.

### Configuration

Enable LaTeX export and enable :ignore: tags

```lisp
(use-package org-contrib
  :ensure t
  :defer t
  :after org)
(require 'ox-latex)
(require 'ox-extra)
(ox-extras-activate '(latex-header-blocks ignore-headlines))
```

Add support for :newpage: tag

```lisp
(defun org/parse-headings-latex-newpage (backend)
  ; add \newpage to headings with :newpage: tag
  (if (member backend '(latex))
	  (org-map-entries
	   (lambda ()
		 (progn
		   (insert "#+LATEX: \\newpage\n")
		   (setq org-map-continue-from (outline-next-heading))
		   ))
	   "+newpage")))

(add-hook 'org-export-before-parsing-hook 'org/parse-headings-latex-newpage)
(add-to-list 'org-tags-exclude-from-inheritance '"newpage")
```

Enable code block syntax highlighting (font-locking)

```lisp
(add-to-list 'org-latex-packages-alist '("" "minted"))
(setq org-latex-listings 'minted)
```

Configure the cli command (requires luatex)

```lisp
(setq org-latex-pdf-process
	  '("latexmk -pdflua -bibtex -shell-escape -f %f"))
```

Clean temporary files after export

```lisp
(setq org-latex-logfiles-extensions
	  (quote ("lof" "lot" "tex~" "aux" "idx" "log" "out"
			  "toc" "nav" "snm" "vrb" "dvi" "fdb_latexmk"
			  "blg" "brf" "fls" "entoc" "ps" "spl" "bbl"
			  "tex" "bcf" "run.xml")))
```

### Commands

To generate the PDF use the key combination `C-c C-e l o`.

### Contributions

Contribution to this template is highly welcome. Please open a pull-request with your proposed changes and do not forget to assign me, so that I get notified.

## Licence

This template is licensed under the MIT Licence. See the [LICENSE](LICENSE) file for more information.
