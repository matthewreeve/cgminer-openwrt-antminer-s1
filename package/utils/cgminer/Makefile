# Buildroot Makefile for kanoi's cgminer
#
# Patches included fix Antminer S1 compatibility issues
# (at the expense of Antminer S2 support) and fix
# the API compatibility with the status page

include $(TOPDIR)/rules.mk

PKG_NAME:=cgminer

PKG_VERSION:=4.4.2
PKG_REV:=24361b5ad795113e6e72611ce30ac08cf18bb6ed

PKG_RELEASE:=1
PKG_INSTALL:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_REV).tar.bz2
PKG_SOURCE_URL:=git://github.com/kanoi/cgminer.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=$(PKG_REV)

PKG_FIXUP:=autoreconf

include $(INCLUDE_DIR)/package.mk

define Package/cgminer
	SECTION:=utils
	CATEGORY:=Utilities
	TITLE:=cgminer
	URL:=https://github.com/ckolivas/cgminer
	DEPENDS:=+libcurl +libpthread +jansson +udev +libncurses
endef

define Package/cgminer/description
Cgminer is a general-purpose mining program with support for many GPUs, CPUs,
ASICs, and FPGAs with support for overclocking and many proof-of-work
algorithms.
endef

TARGET_LDFLAGS += -Wl,-rpath-link=$(STAGING_DIR)/usr/lib

CONFIGURE_ARGS += --enable-ants1 --disable-libcurl

define Build/Prepare
	$(call Build/Prepare/Default)
endef

define Build/Compile
	$(call Build/Compile/Default)
	(cd $(PKG_BUILD_DIR) && \
	  $(TARGET_CC) -Icompat/jansson -Icompat/libusb-1.0/libusb \
	  api-example.c -o cgminer-api;)
endef

define Package/cgminer/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_DIR) $(1)/etc/config

	$(INSTALL_BIN) $(PKG_BUILD_DIR)/cgminer-api	   $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/cgminer  $(1)/usr/bin

endef

$(eval $(call BuildPackage,cgminer))

