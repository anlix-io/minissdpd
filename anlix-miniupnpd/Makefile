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
PKG_SOURCE_VERSION:=5bcfa25031a2072793a81faf3cda6e7e82e4a481
PKG_MIRROR_HASH:=84a8739708573d0b249380f15ed0346348867c2b217a43e6a2d13327afa0409f

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

define Build/Compile
	CFLAGS="$(TARGET_CFLAGS)" \
	LDFLAGS="$(TARGET_LDFLAGS)" \
	$(MAKE) -C $(PKG_BUILD_DIR)/miniupnpd \
		TARGET_OPENWRT=1 TEST=0 LIBS="" \
		CC="$(TARGET_CC) -DIPTABLES_143 -lip4tc -luuid \
			$(if $(CONFIG_IPV6),-lip6tc)" \
		CONFIG_OPTIONS="--portinuse --leasefile --igd2" \
		-f Makefile.linux miniupnpd
endef

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
