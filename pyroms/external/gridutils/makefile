#
# makefile.in - Input makefile for the configure script.
#
#

SHELL = /bin/sh

#### Start of system configuration section. ####

prefix = /u1/uaf/kate/Python
exec_prefix = ${prefix}

bindir = ${exec_prefix}/bin
libdir = /u1/uaf/kate/Python/lib
includedir = ${prefix}/include

INSTALLDIRS =\
$(bindir)\
$(libdir)\
$(includedir)

DISTDIR = ~/pub_web

INSTALL = /usr/bin/install -c
INSTALL_PROGRAM = ${INSTALL}
INSTALL_DATA = ${INSTALL} -m 644

#### End of system configuration section. ####

CC = gcc
CFLAGS = -g -O2 -Wall -pedantic
LDFLAGS = -L/u1/uaf/kate/Python/lib
CPPFLAGS = -I/u1/uaf/kate/Python/include

AR = ar
ARFLAGS = cru

MLIB = -lm
NNLIB = -lnn
CSALIB = -lcsa

SRC =\
getbound.c\
getnodes.c\
gridaverager.c\
gridbathy.c\
gridmap.c\
gridnodes.c\
gucommon.c\
insertgrid.c\
poly.c\
setbathy.c\
subgrid.c\
xy2ij.c

HEADERS =\
gridaverager.h\
gridmap.h\
gridnodes.h\
gucommon.h\
kdtree.h\
nan.h\
poly.h\
version.h

LIBOBJECTS =\
gridmap.o\
gridnodes.o\
gucommon.o\
kdtree.o\
poly.o

SHLIBOBJECTS =\
gridmap.t\
gridnodes.t\
gucommon.t\
kdtree.t\
poly.t

PROGRAMS =\
getbound\
getnodes\
insertgrid\
setbathy\
subgrid\
xy2ij\
gridbathy

%.o: %.c
	$(CC) $(CFLAGS) $(CPPFLAGS) -I. -c $*.c -o $*.o
%.t: %.c
	$(CC) $(CFLAGS) -fPIC $(CPPFLAGS) -I. -c $*.c -o $*.t

all: lib shlib $(PROGRAMS)
	@if [ ! -f libgu.so ] ; then \
	   echo "  type 'make shlib' to make libgu.so"; \
	fi

lib: libgu.a

shlib: libgu.so

libgu.a: $(LIBOBJECTS)
	$(AR) $(ARFLAGS) libgu.a $(LIBOBJECTS)
	chmod go+r libgu.a

libgu.so: $(SHLIBOBJECTS)
	rm -f $@
	$(CC) -shared -o $@ $(SHLIBOBJECTS)

standalone: override LDFLAGS+=-static
standalone: $(PROGRAMS)
	strip $(PROGRAMS)

getbound: libgu.a getbound.o
	$(CC) -o $@ getbound.o $(CFLAGS) $(LDFLAGS) libgu.a $(MLIB)

getnodes: libgu.a getnodes.o
	$(CC) -o $@ getnodes.o $(CFLAGS) $(LDFLAGS) libgu.a $(MLIB)

gridbathy: libgu.a gridaverager.o gridbathy.o
	$(CC) -o $@ gridaverager.o gridbathy.o $(CFLAGS) $(LDFLAGS) $(NNLIB) $(CSALIB) libgu.a $(MLIB)

insertgrid: libgu.a insertgrid.o
	$(CC) -o $@ insertgrid.o $(CFLAGS) $(LDFLAGS) libgu.a $(MLIB)

setbathy: libgu.a setbathy.o
	$(CC) -o $@ setbathy.o $(CFLAGS) $(LDFLAGS) libgu.a $(MLIB)

subgrid: libgu.a subgrid.o
	$(CC) -o $@ subgrid.o $(CFLAGS) $(LDFLAGS) libgu.a $(MLIB)

xy2ij: libgu.a xy2ij.o
	$(CC) -o $@ xy2ij.o $(CFLAGS) $(LDFLAGS) libgu.a $(MLIB)

installdirs:
	@$(SHELL) mkinstalldirs $(INSTALLDIRS)

install: all installdirs
	@for i in libgu.a libgu.so; do \
	   if test -f $$i; then \
	      echo "  installing $$i"; \
	      $(INSTALL_DATA) $$i $(libdir)/$$i; \
	   fi \
	done

	@for i in gridmap.h gucommon.h gridnodes.h; do \
	   echo "  installing $$i"; \
	   $(INSTALL_DATA) $$i $(includedir); \
	done

	@for i in $(PROGRAMS); do \
	   echo "  installing $$i"; \
	   fname=`basename $$i`; \
	   $(INSTALL_PROGRAM) $$i $(bindir); \
	done

clean:
	rm -f *.o *.t *.a *.so $(PROGRAMS) *~ \#*\# core
	cd test; make clean

configclean:
	rm -f gridutilsconfig.h makefile config.cache config.status config.log

distclean: clean configclean

indent:
	indent -T FILE -T gridmap -T gridnodes -T gridaverager -T extent -T poly -T subgrid -T NODETYPE -T COORDTYPE -T gridstats -T kdtree -T kdnode $(SRC) $(HEADERS)
	rm -f *~

dist:
	WDIR=`pwd`;\
	DIR=`basename $$WDIR`;\
	cp -r $$WDIR /tmp;\
	cd /tmp/$$DIR;\
	rm -rf `find /tmp/$$DIR -name CVS`;\
	if [ -e makefile ] ; then\
		make distclean;\
	fi;\
	/tmp/"$$DIR"/configure;\
	make standalone;\
	chmod a+r+x $(PROGRAMS);\
	mv -f $(PROGRAMS) $(DISTDIR);\
	make distclean;\
	cd $(DISTDIR);\
	mv -f $$DIR".tar.gz" $$DIR".tar.gz.prev";\
	gzip -f $(PROGRAMS);\
	cd /tmp;\
	tar czvf $$DIR".tar.gz" $$DIR;\
	chmod a+r $$DIR".tar.gz";\
	mv -f $$DIR".tar.gz" $(DISTDIR)/$$DIR".tar.gz";\
	rm -rf $$DIR
