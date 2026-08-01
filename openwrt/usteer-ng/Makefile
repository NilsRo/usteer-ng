include $(TOPDIR)/rules.mk

PKG_NAME:=usteer-ng
PKG_SOURCE_PROTO:=git

# Resolve the repository root dynamically so this also works when the checkout layout differs.
PKG_GIT_REPO_ROOT:=$(shell git -C $(CURDIR) rev-parse --show-toplevel 2>/dev/null || true)
# Local dev build is explicitly enabled by setting BUILD_HEAD=1.
PKG_IS_BUILD_HEAD:=$(if $(BUILD_HEAD),1,0)

# Diagnostic output to help CI debugging — make prints these at parse time.
$(info ===== usteer-ng package Makefile diagnostics =====)
$(info PKG_GIT_REPO_ROOT='$(PKG_GIT_REPO_ROOT)')
$(info PKG_IS_BUILD_HEAD='$(PKG_IS_BUILD_HEAD)')
$(info ================================================)

# Get the latest tag
PKG_LATEST_TAG:=$(shell git -C $(PKG_GIT_REPO_ROOT) tag --sort=-creatordate | head -n 1 2>/dev/null || echo "0.0.0")

ifeq ($(PKG_IS_BUILD_HEAD),1)
  # For local development builds, use the local repository as the source URL
  # and the latest commit hash
  PKG_SOURCE_URL:=$(PKG_GIT_REPO_ROOT)
  PKG_SOURCE_COMMIT:=$(shell git -C $(PKG_GIT_REPO_ROOT) rev-parse HEAD 2>/dev/null || true)
  PKG_COMMIT:=$(shell git -C $(PKG_GIT_REPO_ROOT) rev-parse HEAD 2>/dev/null || true)
  PKG_COMMIT_HASH:=$(shell git -C $(PKG_GIT_REPO_ROOT) rev-parse --short $(PKG_COMMIT) 2>/dev/null || true)

  PKG_RELEASE:=$(PKG_COMMIT_HASH)
else
  # For release builds, use the remote repository URL and the latest tag
  # Use the commit hash corresponding to the tag
  PKG_SOURCE_URL:=https://github.com/NilsRo/usteer-ng.git
  PKG_SOURCE_COMMIT:=$(PKG_LATEST_TAG)
  PKG_COMMIT:=$(shell git -C $(PKG_GIT_REPO_ROOT) rev-list -n 1 $(PKG_LATEST_TAG) 2>/dev/null || true)
  PKG_COMMIT_HASH:=$(shell git -C $(PKG_GIT_REPO_ROOT) rev-parse --short $(PKG_COMMIT) 2>/dev/null || true)
#   PKG_RELEASE:=1
endif
PKG_VERSION:=$(PKG_LATEST_TAG)
PKG_SOURCE_VERSION:=$(PKG_VERSION)
PKG_HASH:=skip
PKG_SOURCE_HASH:=skip
PKG_MIRROR_HASH:=skip

$(info ===== usteer-ng package version diagnostics =====)
$(info PKG_SOURCE_VERSION='$(PKG_SOURCE_VERSION)')
$(info PKG_VERSION='$(PKG_VERSION)')
$(info PKG_RELEASE='$(PKG_RELEASE)')
$(info ================================================)

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
  PROVIDES:=usteer $(PKG_NAME)
  REPLACES:=usteer
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
