# 
# Copyright (C) 2006 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libupnp
PKG_VERSION:=1.6.6
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=@SF/pupnp
PKG_MD5SUM:=8918dcf7428cd119d0c8275765ff2833

include $(INCLUDE_DIR)/package.mk

define Package/libupnp/Default
  SECTION:=libs
  CATEGORY:=Libraries
  URL:=http://pupnp.sourceforge.net/
endef

define Package/libupnp
  $(call Package/libupnp/Default)
  DEPENDS:=+libpthread
  TITLE:=UPnP SDK library
  MENU:=1
endef

define Package/libupnp/description
        The portable SDK for UPnP Devices (libupnp) provides developers with an
	API and open source code for building control points, devices, and
	bridges that are compliant with Version 1.0 of the  Universal Plug and
	Play Device Architecture Specification and support several operating
	systems like Linux, *BSD, Solaris and others.
endef

define Package/libupnp-sample
  $(call Package/libupnp/Default)
  DEPENDS:=libupnp
  TITLE:=UPnP sample applications
endef

define Package/libupnp-sample/description
	TVcontrolpoint & tvdevice sample applications
	run inside /etc/upnp-tvdevice/
endef

define Build/Configure
	$(call Build/Configure/Default, \
		--enable-shared \
		--enable-static \
	);
endef

TARGET_CFLAGS += $(FPIC)

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) \
		CCOPT="$(TARGET_CFLAGS)" \
		DESTDIR="$(PKG_INSTALL_DIR)" \
		all install
	$(MAKE) -C $(PKG_BUILD_DIR)/upnp/sample/ upnp_tv_device upnp_tv_ctrlpt
endef

define Build/Compile/libupnp-sample
	$(MAKE) -C $(PKG_BUILD_DIR) \
		CCOPT="$(TARGET_CFLAGS)" \
		DESTDIR="$(PKG_INSTALL_DIR)" \
		check
endef

define Build/InstallDev
	mkdir -p $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/upnp $(1)/usr/include/
	mkdir -p $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/lib{ixml,threadutil,upnp}.{a,so*} $(1)/usr/lib/
	mkdir -p $(1)/usr/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/libupnp.pc $(1)/usr/lib/pkgconfig/
endef

define Package/libupnp/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/lib{ixml,threadutil,upnp}.so.* $(1)/usr/lib/
endef

define Package/libupnp-sample/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/upnp/sample/upnp_tv_device $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/upnp/sample/upnp_tv_ctrlpt $(1)/usr/bin
	$(INSTALL_DIR) $(1)/etc/upnp-tvdevice/web
	$(CP) $(PKG_BUILD_DIR)/upnp/sample/web/* $(1)/etc/upnp-tvdevice
endef

$(eval $(call BuildPackage,libupnp))
$(eval $(call BuildPackage,libupnp-sample))
