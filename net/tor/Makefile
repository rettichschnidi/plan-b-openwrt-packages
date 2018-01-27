#
# Copyright (C) 2008-2016 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=plan-b-tor
PKG_VERSION:=0.3.1.9
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://dist.torproject.org/ \
	https://archive.torproject.org/tor-package-archive
PKG_HASH:=6e1b04f7890e782fd56014a0de5075e4ab29b52a35d8bca1f6b80c93f58f3d26
PKG_MAINTAINER:=Reto Schneider <code@reto-schneider.ch>
PKG_LICENSE_FILES:=LICENSE

PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/plan-b-tor/Default
  SECTION:=net
  CATEGORY:=Network
	URL:=https://plan-b.reto-schneider.ch/
  USERID:=tor=52:tor=52
endef

define Package/plan-b-tor/Default/description
 Tor is a toolset for a wide range of organizations and people that want to
 improve their safety and security on the Internet. Using Tor can help you
 anonymize web browsing and publishing, instant messaging, IRC, SSH, and
 more. Tor also provides a platform on which software developers can build
 new applications with built-in anonymity, safety, and privacy features.
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

define Package/plan-b-tor/conffiles
/etc/tor/torrc
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
	$(INSTALL_CONF) $(PKG_INSTALL_DIR)/etc/tor/torrc.sample $(1)/etc/tor/torrc
endef

$(eval $(call BuildPackage,plan-b-tor))
