# $Id$
# public domain
# Makefile[.in] for TeX for the Impatient.

PACKAGE_NAME = teximpatient
PACKAGE_TARNAME = teximpatient
PACKAGE_VERSION = 2.4
PACKAGE_STRING = teximpatient 2.4
PACKAGE_BUGREPORT = impatient@tug.org
PACKAGE_URL = 

prefix = /usr/local
datarootdir = ${prefix}/share
docdir = ${datarootdir}/doc/${PACKAGE_TARNAME}
pdfdir = ${docdir}
dvidir = ${docdir}
htmldir = ${docdir}
DESTDIR = 

SHELL = /bin/sh
ICON = 
TEX = /usr/bin/tex
PDFTEX = /usr/bin/pdftex
HTTEX = /usr/bin/httex

MKDIR_P = /usr/bin/mkdir -p
INSTALL = /usr/bin/install -c
INSTALL_DATA = ${INSTALL} -m 644

TEXFILES = backm.tex book.tex capsule.tex concepts.tex config.tex	\
  copyrght.tex eplain.tex errors.tex examples.tex fdl.tex fonts.tex	\
  frontm.tex genops.tex index.tex macros.tex math.tex modes.tex		\
  pages.tex paras.tex preface.tex read1st.tex tips.tex usebook.tex	\
  usermacs.tex usingtex.tex xmptext.tex

ICONSCRIPTS = index1.icn index2.icn

SDXFILE = book.sdx

AUXFILES = book.aux book.ccs book.idx book.toc

LOGFILES = book.log

PDFFILE = book.pdf

DVIFILE = book.dvi

HTMLFILE = book.html

all: pdf #html dvi

dvi: $(DVIFILE)

pdf: $(PDFFILE)

html: $(HTMLFILE)

install: installdirs install-dvi install-pdf

installdirs:
	$(MKDIR_P) $(DESTDIR)$(dvidir)
	$(MKDIR_P) $(DESTDIR)$(pdfdir)
	$(MKDIR_P) $(DESTDIR)$(htmldir)

install-dvi: dvi
	$(INSTALL_DATA) book.dvi $(DESTDIR)$(dvidir)/teximpatient.dvi

install-pdf: pdf
	$(INSTALL_DATA) book.pdf $(DESTDIR)$(dvidir)/teximpatient.pdf

install-html: html
	$(INSTALL_DATA) book.html $(DESTDIR)$(dvidir)/teximpatient.html

book.dvi: $(TEXFILES)
	$(TEX) '\nonstopmode\input book'

book.pdf: $(TEXFILES) $(SDXFILE)
	$(PDFTEX) '\nonstopmode\input book'

book.html: book.tex
	$(HTTEX) book.tex "html"

book.idx: book.dvi

# + chars in book.idx need to be changed to SPC chars in book.sdx
# somewhere, but currently aren't.  Therefore, we effectively disable
# the rule as long as book.sdx exists.  This also avoids rerunning every
# time (since book.idx changes every time; that file is written by TeX).
# But the page numbers in the existing book.sdx are probably wrong by now :(.
book.sdx:  #book.idx $(ICONSCRIPTS)
	if test -z "$(ICON)"; then touch book.sdx; else \
	  (cat book.idx | $(ICON) index1.icn | sort \
           | $(ICON) index2.icn >book.sdx); fi

dist: all
	tar czf $(PACKAGE_TARNAME)-$(PACKAGE_VERSION).tar.gz \
	  Makefile.in configure configure.ac \
	  NEWS README errata.future \
	  $(ICONSCRIPTS) \
	  $(TEXFILES) \
	  $(SDXFILE) \
	  $(PDFFILE)

dist-cn:
	tar czf $(PACKAGE_TARNAME)-$(PACKAGE_VERSION)cn.tar.gz \
	  cn-git/Makefile cn-git/*.tex cn-git/cnbook.pdf

distclean: clean
	-rm -rf config.log config.status autom4te.cache Makefile

clean:
	-rm -f $(LOGFILES) $(AUXFILES) x.tex 

.PHONY: all dist clean pdf dvi html install installdirs \
        install-dvi install-pdf install-html

configure: configure.ac
	autoconf
