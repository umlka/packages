# SPDX-License-Identifier: GPL-3.0-only
#
# Copyright (C) 2021 ImmortalWrt.org

include $(TOPDIR)/rules.mk

PKG_NAME:=mosdns
PKG_VERSION:=5.1.3-0cb2257
PKG_RELEASE:=$(AUTORELEASE)

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/IrineSistiana/mosdns.git
PKG_SOURCE_VERSION:=0cb2257a6cbb63e0717f2324bb7563c32714934f
PKG_MIRROR_HASH:=skip

PKG_LICENSE:=GPL-3.0
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=Tianling Shen <cnsztl@immortalwrt.org>

PKG_BUILD_DEPENDS:=golang/host
PKG_BUILD_PARALLEL:=1
PKG_BUILD_FLAGS:=no-mips16

GO_PKG:=github.com/IrineSistiana/mosdns
GO_PKG_LDFLAGS_X:=main.version=$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk
include $(TOPDIR)/feeds/packages/lang/golang/golang-package.mk

define Package/$(PKG_NAME)
	SECTION:=net
	CATEGORY:=Network
	SUBMENU:=IP Addresses and Names
	TITLE:=A plug-in DNS forwarder/splitter
	URL:=https://github.com/IrineSistiana/mosdns
	DEPENDS:=$(GO_ARCH_DEPENDS) +ca-bundle +v2dat +v2ray-geoip +v2ray-geosite
	PROVIDES:=mosdns-neo
endef

GO_PKG_TARGET_VARS:=$(filter-out CGO_ENABLED=%,$(GO_PKG_TARGET_VARS)) CGO_ENABLED=0

define Package/$(PKG_NAME)/install
	$(call GoPackage/Package/Install/Bin,$(1))

	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/mosdns.init $(1)/etc/init.d/mosdns

	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) ./files/mosdns.conf $(1)/etc/config/mosdns

	$(INSTALL_DIR) $(1)/etc/mosdns/rule
	$(INSTALL_DATA) ./files/mosdns/rule/* $(1)/etc/mosdns/rule/

	$(INSTALL_DIR) $(1)/etc/mosdns
	$(INSTALL_DATA) ./files/mosdns/cache.dump $(1)/etc/mosdns/cache.dump
	$(INSTALL_DATA) ./files/mosdns/config.yaml $(1)/etc/mosdns/config.yaml
endef

define Package/$(PKG_NAME)/postinst
#!/bin/sh

# check if we are on real system
if [ -z "$${IPKG_INSTROOT}" ]; then
	/etc/init.d/mosdns stop >/dev/null 2>&1
	/etc/init.d/mosdns disable >/dev/null 2>&1
fi
exit 0
endef

$(eval $(call GoBinPackage,$(PKG_NAME)))
$(eval $(call BuildPackage,$(PKG_NAME)))
