SRC_NAME1=squashfs-tools-4.6.1
SRC_NAME2=xz-5.4.6

all: extract_test config_xz build_xz build_squashfs

extract_test:
	( if [ ! -d $(SRC_NAME1) ]; then \
		tar -xzf $(SRC_NAME1).tar.gz; \
		if [ -f $(SRC_NAME1).patch ]; then \
			patch -p0 < $(SRC_NAME1).patch; \
		fi; \
	fi )
	( if [ ! -d $(SRC_NAME2) ]; then \
		tar -xzf $(SRC_NAME2).tar.gz; \
	fi )

config_xz:
	( if [ -f ./config_xz_done ]; then \
		echo "the same configuration"; \
	else \
		make configure_xz && touch config_xz_done; \
	fi )

configure_xz:
	( cd $(SRC_NAME2) ; \
	./configure \
		--disable-shared \
		--disable-werror \
		--disable-xz \
		--disable-xzdec \
		--disable-lzmadec \
		--disable-lzmainfo \
		CFLAGS="-std=c99" ; \
	)

build_xz:
	make -C $(SRC_NAME2)

build_squashfs:
	make -j$(HOST_NCPU) -C $(SRC_NAME1)/squashfs-tools XZUTILSDIR=$(shell pwd)/$(SRC_NAME2) ; \
	cp $(SRC_NAME1)/squashfs-tools/mksquashfs ./
	cp $(SRC_NAME1)/squashfs-tools/unsquashfs ./

clean:
	if [ -f $(SRC_NAME2)/Makefile ] ; then \
		make -C $(SRC_NAME2) distclean ; \
	fi
	if [ -d $(SRC_NAME1)/squashfs-tools ] ; then \
		make -C $(SRC_NAME1)/squashfs-tools clean ; \
	fi
	rm -f config_xz_done
	rm -f mksquashfs unsquashfs
