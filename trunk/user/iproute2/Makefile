SRC_NAME=iproute2-5.15.0

THISDIR = $(shell pwd)

all: extract_test
	$(MAKE) -C $(SRC_NAME) V=1

extract_test:
	( if [ ! -d $(SRC_NAME) ]; then \
		tar xf $(SRC_NAME).tar.xz; \
	fi )

clean:
	if [ -f $(SRC_NAME)/Makefile ] ; then \
		$(MAKE) -C $(SRC_NAME) distclean ; \
	fi

romfs:
	$(ROMFSINST) $(THISDIR)/$(SRC_NAME)/ip/ip /bin/ip
