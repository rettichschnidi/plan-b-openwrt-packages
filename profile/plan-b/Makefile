#
# Copyright (C) 2018 Reto Schneider <code@reto-schneider.ch>
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

include $(INCLUDE_DIR)/package.mk

PKG_NAME:=plan-b
PKG_VERSION:=0.0.1
PKG_RELEASE:=1
PKG_MAINTAINER:=Reto Schneider <code@reto-schneider.ch>

define Package/plan-b/Default
  SECTION:=plan-b
  CATEGORY:=Plan B
  URL:=https://plan-b.digitale-gesellschaft.ch/
endef

define Package/plan-b/Default/description
  Metapackage for packages required by Plan B.
endef

define Package/plan-b
$(call Package/plan-b/Default)
  TITLE:=Metapackage for Plan B
  DEPENDS:=+plan-b-tor +ca-bundle +ca-certificates +curl
endef

$(eval $(call BuildPackage,plan-b))
