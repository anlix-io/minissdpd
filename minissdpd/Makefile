#
# Copyright (C) 2017-2019 Anlix
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=minissdpd
PKG_VERSION:=1.5
PKG_RELEASE:=1

PKG_SOURCE_URL:=http://miniupnp.free.fr/files
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_HASH:=e957f403960c1c6b5fd9f2ea5ff23c3217f5d4486be2f36159122459d08cf540

PKG_MAINTAINER:=Guilherme Senges <guilherme@anlix.io>
PKG_LICENSE:=BSD-3-Clause

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/version.mk

define Package/minissdpd
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=+iptables +libip4tc +IPV6:libip6tc +IPV6:ip6tables +libuuid
  TITLE:=SSDP managing daemon. Designed to work with miniupnpc, miniupnpd
  SUBMENU:=Firewall
  URL:=http://miniupnp.free.fr/
endef

#define Package/minissdpd/conffiles
#/etc/config/upnpd
#endef

define Build/Prepare
	$(call Build/Prepare/Default)
	echo "$(VERSION_NUMBER)" | tr '() ' '_' >$(PKG_BUILD_DIR)/os.openwrt
endef

TARGET_CFLAGS += -flto -ffunction-sections -fdata-sections
TARGET_LDFLAGS += -flto -Wl,--gc-sections
MAKE_FLAGS += \
	TARGET_OPENWRT=1 TEST=0 LIBS="" \
	CC="$(TARGET_CC) -DIPTABLES_143 -lip4tc -luuid \
		$(if $(CONFIG_IPV6),-lip6tc)" \
	CONFIG_OPTIONS="--portinuse --leasefile --igd2 \
		$(if $(CONFIG_IPV6),--ipv6)" \
	-f Makefile minissdpd

define Package/minissdpd/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_DIR) $(1)/etc/hotplug.d/iface
	$(INSTALL_DIR) $(1)/etc/uci-defaults
	$(INSTALL_DIR) $(1)/usr/share/minissdpd

	$(INSTALL_BIN) $(PKG_BUILD_DIR)/minissdpd $(1)/usr/sbin/minissdpd
endef

$(eval $(call BuildPackage,minissdpd))
