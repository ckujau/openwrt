#
# Copyright (C) 2006-2020 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=nrpe
PKG_VERSION:=4.0.0
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=@GITHUB/NagiosEnterprises/nrpe.git
PKG_LICENSE:=GPL-2.0
PKG_HASH:=a3d0a7ac5385421dfcc29afce7eeade13ca10d8d9115ca5c2962b72946c88a1e

PKG_INSTALL:=1
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk

define Package/nrpe
	SECTION:=net
	CATEGORY:=Network
	TITLE:=Nagios Remote Plugin Executor Server
	URL:=https://github.com/NagiosEnterprises/nrpe
	DEPENDS:=
	USERID:=nagios:nagios
	MAINTAINER:=Christian Kujau <openwrt@nerdbynature.de>
endef

define Package/nrpe/description
	This program runs as a background process on the remote host and processes
	command execution requests from the check_nrpe plugin on the Nagios host.
endef

CONFIGURE_ARGS += \
	--disable-ssl
#	--with-ssl=$(STAGING_DIR)/usr \
#	--with-ssl-lib=$(STAGING_DIR)/usr/lib

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) nrpe
endef

define Package/nrpe/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/openwrt-init  $(1)/etc/init.d/$(PKG_NAME)
	$(INSTALL_BIN) ./files/openwrt-procd $(1)/etc/init.d/$(PKG_NAME).procd
	$(CP) $(PKG_BUILD_DIR)/src/nrpe $(1)/usr/sbin
	$(CP) $(PKG_BUILD_DIR)/sample-config/nrpe.cfg $(1)/etc/nrpe.cfg
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/nrpe $(1)/usr/sbin
endef

define Package/nrpe/conffiles
	/etc/nrpe.cfg
endef

$(eval $(call BuildPackage,nrpe))
