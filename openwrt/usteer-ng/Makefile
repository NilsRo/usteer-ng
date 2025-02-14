include $(TOPDIR)/rules.mk

PKG_NAME:=usteer-ng
PKG_VERSION:=1.0.0
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/NilsRo/usteer-ng/tar.gz/$(PKG_NAME)-$(PKG_VERSION)?
PKG_HASH:=105611ae3b6d8fc5656f710af7919e34c65bc915bec37b268eb0a8c3989f294d

PKG_RELEASE:=1
PKG_MAINTAINER:=Nils Hendrik Rottgardt <n.rottgardt@gmail.com>
PKG_LICENSE:=GPL-2.0-only

PKG_BUILD_DEPENDS:=libpcap
PKG_BUILD_PARALLEL:=1

PKG_FILE_DEPENDS:=$(CURDIR)/../..

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	ln -s $(CURDIR)/../../.git $(PKG_BUILD_DIR)/.git
	cd $(PKG_BUILD_DIR) && git checkout .
endef

define Package/usteer-ng
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Wireless
  DEPENDS:=+libubox +libubus +libblobmsg-json +libnl-tiny
  TITLE:=Usteer - Next Generation OpenWrt AP roaming assist daemon
  URL:=https://github.com/NilsRo/usteer-ng/
endef

define Package/usteer-ng/conffiles
/etc/config/usteer
endef

define Package/usteer-ng/install
	$(INSTALL_DIR) $(1)/sbin $(1)/etc/init.d $(1)/etc/config
	$(CP) ./files/* $(1)/
	$(CP) $(PKG_BUILD_DIR)/usteerd $(1)/sbin/
endef

$(eval $(call BuildPackage,usteer-ng))
