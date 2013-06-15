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
$ cd ..
$ cp -pa grub/ ~/copyFiles/


cp android-4.2-test/kernel ~/android-x86
```

```
cp -pa grub/ ~/android-x86
cp android-4.2-test/kernel ~/android-x86
```

5. 


### 下午
## initrd.img 解析
要把 initrd.img 裡面的busybox 重新 build 到 ramdisk.img
```
$ make menuconfig
```
 然後選單打開(1)init (2)ash(shell language)
 

