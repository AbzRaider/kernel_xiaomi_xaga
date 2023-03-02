#
# Copyright (C) 2015 MediaTek Inc.
#
# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License version 2 as
# published by the Free Software Foundation.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
# GNU General Public License for more details.
#

# Connectivity combo driver
# If KERNELRELEASE is defined, we've been invoked from the
# kernel build system and can use its language.
ifneq ($(KERNELRELEASE),)
    subdir-ccflags-y += -I$(srctree)/
    subdir-ccflags-y += -I$(srctree)/drivers/misc/mediatek/base/power/include
    subdir-ccflags-y += -I$(srctree)/drivers/misc/mediatek/clkbuf/src
    subdir-ccflags-y += -Werror -I$(srctree)/drivers/misc/mediatek/include/mt-plat
    subdir-ccflags-y += -I$(srctree)/drivers/mmc/core
    subdir-ccflags-y += -I$(srctree)/drivers/misc/mediatek/eccci/
    subdir-ccflags-y += -I$(srctree)/drivers/clk/mediatek/
    subdir-ccflags-y += -I$(srctree)/drivers/pinctrl/mediatek/
    subdir-ccflags-y += -I$(srctree)/drivers/misc/mediatek/power_throttling/

    #ifneq ($(CONFIG_MTK_TINYSYS_SCP_SUPPORT),)
        # SCP
    #    subdir-ccflags-y += -Werror -I$(srctree)/drivers/misc/mediatek/scp/include
    #    subdir-ccflags-y += -Werror -I$(srctree)/drivers/misc/mediatek/connectivity/conap_scp

        # Aoltest
    #    ifeq ($(CONFIG_MTK_AOL_DEBUG),y)
    #        subdir-ccflags-y += -Werror -I$(srctree)/drivers/misc/mediatek/connectivity/aoltest/
    #    endif
    #endif

    # Do Nothing, move to standalone repo
    MODULE_NAME := connadp
    obj-$(CONFIG_MTK_COMBO) += $(MODULE_NAME).o

    $(MODULE_NAME)-objs += common/connectivity_build_in_adapter.o
    $(MODULE_NAME)-objs += common/wmt_build_in_adapter.o
    $(MODULE_NAME)-objs += power_throttling/adapter.o
    $(MODULE_NAME)-objs += power_throttling/core.o
    ifeq ($(CONFIG_CONN_PWR_DEBUG),y)
        $(MODULE_NAME)-objs += power_throttling/test.o
    endif

    #ifneq ($(CONFIG_MTK_TINYSYS_SCP_SUPPORT),)
        ###########################################################################
        # SCP
    #    $(MODULE_NAME)-objs += conap_scp/msg_thread.o
    #    $(MODULE_NAME)-objs += conap_scp/ring_buffer.o
    #    $(MODULE_NAME)-objs += conap_scp/conap_scp_core.o
    #    $(MODULE_NAME)-objs += conap_scp/conap_scp_ipi.o
    #    $(MODULE_NAME)-objs += conap_scp/conap_platform_data.o
        #$(MODULE_NAME)-objs += conap_scp/conap_scp_test.o
        ###########################################################################
        # Aoltest
    #    ifeq ($(CONFIG_MTK_AOL_DEBUG),y)
    #        ccflags-y += -D AOLTEST_SUPPORT
    #        $(MODULE_NAME)-objs += aoltest/aoltest_core.o
    #        $(MODULE_NAME)-objs += aoltest/aoltest_netlink.o
    #        $(MODULE_NAME)-objs += aoltest/aoltest_ring_buffer.o
    #    endif
    #endif

    ifeq ($(CONFIG_MTK_COMBO), y)
        ccflags-y += -D CFG_CONNADP_BUILD_IN
    endif

    # for gen4m options
    # Edit for 6895
    export CONFIG_MTK_COMBO_WIFI_HIF=axi
    export MTK_COMBO_CHIP=CONNAC2X2_SOC7_0
    export BT_PLATFORM=connac2x
    export WLAN_CHIP_ID=6895
    export MTK_ANDROID_WMT=y
    export MTK_ANDROID_EMI=y
    export WIFI_IP_SET=1
    export MTK_WLAN_SERVICE=yes
    export CONNAC_VER=2_0
    export CFG_BUILD_CONNAC2=true

    # Do build-in for Makefile checking
    # export CONFIG_WLAN_DRV_BUILD_IN=y

    ifeq ($(CONFIG_WLAN_DRV_BUILD_IN),y)
     # Do build-in for xxx.c checking
        subdir-ccflags-y += -D MTK_WCN_REMOVE_KERNEL_MODULE
        subdir-ccflags-y += -D MTK_WCN_BUILT_IN_DRIVER
        obj-y += wmt_drv/
        obj-y += wmt_chrdev_wifi/
        obj-y += wlan_drv_gen4m/
    endif

    obj-$(CONFIG_MTK_COMBO) += common/
    obj-$(CONFIG_MTK_COMBO_WIFI) += wlan/adaptor/
    obj-$(CONFIG_MTK_COMBO_CHIP_CONSYS_6895) += wlan/core/gen4m/
    obj-$(CONFIG_MTK_BTIF) += bt/mt66xx/wmt/
    obj-$(CONFIG_MTK_COMBO_GPS) += gps/gps_pwr/
    obj-$(CONFIG_MTK_COMBO_GPS) += gps/gps_stp/
    obj-$(CONFIG_MTK_FMRADIO) += fmradio/
    obj-$(CONFIG_MTK_COMBO) += connfem/

# Otherwise we were called directly from the command line;
# invoke the kernel build system.
else
    KERNELDIR ?= /lib/modules/$(shell uname -r)/build
    PWD  := $(shell pwd)
default:
	$(MAKE) -C $(KERNELDIR) M=$(PWD) modules
endif
