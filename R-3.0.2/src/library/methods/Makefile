#
# ${R_HOME}/src/library/methods/Makefile


srcdir = .
top_srcdir = ../../..

top_builddir = ../../..
subdir = src/library/methods

include $(top_builddir)/Makeconf
distdir = $(top_builddir)/$(PACKAGE)-$(VERSION)/$(subdir)
DISTFILES = DESCRIPTION.in Makefile.in Makefile.win NAMESPACE

pkg = methods

RSRC = `LC_COLLATE=C ls $(srcdir)/R/*.R`

R_EXE = $(top_builddir)/bin/R --vanilla --slave
EXTRAS =  R_COMPILE_PKGS=1 R_COMPILER_SUPPRESS_ALL=1

all: Makefile DESCRIPTION
	@$(ECHO) "building package '$(pkg)'"
	@$(MKINSTALLDIRS) $(top_builddir)/library/$(pkg)
	@$(MAKE) mkR2 mkdesc
	@$(MAKE) mksrc
	@if test -n "$(R_NO_BASE_COMPILE)"; then \
	  $(MAKE) RfilesLazy; \
	else \
	  $(MAKE) RfilesLazy EXTRAS2=$(EXTRAS); \
	fi

mkR: mkR2

include $(top_srcdir)/share/make/basepkg.mk

RfilesLazy: $(top_builddir)/library/$(pkg)/R/$(pkg).rdb

## loadNamespace() depends on the NAMESPACE file.  We remove the
## dumped copy (it is rebuilt later), and we have already installed
## NAMESPACE in the library tree.
## This depends on many other things, including the list of primitives.
## Depending on names.c at least catches the latter.
$(top_builddir)/library/$(pkg)/R/$(pkg).rdb: all.R  $(srcdir)/NAMESPACE \
  $(top_srcdir)/src/main/names.c
	@rm -f $(top_builddir)/library/$(pkg)/Meta/nsInfo.rds $@
	@$(INSTALL_DATA) all.R $(top_builddir)/library/$(pkg)/R/$(pkg)
	@if test -n "$(EXTRAS2)"; then \
	  $(ECHO) "byte-compiling '$(pkg)'"; \
	fi
	@$(ECHO) "invisible(loadNamespace(\"$(pkg)\"))" | \
	  $(EXTRAS2) R_DEFAULT_PACKAGES=NULL LC_ALL=C $(R_EXE)
	@$(INSTALL_DATA) $(top_srcdir)/share/R/nspackloader.R \
	  $(top_builddir)/library/$(pkg)/R/$(pkg)

## not supported
Rsimple:

Rlazy: mkR
	@rm -f $(top_builddir)/library/$(pkg)/R/$(pkg).rdb
	@$(MAKE) RfilesLazy

Rlazycomp: mkR
	@rm -f $(top_builddir)/library/$(pkg)/R/$(pkg).rdb
	@$(MAKE) RfilesLazy EXTRAS2=$(EXTRAS)
