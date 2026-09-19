include $(TOPDIR)/rules.mk

PKG_NAME:=pcie_mhi_nss
PKG_VERSION:=1.3.8
PKG_RELEASE:=5

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk

PKG_BUILD_PARALLEL:=1

define KernelPackage/pcie_mhi_nss
  SUBMENU:=WWAN Support
  TITLE:=Kernel PCIe driver for MHI device
  DEPENDS:=@(TARGET_qualcommax_ipq807x||TARGET_qualcommax_ipq50xx) \
	    @PCI_SUPPORT \
	    +kmod-rmnet-nss
  FILES:=$(PKG_BUILD_DIR)/pcie_mhi.ko
endef

define KernelPackage/pcie_mhi_nss/description
  Kernel module for register a custom pciemhi platform device.
endef

EXTRA_CFLAGS+= \
	-Wno-unused-function \
	-Wno-missing-prototypes \
	-Wno-missing-declarations

ifneq ($(CONFIG_PACKAGE_kmod-rmnet-nss),)
EXTRA_CFLAGS += -I$(STAGING_DIR)/usr/include/qca-nss-rmnet
EXTRA_CFLAGS += -DCONFIG_QCA_NSS_DRV
endif

define Build/Compile
	+$(KERNEL_MAKE) ccflags-y="$(EXTRA_CFLAGS)" M="$(PKG_BUILD_DIR)" \
		modules
endef

define KernelPackage/pcie_mhi_nss/install
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/pcie_mhi_nss.init $(1)/etc/init.d/pcie_mhi_nss
endef

$(eval $(call KernelPackage,pcie_mhi_nss))
