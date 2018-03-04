#
# Copyright (C) 2008-2016 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=plan-b-tor
PKG_VERSION:=0.3.2.10
PKG_RELEASE:=1

PKG_BUILD_DIR:=$(BUILD_DIR)/tor-$(PKG_VERSION)
PKG_SOURCE:=tor-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://dist.torproject.org/ \
	https://archive.torproject.org/tor-package-archive
PKG_HASH:=60df77c31dcf94fdd686c8ca8c34f3b70243b33a7344ecc0b719d5ca2617cbee
PKG_MAINTAINER:=Reto Schneider <code@reto-schneider.ch>
PKG_LICENSE_FILES:=LICENSE

PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/plan-b-tor/Default
  SECTION:=net
  CATEGORY:=Network
  URL:=https://plan-b.digitale-gesellschaft.ch/
  USERID:=tor=52:tor=52
endef

define Package/plan-b-tor/Default/description
 Tor is a toolset for a wide range of organizations and people that want to
 improve their safety and security on the Internet. Using Tor can help you
 anonymize web browsing and publishing, instant messaging, IRC, SSH, and
 more. Tor also provides a platform on which software developers can build
 new applications with built-in anonymity, safety, and privacy features.

 This build uses only two nodes instead of three to improve the bandwith.
endef

define Package/plan-b-tor
$(call Package/plan-b-tor/Default)
  TITLE:=An anonymous Internet communication system
  DEPENDS:=+libevent2 +libopenssl +libpthread +librt +zlib +libcap
endef

define Package/plan-b-tor/description
$(call Package/plan-b-tor/Default/description)
 This package contains the Plan B tor daemon.
endef

define Package/plan-b-tor-gencert
$(call Package/plan-b-tor/Default)
  TITLE:=Tor certificate generation
  DEPENDS:=+plan-b-tor
endef

define Package/plan-b-tor-gencert/description
$(call Package/plan-b-tor/Default/description)
 Generate certs and keys for Tor directory authorities
endef

define Package/plan-b-tor-resolve
$(call Package/plan-b-tor/Default)
  TITLE:=Plan B tor hostname resolve
  DEPENDS:=+plan-b-tor
endef

define Package/plan-b-tor-resolve/description
$(call Package/plan-b-tor/Default/description)
 Resolve a hostname to an IP address via tor 
endef

define Package/plan-b-tor-geoip
$(call Package/plan-b-tor/Default)
  TITLE:=GeoIP db for tor
  DEPENDS:=+plan-b-tor
endef

define Package/plan-b-tor-geoip/description
$(call Package/plan-b-tor/Default/description)
 This package contains a GeoIP database mapping IP addresses to countries.
endef

define Package/plan-b-tor/conffiles
/var/lib/tor/fingerprint
/var/lib/tor/keys/*
endef

CONFIGURE_ARGS += \
	--with-libevent-dir="$(STAGING_DIR)/usr" \
	--with-ssl-dir="$(STAGING_DIR)/usr" \
	--with-openssl-dir="$(STAGING_DIR)/usr" \
	--with-zlib-dir="$(STAGING_DIR)/usr" \
	--disable-asciidoc \
	--disable-seccomp \
	--disable-libscrypt \
	--disable-unittests \
	--disable-largefile \
	--disable-lzma \
	--with-tor-user=tor \
	--with-tor-group=tor

EXTRA_CFLAGS += -std=gnu99

ifneq ($(CONFIG_SSP_SUPPORT),y)
	CONFIGURE_ARGS += \
		--disable-gcc-hardening
else
	EXTRA_CFLAGS += -fPIC
endif

CONFIGURE_VARS += \
	CROSS_COMPILE="yes"

define Package/plan-b-tor/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/tor $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/torify $(1)/usr/sbin/
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/tor.init $(1)/etc/init.d/tor
	$(INSTALL_DIR) $(1)/etc/tor
endef

define Package/plan-b-tor-gencert/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/tor-gencert $(1)/usr/sbin/
endef

define Package/plan-b-tor-resolve/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/tor-resolve $(1)/usr/sbin/
endef

define Package/plan-b-tor-geoip/install
	$(INSTALL_DIR) $(1)/usr/share/tor
	$(CP) $(PKG_INSTALL_DIR)/usr/share/tor/geoip $(1)/usr/share/tor/
	$(CP) $(PKG_INSTALL_DIR)/usr/share/tor/geoip6 $(1)/usr/share/tor/
endef

$(eval $(call BuildPackage,plan-b-tor))
$(eval $(call BuildPackage,plan-b-tor-gencert))
$(eval $(call BuildPackage,plan-b-tor-resolve))
$(eval $(call BuildPackage,plan-b-tor-geoip))
