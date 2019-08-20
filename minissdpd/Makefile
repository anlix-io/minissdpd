#
# Copyright (C) 2017-2019 Anlix
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=minissdpd
PKG_VERSION:=1.5.20190408
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/anlix-io/miniupnp.git
PKG_SOURCE_DATE:=2019-04-08
PKG_SOURCE_VERSION:=d3b1e177464434e487dc78488752d7d0e183d0fc
PKG_MIRROR_HASH:=39e1bafd1305ea9956cddb02a097382854db7237c09cf245bd1d06e9e1cf533b

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
  $(MAKE) -C $(PKG_BUILD_DIR)/minissdpd \
          CC="$(TARGET_CC)" \
          CFLAGS="$(TARGET_CFLAGS) -Wall" \
          LDFLAGS="$(TARGET_LDFLAGS)"
endef

define Package/minissdpd/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/minissdpd/minissdpd $(1)/usr/sbin/
endef

$(eval $(call BuildPackage,minissdpd))
