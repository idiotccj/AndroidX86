AndroidX86
==========

## 製作可直接開機的Android硬碟

1. 安裝Android x86到virtual box
2. 關機
3. 開啟開發環境的Ubuntu
4. Copy別人的AndroidX86 裡面的 1.System 2./grub 3.kernel

```
$ mkdir copyFiles
$ mkdir mountTemp

$ sudo mount /dev/sdb1 /mountTemp
$ cd mountTemp/
$ ls
android-4.2-test  grub  lost+found

$ cd android-4.2-test/
$ cp -pa initrd.img ~/copyFiles/
$ cp -pa kernel ~/copyFiles/
$ cp -pa ramdisk.img ~/copyFiles/
$ cd system
$ sudo tar cz * > ~/copyFiles/system.tar.gz
$ cd ..
$ cp -pa grub/ ~/copyFiles/

$ sudo umount /dev/sdb1
```

```
$ sudo fdisk /dev/sdb
```
命令 (m 以獲得說明)：

# 先把原本的硬碟格式化
命令 (m 以獲得說明)： d
已選分割區 1

# 開始規劃4個partitions
sdb1 50M
sdb2 550M
sdb3 200M
sdb4 200M

所用裝置 開機      開始         結束      區塊   識別號  系統
/dev/sdb1   *        2048      104447       51200   83  Linux
/dev/sdb2          104448     1230847      563200   83  Linux
/dev/sdb3         1230848     1640447      204800   83  Linux
/dev/sdb4         1640448     2050047      204800   83  Linux

最後記得要 w 離開

# 格式化
```
$ sudo mkfs.ext2 /dev/sdb1
$ sudo mkfs.ext4 /dev/sdb2
$ sudo mkfs.ext2 /dev/sdb3
$ sudo mkfs.ext2 /dev/sdb4
```

# Tune
```
$ sudo tune2fs -i 2048 /dev/sdb1
tune2fs 1.41.14 (22-Dec-2010)
Setting interval between checks to 176947200 seconds
$ sudo tune2fs -i 2048 /dev/sdb2
tune2fs 1.41.14 (22-Dec-2010)
Setting interval between checks to 176947200 seconds
$ sudo tune2fs -i 2048 /dev/sdb3
tune2fs 1.41.14 (22-Dec-2010)
Setting interval between checks to 176947200 seconds
$ sudo tune2fs -i 2048 /dev/sdb4
tune2fs 1.41.14 (22-Dec-2010)
Setting interval between checks to 176947200 seconds
```

# file check
```
$ sudo e2fsck -DC0 /dev/sdb1
e2fsck 1.41.14 (22-Dec-2010)
/dev/sdb1: clean, 11/12824 files, 2436/51200 blocks
$ sudo e2fsck -DC0 /dev/sdb2
e2fsck 1.41.14 (22-Dec-2010)
/dev/sdb2: clean, 11/35200 files, 6420/140800 blocks
$ sudo e2fsck -DC0 /dev/sdb3
e2fsck 1.41.14 (22-Dec-2010)
/dev/sdb3: clean, 11/51200 files, 8013/204800 blocks
$ sudo e2fsck -DC0 /dev/sdb4
e2fsck 1.41.14 (22-Dec-2010)
/dev/sdb4: clean, 11/51200 files, 8013/204800 blocks
```

5. 


### 下午
## initrd.img 解析
要把 initrd.img 裡面的busybox 重新 build 到 ramdisk.img
```
$ make menuconfig
```
 然後選單打開(1)init (2)ash(shell language)
 
# 要把kernel裡面的設定 init=/init 拿掉

# 接下來執行 會出現錯誤：找不到init process
原因是busybox 的 init process找不到shared library
所以要去看他需要哪些shared library --> 利用ldd
上課的解法：打開Build BusyBox as a static binary
然後重新make install 重新開機就可以了


