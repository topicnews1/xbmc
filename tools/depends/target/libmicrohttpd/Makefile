include ../../Makefile.include LIBMICROHTTPD-VERSION ../../download-files.include
DEPS = ../../Makefile.include Makefile LIBMICROHTTPD-VERSION ../../download-files.include \
                                    001-remove-assert.patch

# configuration settings
CONFIGURE=cp -f $(CONFIG_SUB) $(CONFIG_GUESS) .; \
          ./configure --prefix=$(PREFIX) \
                      --disable-shared \
                      --disable-doc \
                      --disable-examples \
                      --disable-curl \
                      --enable-https

ifeq ($(DEBUG_BUILD), yes)
  CONFIGURE+= --enable-asserts
endif

LIBDYLIB=$(PLATFORM)/src/microhttpd/.libs/$(LIBNAME).a

all: .installed-$(PLATFORM)

$(PLATFORM): $(TARBALLS_LOCATION)/$(ARCHIVE).$(HASH_TYPE) $(DEPS)
	rm -rf $(PLATFORM)/*; mkdir -p $(PLATFORM)
	cd $(PLATFORM); $(ARCHIVE_TOOL) $(ARCHIVE_TOOL_FLAGS) $(TARBALLS_LOCATION)/$(ARCHIVE)
	cd $(PLATFORM); patch -p1 -i ../001-remove-assert.patch
	cd $(PLATFORM); $(CONFIGURE)

$(LIBDYLIB): $(PLATFORM)
	$(MAKE) -C $(PLATFORM)

.installed-$(PLATFORM): $(LIBDYLIB)
	$(MAKE) -C $(PLATFORM) install
	touch $@

clean:
	$(MAKE) -C $(PLATFORM) clean
	rm -f .installed-$(PLATFORM)

distclean::
	rm -rf $(PLATFORM) .installed-$(PLATFORM)
