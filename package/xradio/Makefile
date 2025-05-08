include $(TOPDIR)/rules.mk

PKG_NAME:=xradio
PKG_RELEASE=1

PKG_LICENSE:=GPLv2
PKG_LICENSE_FILES:=

PKG_SOURCE_URL:=https://github.com/zfdx123/xradio.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2024-12-06
PKG_SOURCE_VERSION:=180aafb14191c78c1529d5a28ca58c7c9dcf2c55
PKG_HASH:=c221b4cd4e275d99b5e30021f7a7d04a371696338094b2300fe397d0d2c46a5a

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk


define KernelPackage/xradio
  SUBMENU:=Wireless Drivers xr819
  TITLE:=Xradio xr819 wifi-drivers support
  DEPENDS:=@TARGET_sunxi +kmod-mac80211 +wpad-mini
  KCONFIG:= \
	CONFIG_WLAN_VENDOR_XRADIO=m \
	CONFIG_XRADIO_USE_EXTENSIONS=y
  FILES:=$(PKG_BUILD_DIR)/xradio_wlan.ko
  AUTOLOAD:=$(call AutoProbe,xradio_wlan)
endef

NOSTDINC_FLAGS = \
	-I$(PKG_BUILD_DIR) \
	-I$(STAGING_DIR)/usr/include/mac80211-backport/uapi \
	-I$(STAGING_DIR)/usr/include/mac80211-backport \
	-I$(STAGING_DIR)/usr/include/mac80211/uapi \
	-I$(STAGING_DIR)/usr/include/mac80211 \
	-include backport/autoconf.h \
	-include backport/backport.h

define Build/Compile
	+$(MAKE) $(PKG_JOBS) -C "$(LINUX_DIR)" \
		$(KERNEL_MAKE_FLAGS) \
		$(PKG_MAKE_FLAGS) \
		M="$(PKG_BUILD_DIR)" \
		NOSTDINC_FLAGS="$(NOSTDINC_FLAGS)" \
		modules
endef

MAKEFILE_DIR := $(dir $(lastword $(MAKEFILE_LIST)))

define KernelPackage/xradio/install
	$(INSTALL_DIR) $(1)/lib/firmware/xr819
	$(TAR) -C $(1)/lib/firmware -xjf $(MAKEFILE_DIR)/firmware/xr819.tar.bz2
endef

$(eval $(call KernelPackage,xradio))