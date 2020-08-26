u-boot修改：



主要看《制作荔枝派开发板TF卡启动学习_论坛_kk非常重要.pdf》里的uboot设置


(1)u-boot直接从tf卡启动：
要修改：include/configs/sun8i.h
加下边的代码：
//kk start
#define CONFIG_BOOTCOMMAND "setenv bootm_boot_mode sec;"\
                        "load mmc 0:1 0x41000000 zImage;"\
                        "load mmc 0:1 0x41800000 sun8i-v3s-licheepi-zero-dock.dtb;"\
                        "bootz 0x41000000 - 0x41800000;"
#define CONFIG_BOOTARGS "console=ttyS0,115200 panic=5 rootwait root=/dev/mmcblk0p2 earlyprintk rw vt.global_cursor_default=0" 

//kk end


(2)uboot开机图片
1）u-boot-3s-current/tools/Makefile文件：
endif # !LOGO_BMP

LOGO_BMP= $(srctree)/$(src)/logos/logokk256.bmp //加上
2）然后在u-boot-3s-current/tools/logos放入8位（256色）的logokk256.bmp图片，需要满屏显示：如800X480像素

3）在u-boot-3s-current/drivers/video/cfb_console.c加入下边定义：
//kk
#define CONFIG_HIDE_LOGO_VERSION
#define CONFIG_VIDEO_BMP_LOGO

1.首先生成.config
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- LicheePi_Zero_800x480LCD_defconfig

2.运行一次：menuconfig
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- menuconfig
3.编译
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf-
