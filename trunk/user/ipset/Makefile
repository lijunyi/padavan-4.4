SRC_NAME=ipset-7.23

THISDIR = $(shell pwd)

all: extract_test config_test
	$(MAKE) -C $(SRC_NAME)
	$(MAKE) modules -C $(SRC_NAME)

extract_test:
	( if [ ! -d $(SRC_NAME) ]; then \
		tar xjf $(SRC_NAME).tar.bz2; \
		if [ -f fix-configure-strscpy.patch ]; then \
			patch -d $(SRC_NAME) -p1 < fix-configure-strscpy.patch; \
		fi; \
	fi )

config_test:
	( if [ -f ./config_done ]; then \
		echo "the same configuration"; \
	else \
		make configure && touch config_done; \
	fi )

configure:
	( cd $(SRC_NAME) ; \
	./autogen.sh ; \
	PKG_CONFIG_PATH=$(STAGEDIR)/lib/pkgconfig \
	./configure \
		--prefix= \
		--enable-static \
		--disable-shared \
		libmnl_CFLAGS="-I$(STAGEDIR)/include" \
		libmnl_LIBS="-L$(STAGEDIR)/lib -lmnl" \
		--with-kmod=yes \
		--with-kbuild=$(ROOTDIR)/$(LINUXDIR) \
		--host=$(HOST_TARGET) \
		--build=$(HOST_BUILD) ; \
	)

clean:
	if [ -f $(SRC_NAME)/Makefile ] ; then \
		$(MAKE) -C $(SRC_NAME) modules_clean ; \
		$(MAKE) -C $(SRC_NAME) distclean ; \
	fi ; \
	rm -f config_done

romfs:
	$(ROMFSINST) $(THISDIR)/$(SRC_NAME)/src/ipset /sbin/ipset
