LATEX = pdflatex
MAINFILE = tut01r-riondato
UPLOADHOST = ssh.cs.brown.edu
UPLOADURI = public_html/RiondatoUpfal-VCTutorial.pdf

.PHONY: all archive check clean osx pdf upload view

all : archive

$(MAINFILE).aux: *.tex $(wildcard *.eps)
	$(LATEX) $(MAINFILE).tex

$(MAINFILE).bbl: *.tex  *.bib
	-bibtex --min-crossrefs=20 $(MAINFILE)

$(MAINFILE).pdf: $(MAINFILE).aux $(MAINFILE).bbl
	$(LATEX) $(MAINFILE).tex
	$(LATEX) $(MAINFILE).tex

$(MAINFILE).tar.bz2: $(MAINFILE).pdf
	env COPYFILE_DISABLE=1 tar cjvfh $(MAINFILE).tar.bz2 *.tex *.bib *.pdf $(wildcard figures/*pdf) $(wildcard *.bst) $(wildcard *.cls) $(wildcard *.clo) $(wildcard *.eps) $(wildcard *.svg) Makefile

archive: $(MAINFILE).tar.bz2

check: *.tex
	($(LATEX) $(MAINFILE).tex | grep -s -e "multiply" -e "undefined") || echo "all OK"

clean:
	-/bin/rm -f $(MAINFILE).pdf $(MAINFILE).tar.bz2 *.dvi *.aux *.ps *~
	-/bin/rm -f *.log *.lot *.lof *.toc *.blg *.bbl *.idx *.out

pdf: $(MAINFILE).pdf

osx: pdf
	open $(MAINFILE).pdf

upload: pdf
	rsync -e ssh --perms --chmod=F644 $(MAINFILE).pdf $(UPLOADHOST):$(UPLOADURI)

view: pdf
	acroread  -geometry 1000x1000 $(MAINFILE).pdf

