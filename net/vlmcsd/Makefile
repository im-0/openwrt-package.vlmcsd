#
# Copyright (C) 2006-2024 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=vlmcsd
PKG_SOURCE_DATE:=2024-04-09
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/Wind4/vlmcsd.git
PKG_SOURCE_VERSION:=70e03572b254688b8c3557f898e7ebd765d29ae1

PKG_MAINTAINER:=Ivan Mironov <mironov.ivan@gmail.com>
PKG_LICENSE:=AS-IS

PKG_BUILD_PARALLEL:=1
PKG_BUILD_FLAGS:=gc-sections lto

include $(INCLUDE_DIR)/package.mk

MAKE_FLAGS+=STRIP=1 VERBOSE=1 THREADS=0 FEATURES=full

define Package/vlmcsd
	SECTION:=net
	CATEGORY:=Network
	TITLE:=KMS Emulator in C
endef

define Package/vlmcsd/description
	Windows KMS Emulator written in C.
endef

define Build/Compile
	$(call Build/Compile/Default)
	$(SED) 's,^\s*;\s*KmsData\s*=.*$$$$,KmsData=/usr/share/vlmcsd/vlmcsd.kmd,' $(PKG_BUILD_DIR)/etc/vlmcsd.ini
endef

define Package/vlmcsd/install	
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/bin/vlmcsd $(1)/usr/sbin/

	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/vlmcsd.init $(1)/etc/init.d/vlmcsd

	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_BIN) ./files/vlmcsd.conf $(1)/etc/config/vlmcsd

	$(INSTALL_DIR) $(1)/usr/share/vlmcsd
	# https://github.com/kankerdev/vlmcsd/commit/88818956e9e476bbff1797888b3367bd57181a05
	$(INSTALL_BIN) ./files/vlmcsd.kmd $(1)/usr/share/vlmcsd/

	$(INSTALL_DIR) $(1)/etc/vlmcsd
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/etc/vlmcsd.ini $(1)/etc/vlmcsd/
endef

$(eval $(call BuildPackage,vlmcsd))
