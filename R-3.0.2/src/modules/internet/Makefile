#
# ${R_HOME}/src/modules/internet/Makefile


srcdir = .
top_srcdir = ../../..

top_builddir = ../../..
subdir = src/modules/internet
R_HOME = $(top_builddir)

include $(top_builddir)/Makeconf

SOURCES = Rhttpd.c Rsock.c internet.c nanoftp.c nanohttp.c sock.c sockconn.c
HEADERS = sock.h
DEPENDS = $(SOURCES:.c=.d)
OBJECTS = $(SOURCES:.c=.o)

distdir = $(top_builddir)/$(PACKAGE)-$(VERSION)/$(subdir)
DISTFILES = \
	README Makefile.in Makefile.win $(SOURCES) $(HEADERS) \
	dllversion.rc

internet_la = internet$(SHLIB_EXT)
internet_la_LIBADD = $(LIBR) # $(LIBINTL)

ALL_CFLAGS = $(ALL_CFLAGS_LO) -fvisibility=hidden

all: Makefile Makedeps R

Makefile: $(srcdir)/Makefile.in \
  $(top_builddir)/config.status \
  $(SOURCES)
	@cd $(top_builddir) && $(SHELL) ./config.status $(subdir)/$@

Makedeps: Makefile $(DEPENDS)
	@cat $(DEPENDS) >> Makefile
	@touch $@

R: Makefile
	@$(MAKE) Makedeps
	@$(MAKE) $(internet_la)

$(internet_la): $(OBJECTS)
	$(SHLIB_LINK) -o $@ $(OBJECTS) $(internet_la_LIBADD)
## <FIXME>
## Not sure if we want to do this ...
	@$(MAKE) rhome="$(abs_top_builddir)" install
## </FIXME>

install: installdirs
	@$(SHELL) $(top_srcdir)/tools/copy-if-change $(internet_la) "$(DESTDIR)$(Rexecmodulesdir)/$(internet_la)"
installdirs:
	@$(MKINSTALLDIRS) "$(DESTDIR)$(Rexecmodulesdir)"
install-strip: installdirs
	@$(SHELL) $(top_srcdir)/tools/copy-if-change $(internet_la) "$(DESTDIR)$(Rexecmodulesdir)/$(internet_la)"
	@if test -n "$(STRIP_LIBS)"; then \
	  $(STRIP_LIBS) "$(DESTDIR)$(Rexecmodulesdir)/$(internet_la)" ;\
	fi
uninstall:
	@rm -f "$(Rexecmodulesdir)/$(internet_la)"

mostlyclean: clean
clean:
	@-rm -rf .libs _libs
	@-rm -f Makedeps *.d *.o *.a *.lo *.la *$(SHLIB_EXT)
distclean: clean
	@-rm -f Makefile
maintainer-clean: distclean

TAGS info dvi check:

distdir: $(DISTFILES)
	@for f in $(DISTFILES); do \
	  test -f $(distdir)/$${f} \
	    || ln $(srcdir)/$${f} $(distdir)/$${f} 2>/dev/null \
	    || cp -p $(srcdir)/$${f} $(distdir)/$${f}; \
	done

## Automagically generated dependencies:

Rhttpd.o: Rhttpd.c ../../../src/include/config.h \
 ../../../src/include/Defn.h ../../../src/include/R_ext/Complex.h \
 ../../../src/include/Rinternals.h ../../../src/include/R_ext/Arith.h \
 ../../../src/include/R_ext/libextern.h \
 ../../../src/include/R_ext/Boolean.h ../../../src/include/R_ext/Error.h \
 ../../../src/include/R_ext/Memory.h ../../../src/include/R_ext/Utils.h \
 ../../../src/include/R_ext/Print.h ../../../src/include/Rinlinedfuns.h \
 ../../../src/include/Errormsg.h ../../../src/include/Fileio.h \
 ../../../src/include/Rconnections.h \
 ../../../src/include/R_ext/Connections.h \
 ../../../src/include/Rmodules/Rinternet.h \
 ../../../src/include/R_ext/eventloop.h
Rsock.o: Rsock.c ../../../src/include/config.h sock.h \
 ../../../src/include/R_ext/Print.h \
 ../../../src/include/R_ext/R-ftp-http.h \
 ../../../src/include/R_ext/eventloop.h
internet.o: internet.c ../../../src/include/config.h \
 ../../../src/include/Defn.h ../../../src/include/R_ext/Complex.h \
 ../../../src/include/Rinternals.h ../../../src/include/R_ext/Arith.h \
 ../../../src/include/R_ext/libextern.h \
 ../../../src/include/R_ext/Boolean.h ../../../src/include/R_ext/Error.h \
 ../../../src/include/R_ext/Memory.h ../../../src/include/R_ext/Utils.h \
 ../../../src/include/R_ext/Print.h ../../../src/include/Rinlinedfuns.h \
 ../../../src/include/Errormsg.h ../../../src/include/Fileio.h \
 ../../../src/include/Rconnections.h \
 ../../../src/include/R_ext/Connections.h \
 ../../../src/include/R_ext/R-ftp-http.h \
 ../../../src/include/Rmodules/Rinternet.h sock.h \
 ../../../src/include/R_ext/RS.h ../../../src/include/Rconfig.h \
 ../../../src/include/R_ext/Rdynload.h
nanoftp.o: nanoftp.c ../../../src/include/config.h \
 ../../../src/include/R_ext/R-ftp-http.h \
 ../../../src/include/R_ext/eventloop.h
nanohttp.o: nanohttp.c ../../../src/include/config.h \
 ../../../src/include/R_ext/R-ftp-http.h \
 ../../../src/include/R_ext/eventloop.h
sock.o: sock.c ../../../src/include/config.h \
 ../../../src/include/R_ext/Error.h sock.h
sockconn.o: sockconn.c ../../../src/include/config.h \
 ../../../src/include/Defn.h ../../../src/include/R_ext/Complex.h \
 ../../../src/include/Rinternals.h ../../../src/include/R_ext/Arith.h \
 ../../../src/include/R_ext/libextern.h \
 ../../../src/include/R_ext/Boolean.h ../../../src/include/R_ext/Error.h \
 ../../../src/include/R_ext/Memory.h ../../../src/include/R_ext/Utils.h \
 ../../../src/include/R_ext/Print.h ../../../src/include/Rinlinedfuns.h \
 ../../../src/include/Errormsg.h ../../../src/include/Rconnections.h \
 ../../../src/include/R_ext/Connections.h \
 ../../../src/include/R_ext/R-ftp-http.h sock.h
