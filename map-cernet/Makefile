#
# Copyright (C) 2006-2012 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=cernet-MAP
PKG_VERSION:=1
PKG_RELEASE:=0

# PKG_SOURCE:=master.zip
# $(PKG_NAME)_$(PKG_VERSION).tar.gz
# PKG_SOURCE_URL:=https://github.com/cernet/MAP/archive/

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/cernet/MAP.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=77d1b352a982794c35793ed1cb80bdea59eedf81
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)


# PKG_MD5SUM:=5206a18e0af02cb694737b6f2c698f18

# PKG_BUILD_DIR:=$(BUILD_DIR)/MAP-master

include $(INCLUDE_DIR)/package.mk

define Package/map-cernet
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=+kmod-ipv6 +kmod-ip6-tunnel +kmod-map-cernet
  TITLE:=cernet MAP-E/MAP-T driver/control utility
  MAINTAINER:=CERNET MAP team <mapt.cernet@gmail.com>
  PKGARCH:=all
endef

define KernelPackage/map-cernet
  DEPENDS:=+kmod-ipv6
  TITLE:=IVI cernet MAP-E/MAP-T kernel module
  SECTION:=kernel
  SUBMENU:=Network Support
  FILES:=$(PKG_BUILD_DIR)/modules/ivi.ko 
endef

MAKE_KMOD := $(MAKE) -C "$(LINUX_DIR)" \
		CROSS_COMPILE="$(TARGET_CROSS)" \
		ARCH="$(LINUX_KARCH)" \
		PATH="$(TARGET_PATH)" \
		SUBDIRS="$(PKG_BUILD_DIR)/kmod" \

define Build/Prepare
	$(call Build/Prepare/Default)
endef

define Build/Compile
	# Compile the kernel part
	$(MAKE_KMOD) \
		SUBDIRS="$(PKG_BUILD_DIR)/modules" \
		MODFLAGS="-DMODULE -mlong-calls" \
		modules

	pwd

	$(TARGET_CC) -o $(PKG_BUILD_DIR)/ivictl \
		-I$(PKG_BUILD_DIR)/module \
		$(PKG_BUILD_DIR)/utils/ivictl.c 

endef

define Build/InstallDev
endef

define Package/map-cernet/install
	$(INSTALL_DIR) $(1)/sbin
	$(CP) -r ./files/* $(1)/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/ivictl $(1)/sbin/
endef

$(eval $(call KernelPackage,map-cernet))
$(eval $(call BuildPackage,map-cernet))
