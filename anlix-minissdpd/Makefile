#
# Copyright (C) 2017-2019 Anlix
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=minissdpd
PKG_VERSION:=1.5.20190823
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/anlix-io/miniupnp.git
PKG_SOURCE_DATE:=2019-08-23
PKG_SOURCE_VERSION:=bde74ff9e87aeaf7e6c705251bf1dcbb4505d52d
PKG_MIRROR_HASH:=963bcd69cf9e2867a780bdefae29c4b98aa8a7c4c85d36a1579ee3eed188c06

PKG_MAINTAINER:=Guilherme Senges <guilherme@anlix.io>
PKG_LICENSE:=BSD-3-Clause

PKG_BUILD_DIR := $(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk

define Package/anlix-minissdpd
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=+libnfnetlink
  TITLE:=SSDP managing daemon. Designed to work with miniupnpc, miniupnpd
  URL:=http://miniupnp.free.fr/
endef

define Build/Compile
  $(MAKE) -C $(PKG_BUILD_DIR)/minissdpd \
          CC="$(TARGET_CC)" \
          CFLAGS="$(TARGET_CFLAGS) -Wall" \
          LDFLAGS="$(TARGET_LDFLAGS)"
endef

define Package/anlix-minissdpd/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_DIR) $(1)/etc/init.d

	$(INSTALL_BIN) $(PKG_BUILD_DIR)/minissdpd/minissdpd $(1)/usr/sbin/
	$(INSTALL_BIN) ./files/minissdpd.init $(1)/etc/init.d/minissdpd
endef

$(eval $(call BuildPackage,anlix-minissdpd))
