# mymenuos
内核启动完成后进入menu程序（类似Linux平台，使用自己的根文件系统），支持5个命令help,version,time,quit,pid-asm

使用自己的Linux系统环境搭建MenuOS的过程(我使用的是 ubuntu14.04-server)

# 下载内核源代码编译内核

cd ~/LinuxKernel/

wget https://www.kernel.org/pub/linux/kernel/v3.x/linux-3.18.6.tar.xz

xz -d linux-3.18.6.tar.xz

tar -xvf linux-3.18.6.tar

cd linux-3.18.6

make i386_defconfig

make # 一般要编译很长时间，少则20分钟多则数小时
 
# 制作根文件系统

cd ~/LinuxKernel/

mkdir rootfs

git clone https://github.com/mengning/menu.git  # 如果被墙，可以使用附件menu.zip 

cd menu

gcc -o init linktable.c menu.c test.c -m32 -static –lpthread

cd ../rootfs

cp ../menu/init ./

find . | cpio -o -Hnewc |gzip -9 > ../rootfs.img
 
# 启动MenuOS系统

cd ~/LinuxKernel/

qemu -kernel linux-3.18.6/arch/x86/boot/bzImage -initrd rootfs.img

进入我们自己搭建的menuos系统后 大家就可以使用命令行模拟Linux了～～
