include ../../Makefile.include
DEPS= ../../Makefile.include Makefile

# lib name, version
LIBNAME=lame
VERSION=3.99.5
SOURCE=$(LIBNAME)-$(VERSION)
ARCHIVE=$(SOURCE).tar.gz

# configuration settings
CONFIGURE=cp -f $(CONFIG_SUB) $(CONFIG_GUESS) .; \
          ./configure --prefix=$(PREFIX) --disable-gtktest --disable-frontend --disable-decoder

LIBDYLIB=$(PLATFORM)/libmp3lame/.libs/libmp3lame.a

CLEAN_FILES=$(ARCHIVE) $(PLATFORM)

all: .installed-$(PLATFORM)

$(TARBALLS_LOCATION)/$(ARCHIVE):
	cd $(TARBALLS_LOCATION); $(RETRIEVE_TOOL) $(RETRIEVE_TOOL_FLAGS) $(BASE_URL)/$(ARCHIVE)

$(PLATFORM): $(TARBALLS_LOCATION)/$(ARCHIVE) $(DEPS)
	rm -rf $(PLATFORM)/*; mkdir -p $(PLATFORM)
	cd $(PLATFORM); $(ARCHIVE_TOOL) $(ARCHIVE_TOOL_FLAGS) $(TARBALLS_LOCATION)/$(ARCHIVE)
	cd $(PLATFORM); $(CONFIGURE)

$(LIBDYLIB): $(PLATFORM)
	$(MAKE) -C $(PLATFORM)/libmp3lame

.installed-$(PLATFORM): $(LIBDYLIB)
	$(MAKE) -C $(PLATFORM)/include install
	$(MAKE) -C $(PLATFORM)/libmp3lame install
ifeq ($(OS),android)
	rm -f $(PREFIX)/lib/libmp3lame.la $(PREFIX)/lib/libmp3lame.so $(PREFIX)/lib/libmp3lame.so.0
	mv -f $(PREFIX)/lib/libmp3lame.so.0.0.0 $(PREFIX)/lib/libmp3lame.so
	$(RPL) -e "libmp3lame.so.0" "libmp3lame.so\x00\x00" $(PREFIX)/lib/libmp3lame.so
	-$(READELF) --dynamic $(PREFIX)/lib/libmp3lame.so | grep ibrary
endif
	touch $@

clean:
	$(MAKE) -C $(PLATFORM) clean
	rm -f .installed-$(PLATFORM)

distclean::
	rm -rf $(PLATFORM) .installed-$(PLATFORM)
