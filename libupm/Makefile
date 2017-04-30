#
# Copyright (C) 2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libupm
PKG_VERSION:=1.2.0

PKG_RELEASE=$(PKG_SOURCE_VERSION)

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/intel-iot-devkit/upm.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=7b812284c5692e70f1ab35435fada88e5a9d67d1
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz
CMAKE_INSTALL:=1

PKG_MAINTAINER:=John Crispin <blogic@openwrt.org>
PKG_LICENSE:=LGPL-2.1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

UPM_MODULES:= a110x

CMAKE_OPTIONS=-DBUILDARCH=$(CONFIG_ARCH) \
	-DNODE_EXECUTABLE=$(STAGING_DIR_HOST)/bin/node \
	-DSWIG_DIR=$(STAGING_DIR_HOST)/bin

define Package/libupm/Default
  SECTION:=libs
  CATEGORY:=Libraries
  DEPENDS:=+libmraa +librt +libpthread
  SUBMENU:=IoT
endef

define Package/libupm
  $(call Package/libupm/Default)
  TITLE:=Intel IoT sensor library - Full
endef

define Package/libupm/install/Default
	$(INSTALL_DIR) $(1)/usr/lib/{node/,python2.7/site-packages}; \
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libupm-$(2).so* $(1)/usr/lib/; \
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libupmc-$(2).so* $(1)/usr/lib/; \
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/node_modules/jsupm_$(2) $(1)/usr/lib/node/; \
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/python2.7/site-packages/upm/{pyupm_$(2).py,_pyupm_$(2).so} \
		$(1)/usr/lib/python2.7/site-packages/ ;
endef

define Package/libupm/install
	$(foreach module, $(UPM_MODULES),	\
  		$(call Package/libupm/install/Default,$(1),$(module)))
endef

define UpmPackage
define Package/libupm-$(1)
  $(call Package/libupm/Default)
  TITLE:=Intel IoT sensor library - $(1)
endef

define Package/libupm-$(1)/install
  	$(call Package/libupm/install/Default,$$(1),$(1))
endef
endef

$(eval $(call BuildPackage,libupm))
$(foreach package, $(UPM_MODULES),			\
	$(eval $(call UpmPackage,$(package)))		\
	$(eval $(call BuildPackage,libupm-$(package)))	\
)
