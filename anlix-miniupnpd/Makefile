#
# Copyright (C) 2006-2014 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=miniupnpd
PKG_VERSION:=2.1.20190824
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/anlix-io/miniupnp.git
PKG_SOURCE_DATE:=2019-08-24
PKG_SOURCE_VERSION:=8ab91c37d12ee6cf36ecc041764b71b3d9f92815
PKG_MIRROR_HASH:=28a85c92e0addb08fbd9fa5c8667f7b898c385de5b34d52f7be3394b7ea199c5

PKG_MAINTAINER:=Guilherme Senges <guilherme@anlix.io>
PKG_LICENSE:=BSD-3-Clause

PKG_BUILD_DIR := $(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/version.mk

define Package/anlix-miniupnpd
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=+iptables +libip4tc +IPV6:libip6tc +IPV6:ip6tables +libuuid +anlix-minissdpd
  TITLE:=Lightweight UPnP IGD, NAT-PMP & PCP daemon
  URL:=http://miniupnp.free.fr/
endef

define Package/anlix-miniupnpd/conffiles
/etc/config/upnpd
endef

define Build/Prepare
	$(call Build/Prepare/Default)
	echo "$(VERSION_NUMBER)" | tr '() ' '_' >$(PKG_BUILD_DIR)/miniupnpd/os.openwrt
endef

TARGET_CFLAGS += -flto -ffunction-sections -fdata-sections
TARGET_LDFLAGS += -flto -Wl,--gc-sections
MAKE_FLAGS += \
	TARGET_OPENWRT=1 TEST=0 LIBS="" \
	CC="$(TARGET_CC) -DIPTABLES_143 -lip4tc -luuid \
		$(if $(CONFIG_IPV6),-lip6tc)" \
	CONFIG_OPTIONS="--portinuse --leasefile --igd2" \
	-f Makefile.linux miniupnpd

define Package/anlix-miniupnpd/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_DIR) $(1)/etc/hotplug.d/iface
	$(INSTALL_DIR) $(1)/etc/uci-defaults
	$(INSTALL_DIR) $(1)/usr/share/miniupnpd

	$(INSTALL_BIN) $(PKG_BUILD_DIR)/miniupnpd/miniupnpd $(1)/usr/sbin/miniupnpd
	$(INSTALL_BIN) ./files/miniupnpd.init $(1)/etc/init.d/miniupnpd
	$(INSTALL_CONF) ./files/upnpd.config $(1)/etc/config/upnpd
	$(INSTALL_DATA) ./files/miniupnpd.hotplug $(1)/etc/hotplug.d/iface/50-miniupnpd
	$(INSTALL_BIN) ./files/miniupnpd.defaults $(1)/etc/uci-defaults/99-miniupnpd
	$(INSTALL_DATA) ./files/firewall.include $(1)/usr/share/miniupnpd/firewall.include
endef

$(eval $(call BuildPackage,anlix-miniupnpd))
