SRC_NAME=dropbear-2024.86

ifeq ($(CONFIG_FIRMWARE_INCLUDE_DROPBEAR_FAST_CODE),y)
COPT=-DDROPBEAR_SMALL_CODE=0
else
COPT=-DDROPBEAR_SMALL_CODE=1
endif

ifeq ($(CONFIG_FIRMWARE_INCLUDE_SFTP),y)
COPT+=-DSFTPSERVER_PATH=\"/usr/libexec/sftp-server\"
else
COPT+=-DSFTPSERVER_PATH=\"/opt/libexec/sftp-server\"
endif
CFLAGS+=$(COPT) -ffunction-sections -fdata-sections
LDFLAGS += -Wl,--gc-sections

all: extract_test config_test
	$(MAKE) -j$(HOST_NCPU) -C $(SRC_NAME) PROGRAMS="dropbear dbclient dropbearkey dropbearconvert scp" MULTI=1

extract_test:
	( if [ ! -d $(SRC_NAME) ]; then \
		tar xjf $(SRC_NAME).tar.bz2; \
		if [ -f localoptions.h ]; then \
			cp localoptions.h $(SRC_NAME)/localoptions.h; \
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
	autoreconf -v; \
	./configure \
		--prefix=/usr \
		--disable-lastlog \
		--disable-utmp \
		--disable-utmpx \
		--disable-wtmp \
		--disable-wtmpx \
		--disable-loginfunc \
		--disable-pututline \
		--disable-pututxline \
		--enable-zlib \
		--enable-bundled-libtom \
		--host=$(HOST_TARGET) \
		--build=$(HOST_BUILD) ; \
	)

clean:
	if [ -f $(SRC_NAME)/Makefile ] ; then \
		$(MAKE) -C $(SRC_NAME) distclean ; \
	fi ; \
	rm -f dropbearmulti
	rm -f config_done

romfs:
	cp $(SRC_NAME)/dropbearmulti .
	$(STRIP) dropbearmulti
	$(ROMFSINST) /usr/bin/dropbearmulti
	$(ROMFSINST) -p +x /usr/bin/sshd.sh
	$(ROMFSINST) -s dropbearmulti /usr/bin/ssh
	$(ROMFSINST) -s dropbearmulti /usr/bin/scp
	$(ROMFSINST) -s dropbearmulti /usr/bin/dropbearkey
	$(ROMFSINST) -s dropbearmulti /usr/bin/dropbearconvert
	$(ROMFSINST) -s /usr/bin/dropbearmulti /usr/sbin/dropbear
