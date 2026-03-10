SRC_NAME=curl-8.7.1

LDFLAGS += -Wl,-rpath-link=$(STAGEDIR)/lib

all: extract_test config_test
	$(MAKE) -j$(HOST_NCPU) -C $(SRC_NAME)

extract_test:
	( if [ ! -d $(SRC_NAME) ]; then \
		tar xf $(SRC_NAME).tar.xz; \
		-patch -d $(SRC_NAME) -p1 -i ../no_docs_test.patch; \
		sed -i 's/SUBDIRS = lib src tests packages scripts include docs/SUBDIRS = lib src packages include/g' $(SRC_NAME)/Makefile.in; \
		sed -i 's/DIST_SUBDIRS = $(SUBDIRS) tests packages scripts include docs/DIST_SUBDIRS = $(SUBDIRS) packages include/g' $(SRC_NAME)/Makefile.in; \
	fi )

config_test:
	( if [ -f ./config_done ]; then \
		echo "the same configuration"; \
	else \
		make configure && touch config_done; \
	fi )

configure:
	( cd $(SRC_NAME) ; \
	./configure \
		--prefix=$(STAGEDIR) \
		--enable-static \
		--enable-shared \
		$(if $(CONFIG_IPV6),--enable-ipv6,--disable-ipv6) \
		--disable-gopher \
		--disable-ldap \
		--disable-ldaps \
		--disable-rtsp \
		--disable-telnet \
		--disable-tftp \
		--disable-smtp \
		--disable-pop3 \
		--disable-imap \
		--disable-sspi \
		--disable-dict \
		--disable-ares \
		--disable-debug \
		--disable-curldebug \
		--disable-manual \
		--disable-verbose \
		--disable-tls-srp \
		--disable-libcurl-option \
		--disable-threaded-resolver \
		--without-gnutls \
		--without-ca-bundle \
		--without-libidn2 \
		--without-libpsl \
		--without-libssh2 \
		--without-krb4 \
		--without-nss \
		--with-random=/dev/urandom \
		--with-zlib=$(STAGEDIR) \
		--with-ssl=$(STAGEDIR) \
		--host=$(HOST_TARGET) \
		--build=$(HOST_BUILD) ; \
	)

clean:
	if [ -f $(SRC_NAME)/Makefile ] ; then \
		$(MAKE) -C $(SRC_NAME) clean ; \
	fi ; \
	rm -f config_done

install:
	$(MAKE) -C $(SRC_NAME) install DESTDIR=""

romfs:
ifeq ($(CONFIG_FIRMWARE_INCLUDE_CURL),y)
	$(ROMFSINST) -d $(DESTDIR)/bin/curl /usr/bin/curl
endif
	cp -fP $(DESTDIR)/lib/libcurl.so* $(ROMFSDIR)/lib
