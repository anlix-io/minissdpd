#
# Copyright (C) 2017-2019 Anlix
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=minissdpd
PKG_VERSION:=1.5.20190210
PKG_RELEASE:=1

PKG_SOURCE_URL:=http://miniupnp.free.fr/files
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_HASH:=79f7eea9fe71a08dab83211370f2b8a8eda11fd3b197628a87ae12a021a1a518

PKG_MAINTAINER:=Guilherme Senges <guilherme@anlix.io>
PKG_LICENSE:=BSD-3-Clause

PKG_BUILD_DIR := $(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk

define Package/minissdpd
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=+libnfnetlink
  TITLE:=SSDP managing daemon. Designed to work with miniupnpc, miniupnpd
  SUBMENU:=Firewall
  URL:=http://miniupnp.free.fr/
endef

define Build/Compile
  $(MAKE) -C $(PKG_BUILD_DIR) \
          CC="$(TARGET_CC)" \
          CFLAGS="$(TARGET_CFLAGS) -Wall" \
          LDFLAGS="$(TARGET_LDFLAGS)"
endef

define Package/minissdpd/install
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/minissdpd.init.d.script $(1)/etc/init.d/minissdpd
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/minissdpd $(1)/usr/sbin/
endef

$(eval $(call BuildPackage,minissdpd))
