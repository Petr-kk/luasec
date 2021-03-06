LMOD=ssl.lua

OBJS= \
 options.o \
 x509.o    \
 context.o \
 ssl.o     \
 config.o  \
 ec.o

ifeq ($(OS),Windows_NT)
  CMOD=ssl.dll
  LIBS = -lssl32 -leay32 -lluasocket -lws2_32 $(EXT_LIBS)
else
  CMOD=ssl.so
  LIBS = -lssl -lcrypto -lluasocket $(EXT_LIBS)
endif

WARN=-Wall -pedantic

BSD_CFLAGS=-O2 -fPIC $(WARN) $(INCDIR) $(DEFS)
BSD_LDFLAGS=-O -fPIC -shared $(LIBDIR)

LNX_CFLAGS=-O2 -fPIC $(WARN) $(INCDIR) $(DEFS)
LNX_LDFLAGS=-O -fPIC -shared $(LIBDIR)

MAC_ENV=env MACOSX_DEPLOYMENT_TARGET='$(MACVER)'
MAC_CFLAGS=-O2 -fno-common $(WARN) $(INCDIR) $(DEFS)
MAC_LDFLAGS=-bundle -undefined dynamic_lookup $(LIBDIR)

WIN_CFLAGS=-O2 -std=gnu99 $(WARN) $(INCDIR) $(DEFS)
WIN_LDFLAGS=-O -shared $(LIBDIR)

INSTALL  = install
CC      ?= cc
CCLD      ?= $(MYENV) $(CC)
CFLAGS  += $(MYCFLAGS)
LDFLAGS += $(MYLDFLAGS)

.PHONY: all clean install none linux bsd macosx luasocket

all:

install: $(CMOD) $(LMOD)
	$(INSTALL) -d $(DESTDIR)$(LUAPATH)/ssl $(DESTDIR)$(LUACPATH)
	$(INSTALL) $(CMOD) $(DESTDIR)$(LUACPATH)
	$(INSTALL) -m644 $(LMOD) $(DESTDIR)$(LUAPATH)
	$(INSTALL) -m644 https.lua $(DESTDIR)$(LUAPATH)/ssl

linux:
	@$(MAKE) $(CMOD) MYCFLAGS="$(LNX_CFLAGS)" MYLDFLAGS="$(LNX_LDFLAGS)" EXTRA="$(EXTRA)"

bsd:
	@$(MAKE) $(CMOD) MYCFLAGS="$(BSD_CFLAGS)" MYLDFLAGS="$(BSD_LDFLAGS)" EXTRA="$(EXTRA)"

macosx:
	@$(MAKE) $(CMOD) MYCFLAGS="$(MAC_CFLAGS)" MYLDFLAGS="$(MAC_LDFLAGS)" MYENV="$(MAC_ENV)" EXTRA="$(EXTRA)"

windows:
	@$(MAKE) $(CMOD) MYCFLAGS="$(WIN_CFLAGS)" MYLDFLAGS="$(WIN_LDFLAGS)" EXTRA="$(EXTRA)" CC=$(CC)

luasocket:
	@cd luasocket && $(MAKE)

$(CMOD): $(EXTRA) $(OBJS)
	$(CCLD) $(LDFLAGS) -o $@ $(OBJS) $(LIBS)

clean:
	cd luasocket && $(MAKE) clean
	rm -f $(OBJS) $(CMOD)

options.o: options.h options.c
ec.o: ec.c ec.h
x509.o: x509.c x509.h compat.h
context.o: context.c context.h ec.h compat.h options.h
ssl.o: ssl.c ssl.h context.h x509.h compat.h
config.o: config.c ec.h options.h compat.h
