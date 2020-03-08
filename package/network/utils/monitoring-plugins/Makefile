#
# Copyright (C) 2008-2020 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=monitoring-plugins
PKG_VERSION:=2.2
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/monitoring-plugins/monitoring-plugins.git
PKG_SOURCE_VERSION:=db93deea319e54aea52618108adfebdfab1f1a26
PKG_MIRROR_HASH:=d3b100ed6077dabd30ec6846ef7cd80c0d911a7e738d99742b6af2ea2c8d005b
PKG_LICENSE:=GPL-3.0

include $(INCLUDE_DIR)/package.mk

define Package/monitoring-plugins
	SECTION:=net
	CATEGORY:=Network
	TITLE:=Monitoring Plugins
	URL:=https://www.monitoring-plugins.org
	DEPENDS:=
#	DEPENDS:=+libopenssl +libpthread
	MAINTAINER:=Christian Kujau <openwrt@nerdbynature.de>
endef
  
define Package/nrpe/description
	Plugins for Nagios compatible monitoring systems like Naemon or Icinga.
endef

define Build/Prepare
	$(Build/Prepare/Default)
	(cd $(PKG_BUILD_DIR) && ./tools/setup)
endef

CONFIGURE_ARGS += \
        --disable-extra-opts \
        --disable-largefile \
        --disable-maintainer-mode \
        --disable-nls \
        --disable-threads \
        --without-cgiurl \
	--without-dbi \
	--without-ldap \
	--without-mysql \
        --without-openssl \
	--without-pgsql \
	--without-radius \
	--with-ping-command="/bin/ping -w %d -c %d %s" \
	--with-ping6-command="/bin/ping6 -w %d -c %d %s" \
	--with-ps-command="/bin/ps w" \
	--with-ps-cols="6" \
	--with-ps-format="%d %s %d %s %s %n" \
	--with-ps-varlist="&procpid$(comma)&procuid$(comma)&procvsz$(comma)procstat$(comma)procprog$(comma)&pos" \
	--with-uptime-command="/usr/bin/uptime" \
	--with-ssh-command="/usr/bin/ssh" \
	--with-nslookup-command="/usr/bin/nslookup"

define Package/monitoring-plugins/install
	$(INSTALL_DIR) $(1)/usr/libexec/nagios
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/plugins/check_* $(1)/usr/libexec/nagios/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/plugins/negate  $(1)/usr/libexec/nagios/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/plugins/urlize  $(1)/usr/libexec/nagios/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/plugins-scripts/utils.sh      $(1)/usr/libexec/nagios/
	$(RM) $(1)/usr/libexec/nagios/*.c $(1)/usr/libexec/nagios/*.h $(1)/usr/libexec/nagios/*.o
endef

$(eval $(call BuildPackage,monitoring-plugins))
