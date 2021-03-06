# Makefile for Sphinx documentation
#
PYTHON       = python
RST2HTML     = rst2html

# You can set these variables from the command line.
SPHINXOPTS   =
SPHINXVER    = 0.5
SPHINXBUILD  = PYTHONPATH=..:$(PYTHONPATH) sphinx-build
PAPER        =

SVGFILES = $(wildcard src/modules/physics/mechanics/*.svg)
PDFFILES = $(SVGFILES:%.svg=%.pdf)

ALLSPHINXOPTS = -d _build/doctrees $(SPHINXOPTS) src
ALLSPHINXOPTSapi = -d _build/doctrees-api $(SPHINXOPTS) api
ALLSPHINXOPTSlatex = -d _build/doctrees-latex -D latex_paper_size=$(PAPER) \
                $(SPHINXOPTS) src

LANGUAGES = bg cs de fr pl ru sr

MOFILES = $(foreach l,$(LANGUAGES),$(addprefix _build/i18n/,$(addsuffix /LC_MESSAGES/tutorial.mo,${l})))
POFILES = $(foreach l,$(LANGUAGES),$(addprefix src/tutorial/tutorial.,$(addsuffix .po,${l})))

.PHONY: changes cheatsheet clean gettext help html html-errors \
        htmlapi htmlhelp htmli18n info latex linkcheck texinfo \
        update-po web

.SUFFIXES: .pdf .svg

help:
	@echo "Please use \`make <target>' where <target> is one of"
	@echo "  changes     to make an overview over all changed/added/deprecated items"
	@echo "  cheatsheet  to make the Cheatsheet"
	@echo "  clean       to remove generated files"
	@echo "  gettext     to make pot files for translation"
	@echo "  html        to make standalone HTML files"
	@echo "  html-errors to make the standalone HTML files, stopping on any errors or warnings"
	@echo "  htmlapi     to make HTML API docs"
	@echo "  htmlhelp    to make HTML files and a HTML help project"
	@echo "  htmli18n    to make the i18n versions"
	@echo "  info        to make Texinfo files and run them through makeinfo"
	@echo "  latex       to make LaTeX files, you can set PAPER=a4 or PAPER=letter"
	@echo "  linkcheck   to check all external links for integrity"
	@echo "  texinfo     to make Texinfo files"
	@echo "  update-po   to update po files for translation"
	@echo "  web         to make files usable by Sphinx.web"

clean:
	-rm -rf _build
	-rm -rf sphinx
	-rm -f $(PDFFILES)

html: htmli18n
	mkdir -p src/.static
	mkdir -p _build/html
	mkdir -p _build/doctrees
	mkdir -p src/modules
	$(SPHINXBUILD) -b html $(ALLSPHINXOPTS) _build/html
	cp -r src/pics _build/html/
	cp -r src/figures _build/html/
	@echo
	@echo "Build finished. The HTML pages are in _build/html."

html-errors: SPHINXOPTS += -W
html-errors: html

htmlapi:
	mkdir -p api/.static
	mkdir -p api/modules
	mkdir -p _build/api _build/doctreesapi
	rm -f api/modules/sympy*.rst
	$(SPHINXBUILD) -b html $(ALLSPHINXOPTSapi) _build/api
	@echo
	@echo "Build finished. The API docs pages are in _build/api."

gettext:
	$(SPHINXBUILD) -b gettext -t gettext -D master_doc=tutorial/tutorial \
		-D source_suffix=.en.rst -d _build/doctrees-en src src/tutorial
	sed -i "/^\# [0-9a-zA-Z]*$$/d" src/tutorial/tutorial.pot

update-po: $(POFILES)

htmli18n: $(MOFILES)
	mkdir -p _build/i18n/
	mkdir -p _build/html/tutorial
	for l in $(LANGUAGES); do \
		$(SPHINXBUILD) $(SPHINXOPTS) -b html -D language=$$l -D master_doc=tutorial/tutorial \
			-D source_suffix=.en.rst -d _build/doctrees-$$l src _build/i18n/$$l ; \
		cp _build/i18n/$$l/tutorial/tutorial.html _build/html/tutorial/tutorial.$$l.html ; \
	done

$(POFILES): src/tutorial/tutorial.pot
	msgmerge -U $@ $<
	touch $@

$(MOFILES): _build/i18n/%/LC_MESSAGES/tutorial.mo: src/tutorial/tutorial.%.po
	mkdir -p $(dir $@)
	msgfmt $< -o $@
	rm -rf _build/doctrees-$*

web:
	mkdir -p _build/web _build/doctrees
	$(SPHINXBUILD) -b web $(ALLSPHINXOPTS) _build/web
	@echo
	@echo "Build finished; now you can run"
	@echo "  python -m sphinx.web _build/web"
	@echo "to start the server."

htmlhelp:
	mkdir -p _build/htmlhelp _build/doctrees
	$(SPHINXBUILD) -b htmlhelp $(ALLSPHINXOPTS) _build/htmlhelp
	@echo
	@echo "Build finished; now you can run HTML Help Workshop with the" \
	      ".hhp project file in _build/htmlhelp."

latex: $(PDFFILES)
	mkdir -p _build/latex _build/doctrees
	$(SPHINXBUILD) -b latex $(ALLSPHINXOPTSlatex) _build/latex
	sed -i "s/pdflatex/xelatex/g" _build/latex/Makefile
	@echo
	@echo "Build finished; the LaTeX files are in _build/latex."
	@echo "Run \`make all' in that directory to run these through xelatex."

.svg.pdf:
	inkscape --file=$< --export-area-drawing --without-gui --export-pdf=$@

changes:
	mkdir -p _build/changes _build/doctrees
	$(SPHINXBUILD) -b changes $(ALLSPHINXOPTS) _build/changes
	@echo
	@echo "The overview file is in _build/changes."

linkcheck:
	mkdir -p _build/linkcheck _build/doctrees
	$(SPHINXBUILD) -b linkcheck $(ALLSPHINXOPTS) _build/linkcheck
	@echo
	@echo "Link check complete; look for any errors in the above output " \
	      "or in _build/linkcheck/output.txt."

cheatsheet: _build/cheatsheet/cheatsheet.pdf

_build/cheatsheet/cheatsheet.pdf: cheatsheet/cheatsheet.tex
	mkdir -p _build/cheatsheet
	pdflatex -output-directory=_build/cheatsheet cheatsheet/cheatsheet.tex
	pdflatex -output-directory=_build/cheatsheet cheatsheet/cheatsheet.tex

texinfo:
	mkdir -p _build/texinfo
	$(SPHINXBUILD) -b texinfo $(ALLSPHINXOPTS) _build/texinfo
	@echo
	@echo "Build finished. The Texinfo files are in _build/texinfo."
	@echo "Run \`make' in that directory to run these through makeinfo" \
	      "(use \`make info' here to do that automatically)."

info:
	mkdir -p _build/texinfo
	$(SPHINXBUILD) -b texinfo $(ALLSPHINXOPTS) _build/texinfo
	@echo "Running Texinfo files through makeinfo..."
	make -C _build/texinfo info
	@echo "makeinfo finished; the Info files are in _build/texinfo."
