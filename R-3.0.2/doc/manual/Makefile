#
# ${R_HOME}/doc/manual/Makefile


srcdir = .
top_srcdir = ../..

top_builddir = ../..
subdir = doc/manual

include $(top_builddir)/Makeconf

SORT = sort # to match Windows

distdir = $(top_builddir)/$(PACKAGE)-$(VERSION)/$(subdir)
SOURCES_TEXI = R-FAQ.texi R-admin.texi R-data.texi R-exts.texi \
  R-intro.texi R-ints.texi R-lang.texi
DISTFILES = \
	README \
	Makefile.in Makefile.win \
	R-defs.texi dir epsf.tex pdfcolor.tex \
	$(SOURCES_TEXI) rw-FAQ.texi \
	resources.texi \
	R-exts.R R-exts.c R-intro.R \
	refman.top refman.bot \
	ISBN Rman.css Rfaq.css quot.sed
SUBDIRS_WITH_NO_BUILD = images

OBJECTS_HTML = $(SOURCES_TEXI:.texi=.html)
OBJECTS_INFO = $(SOURCES_TEXI:.texi=.info)
OBJECTS_PDF = $(SOURCES_TEXI:.texi=.pdf)

MAKEINDEX = /usr/bin/makeindex
MAKEINFO = 
MAKEINFO_HTML_OPTS = --html --no-split --css-include=$(srcdir)/Rman.css
MAKEINFO_TEXT_OPTS = --number-sections --fill-column=76 --no-split --no-headers
TEXI2HTML = $(MAKEINFO) $(MAKEINFO_HTML_OPTS)
TEXI2TEXT = $(MAKEINFO) $(MAKEINFO_TEXT_OPTS)
## some broken versions of texi2dvi need C collation.
TEXI2DVI0 = 
TEXI2DVI = LC_COLLATE=C  --texinfo="@set UseExternalXrefs "
TEXI2PDF = $(TEXI2DVI) --pdf
PDFLATEX = /usr/bin/pdflatex
PDFTEX = /usr/bin/pdftex

BATCHPDFLATEX = $(PDFLATEX) -interaction=nonstopmode

datarootdir = ${prefix}/share
infodir = ${datarootdir}/info
INSTALL_INFO = 

R_PAPERSIZE = a4
R_RD4PDF = times,inconsolata,hyper

texinputs_BASE = $(R_PKGS_BASE:=-pkg.tex)
texinputs_RECOMMENDED = $(R_PKGS_RECOMMENDED:=-pkg.tex)
refman_DEPENDENCIES = version.tex $(top_srcdir)/share/texmf/tex/latex/Rd.sty
fullrefman_TEXINPUTS = $(texinputs_BASE)  $(texinputs_RECOMMENDED)
SVN_REV = $(top_builddir)/SVN-REVISION

texiincludes = version.texi $(srcdir)/R-defs.texi

.SUFFIXES:
.SUFFIXES: .html .info .texi .pdf

all: Makefile R docs

Makefile: $(srcdir)/Makefile.in $(top_builddir)/config.status
	@cd $(top_builddir) && $(SHELL) ./config.status $(subdir)/$@

R: Makefile

docs: html

.texi.html:
	@if test -z "$(MAKEINFO)"; then \
	  $(ECHO) "'makeinfo' v4.7 or later needed to make HTML docs but missing on your system." ; \
	  $(ECHO) "file $@ will be missing"; \
	else \
	  $(ECHO) "creating doc/manual/$@"; \
	  $(TEXI2HTML) -D UseExternalXrefs -I$(srcdir) $< -o $@.tmp || touch $@; \
	  $(SED) -f $(srcdir)/quot.sed $@.tmp > $@; \
	  rm -f $@.tmp; \
	fi

.texi.info:
	@if test -z "$(MAKEINFO)"; then \
	  $(ECHO) "ERROR: 'makeinfo' v4.7 or later needed but missing on your system."; \
	  exit 1; \
	fi
	$(MAKEINFO) --enable-encoding -D UseExternalXrefs -I$(srcdir) $<

.texi.pdf:
	@if test -z "$(PDFTEX)"; then \
	  $(ECHO) "ERROR: 'pdftex' needed but missing on your system."; \
	  exit 1; \
	fi
	@if test -z "$(TEXI2DVI0)"; then \
	  $(ECHO) "ERROR: 'texi2dvi' needed but missing on your system."; \
	  exit 1; \
	fi
	TEXINPUTS="$(srcdir):$$TEXINPUTS" $(TEXI2PDF) $<

R-admin.html: $(srcdir)/R-admin.texi
	@if test -z "$(MAKEINFO)"; then \
	  $(ECHO) "'makeinfo' v4.7 or later needed to make HTML docs but missing on your system." ; \
	  $(ECHO) "file $@ will be missing"; \
	else \
	  $(ECHO) "creating doc/manual/$@"; \
	  $(TEXI2HTML) -I$(srcdir) $(srcdir)/R-admin.texi -o $@.tmp || touch $@;\
	  $(SED) -f $(srcdir)/quot.sed $@.tmp > $@; \
	  rm -f $@.tmp; \
	fi

R-FAQ.html: $(srcdir)/R-FAQ.texi
	@if test -z "$(MAKEINFO)"; then \
	  $(ECHO) "'makeinfo' v4.7 or later needed to make HTML docs but missing on your system." ; \
	  $(ECHO) "file $@ will be missing"; \
	else \
	  $(ECHO) "creating doc/manual/$@"; \
	  $(MAKEINFO) --html --no-split --css-include=$(srcdir)/Rfaq.css -D UseExternalXrefs -I$(srcdir) $< -o $@.tmp || touch $@; \
	  $(SED) -f $(srcdir)/quot.sed $@.tmp > $@; \
	  rm -f $@.tmp; \
	fi


version.tex: Makefile $(top_srcdir)/VERSION $(SVN_REV)
	@$(ECHO) "creating $(subdir)/$@"
	@(v=`cat $(top_srcdir)/VERSION`; \
	  v="$${v} (`sed -e 1d -e 's/^Last Changed Date: //' $(SVN_REV)`)"; \
	  $(ECHO) "$${v}") > $@

R_EXE = $(top_builddir)/bin/R --slave --vanilla

$(texinputs_BASE): FORCE
	@(pkg=`basename $@ -pkg.tex`; \
	if $(R_EXE) -e \
	  "tools:::.Rdnewer(\"$(top_srcdir)/src/library/$${pkg}\", \"$@\")"; then \
	  $(ECHO) "collecting LaTeX docs for package '$${pkg}' ..."; \
	  $(ECHO) "tools:::.pkg2tex(\"$(top_builddir)/library/$${pkg}\")" \
	    | LC_ALL=C $(R_EXE) ; \
	fi)
FORCE:

$(texinputs_RECOMMENDED): FORCE
	@(pkg=`basename $@ -pkg.tex`; \
	if find $@ -newer $(top_builddir)/library/$${pkg}/DESCRIPTION \
	    -print 2> /dev/null | grep $@ > /dev/null; then :; else \
	  $(ECHO) "collecting LaTeX docs for package '$${pkg}' ..."; \
	  $(ECHO) "tools:::.pkg2tex(\"$(top_builddir)/library/$${pkg}\")" \
	    | LC_ALL=C $(R_EXE) ; \
	fi)

version.texi: Makefile $(top_srcdir)/VERSION $(SVN_REV)
	@$(ECHO) "creating $(subdir)/$@"
	@(vv=`sed 's/ *(.*//' $(top_srcdir)/VERSION`;v=`echo $${vv}| sed 's/\([^ ]*\).*/\1/'`; \
	  $(ECHO) "@set VERSIONno $${v}" > $@; \
	  v="$${vv} (`sed -e 1d -e 's/^Last Changed Date: //' $(SVN_REV)`)"; \
	  $(ECHO) "@set VERSION $${v}" >> $@; \
	  rwv=`$(R_EXE) -f $(top_srcdir)/src/gnuwin32/fixed/rwver.R`; \
	  $(ECHO) "@set RWVERSION $${rwv}" >> $@ )
	@if test "$(R_PAPERSIZE)" = "a4"; then \
	  $(ECHO) "@afourpaper" >> $@ ; \
	fi

html: $(OBJECTS_HTML)
	@(cd ../html; $(MAKE) index.html)

R-admin.html: $(texiincludes) $(srcdir)/Rman.css
R-data.html: $(texiincludes) $(srcdir)/Rman.css
R-exts.html: $(texiincludes) $(srcdir)/Rman.css
R-intro.html: $(texiincludes) $(srcdir)/Rman.css
R-ints.html: $(texiincludes) $(srcdir)/Rman.css
R-lang.html: $(texiincludes) $(srcdir)/Rman.css
R-FAQ.html: $(srcdir)/Rfaq.css

info: $(OBJECTS_INFO)
R-admin.info: $(texiincludes)
R-data.info: $(texiincludes)
R-exts.info: $(texiincludes)
R-intro.info: $(texiincludes)
R-ints.info: $(texiincludes)
R-lang.info: $(texiincludes)

pdf: fullrefman.pdf $(OBJECTS_PDF)
refman.pdf: $(refman_DEPENDENCIES) $(texinputs_BASE)
	@if test -z "$(PDFLATEX)"; then \
	  $(ECHO) "ERROR: 'pdflatex' needed but missing on your system."; \
	  exit 1; \
	fi
	@$(ECHO) "PDF/LaTeX documentation: reference index ..."
	@rm -f *.aux refman.toc refman.ind
## <NOTE>
## We do not make sure $(ECHO) does not interpret backslash escapes.
## Could be trouble ...
	@(opt="$(R_PAPERSIZE)paper"; \
	  $(ECHO) "\\documentclass[$${opt}]{book}"; \
	  opt="$(R_RD4PDF)"; \
	  $(ECHO) "\\usepackage[$${opt}]{Rd}"; \
	  $(ECHO) "\\usepackage[utf8]{inputenc}"; \
	  cat $(srcdir)/refman.top; \
	  texinputs=`(for f in $(texinputs_BASE); \
	    do $(ECHO) $${f}; done) | LC_COLLATE=C $(SORT)`; \
	  for f in $${texinputs}; do $(ECHO) "\\input{$${f}}"; done; \
	  cat $(srcdir)/refman.bot) > refman.tex
## </NOTE>
	@TEXINPUTS="$(top_srcdir)/share/texmf/tex/latex:$$TEXINPUTS" \
	  $(BATCHPDFLATEX) refman.tex
	@-$(MAKEINDEX) refman
	@TEXINPUTS="$(top_srcdir)/share/texmf/tex/latex:$$TEXINPUTS" \
	  $(BATCHPDFLATEX) refman.tex
	@TEXINPUTS="$(top_srcdir)/share/texmf/tex/latex:$$TEXINPUTS" \
	  $(BATCHPDFLATEX) refman.tex
fullrefman.pdf: $(refman_DEPENDENCIES) $(fullrefman_TEXINPUTS)
	@if test -z "$(PDFLATEX)"; then \
	  $(ECHO) "ERROR: 'pdflatex' needed but missing on your system."; \
	  exit 1; \
	fi
	@$(ECHO) "PDF/LaTeX documentation: full reference index ..."
	@rm -f *.aux fullrefman.toc fullrefman.ind
	@(opt="$(R_PAPERSIZE)paper"; \
	  $(ECHO) "\\documentclass[$${opt}]{book}"; \
	  opt="$(R_RD4PDF)"; \
	  $(ECHO) "\\usepackage[$${opt}]{Rd}"; \
	  $(ECHO) "\\usepackage[utf8]{inputenc}"; \
	  cat $(srcdir)/refman.top; \
	  if test -z ""; then \
	    $(ECHO) "\\part{}"; \
	    texinputs=`(for f in $(texinputs_BASE); \
	      do $(ECHO) $${f}; done) | LC_COLLATE=C $(SORT)`; \
	    for f in $${texinputs}; do $(ECHO) "\\input{$${f}}"; done; \
	    $(ECHO) "\\part{}"; \
	    texinputs=`(for f in $(texinputs_RECOMMENDED); \
	      do $(ECHO) $${f}; done) | LC_COLLATE=C $(SORT)`; \
	    for f in $${texinputs}; do $(ECHO) "\\input{$${f}}"; done; \
	  else \
	    texinputs=`(for f in $(texinputs_BASE); \
	      do $(ECHO) $${f}; done) | LC_COLLATE=C $(SORT)`; \
	    for f in $${texinputs}; do $(ECHO) "\\input{$${f}}"; done; \
	  fi; \
	  cat $(srcdir)/refman.bot) > fullrefman.tex
	@TEXINPUTS="$(top_srcdir)/share/texmf/tex/latex:$$TEXINPUTS" \
	  $(BATCHPDFLATEX) fullrefman.tex
	@-$(MAKEINDEX) fullrefman
	@TEXINPUTS="$(top_srcdir)/share/texmf/tex/latex:$$TEXINPUTS" \
	  $(BATCHPDFLATEX) fullrefman.tex
	@TEXINPUTS="$(top_srcdir)/share/texmf/tex/latex:$$TEXINPUTS" \
	  $(BATCHPDFLATEX) fullrefman.tex

R-admin.pdf: $(texiincludes)
R-data.pdf: $(texiincludes)
R-exts.pdf: $(texiincludes)
R-intro.pdf: stamp-images-pdf $(texiincludes)
R-ints.pdf: $(texiincludes)
stamp-images-pdf:
	@if test "$(BUILDDIR_IS_SRCDIR)" = no ; then \
	  $(MAKE) rdocdir=$(top_builddir)/doc install-images-pdf; \
	fi
	@touch $@
R-lang.pdf: $(texiincludes)

install: install-message installdirs install-html
install-message:
	@$(ECHO) "installing $(subdir) ..."
installdirs:
	@$(MKINSTALLDIRS) "$(DESTDIR)$(rdocdir)/manual"
install-strip: install
install-images-pdf: installdirs
	@$(MKINSTALLDIRS) "$(DESTDIR)$(rdocdir)/manual/images"
	@-for f in $(srcdir)/images/*.pdf; do \
	  $(INSTALL_DATA) $${f} "$(DESTDIR)$(rdocdir)/manual/images"; \
	done
## if there is no makeinfo >= 4.7 then there are no html pages
install-html: installdirs
	@for f in $(OBJECTS_HTML); do \
	  if test -f $${f} ; then \
	    $(INSTALL_DATA) $${f} "$(DESTDIR)$(rdocdir)/manual"; \
	  fi \
	done
install-info: info $(DESTDIR)$(infodir)/dir
	@$(ECHO) "installing R info pages ..."
	@for f in $(OBJECTS_INFO:.info=.info*); do \
	  $(INSTALL_DATA) $${f} "$(DESTDIR)$(infodir)"; \
	  $(R_GZIPCMD) -f "$(DESTDIR)$(infodir)/$${f}"; \
	done
	@$(ECHO) "updating '$(infodir)/dir' ..."
	@for f in $(OBJECTS_INFO); do \
	  $(INSTALL_INFO) --info-dir=$(DESTDIR)$(infodir) $(DESTDIR)$(infodir)/$${f} \
	    || $(ECHO) "you need to adjust $(infodir)/dir manually"; \
	done
install-pdf: pdf installdirs
	@$(ECHO) "installing R manuals in PDF format ..."
	@for f in *.pdf; do \
	  $(INSTALL_DATA) $${f} "$(DESTDIR)$(rdocdir)/manual"; \
	done

$(DESTDIR)$(infodir)/dir:
	@$(MKINSTALLDIRS) "$(DESTDIR)$(infodir)"
	$(INSTALL_DATA) "$(srcdir)/dir" "$(DESTDIR)$(infodir)"

uninstall:
	@if test -f "$(DESTDIR)$(infodir)/R-data.info" \
	     || test -f "$(DESTDIR)$(infodir)/R-data.info.gz"; then \
	  $(MAKE) uninstall-info; \
	fi
	@$(ECHO) "uninstalling $(subdir) ..."
	@rm -rf "$(DESTDIR)$(rdocdir)/manual"
uninstall-info:
	@$(ECHO) "uninstalling R info pages ..."
	@for f in $(OBJECTS_INFO); do \
	  $(INSTALL_INFO) --remove --info-dir=$(DESTDIR)$(infodir) $${f} \
	    || $(ECHO) "you need to adjust $(infodir)/dir manually"; \
	done
	@for f in $(OBJECTS_INFO:.info=.info*); do \
	  rm -f "$(DESTDIR)$(infodir)/$${f}" "$(DESTDIR)$(infodir)/$${f}.gz" ; \
	done
uninstall-pdf:
	@$(ECHO) "uninstalling R manuals in PDF format ..."
	@-rm -f "$(DESTDIR)$(rdocdir)/manual"/*.pdf

mostlyclean: clean
clean:
	@if test "$(BUILDDIR_IS_SRCDIR)" = no ; then \
	  rm -rf $(INSTFILES) images; \
	fi
	-@rm -f *.aux *.cp *.cps *.dvi *.en *.ens *.fn *.fns *.html *.info* \
	  *.ky *.kys *.log *.out *.pdf *.pg *.pgs *.ps *.ps-2 *.tmp \
	  *.toc *.tp *.vr *.vrs \
	  *-pkg.tex refman.i?? refman.tex fullrefman.i?? fullrefman.tex \
	  version.tex version.texi
	-@rm -f stamp-*
distclean: clean
	-@rm -f Makefile
maintainer-clean: distclean

distdir: $(DISTFILES)
	@for f in $(DISTFILES); do \
	  test -f $(distdir)/$${f} \
	    || ln $(srcdir)/$${f} $(distdir)/$${f} 2>/dev/null \
	    || cp -p $(srcdir)/$${f} $(distdir)/$${f}; \
	done
	@for d in $(SUBDIRS_WITH_NO_BUILD); do \
	  ((cd $(srcdir); $(TAR) -c -f - $(DISTDIR_TAR_EXCLUDE) $${d}) \
	      | (cd $(distdir); $(TAR) -x -f -)) \
	    || exit 1; \
	done
	@if test -z "$(MAKEINFO)"; then \
	  $(ECHO) "ERROR: 'makeinfo' v4.7 or later needed but missing on your system."; \
	  exit 1; \
	fi
#	$(distdir)/doc/html has already been created
	@$(TEXI2HTML) --no-headers --number-sections \
	  $(srcdir)/R-admin.texi -I$(srcdir) \
	  -o $(distdir)/../html/R-admin.html
#	front-matter files
#	@$(TEXI2TEXT) $(srcdir)/resources.texi -o $(distdir)/../../RESOURCES
#	@$(TEXI2TEXT) $(srcdir)/R-FAQ.texi -o $(distdir)/../../FAQ
#	@$(TEXI2HTML) --no-headers $(srcdir)/resources.texi \
#	  -o $(distdir)/../html/resources.html

front-matter: ${top_builddir}/doc/RESOURCES ${top_builddir}/doc/FAQ

${top_builddir}/doc/RESOURCES: $(srcdir)/resources.texi
	@$(ECHO) "creating RESOURCES"
	@$(TEXI2TEXT) $(srcdir)/resources.texi -o $@

${top_builddir}/doc/FAQ: $(srcdir)/R-FAQ.texi
	@$(ECHO) "creating FAQ"
	@$(TEXI2TEXT) $(srcdir)/R-FAQ.texi -o $@

html-non-svn: ../html/resources.html

../html/resources.html: $(srcdir)/resources.texi
	@$(ECHO) "creating doc/html/resources.html"
	@$(TEXI2HTML) --no-headers $(srcdir)/resources.texi -o $@
