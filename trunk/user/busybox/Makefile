SRC_NAME=busybox-1.36.1

all: extract_test
	$(MAKE) -j$(HOST_NCPU) -C $(SRC_NAME)

extract_test:
	( if [ ! -d $(SRC_NAME) ]; then \
		tar xjf $(SRC_NAME).tar.bz2; \
	fi )

clean:
	if [ -f $(SRC_NAME)/Makefile ] ; then \
		$(MAKE) -C $(SRC_NAME) distclean ; \
	fi

romfs:
	$(MAKE) -C $(SRC_NAME) CONFIG_PREFIX=$(ROMFSDIR) install
