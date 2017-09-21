### 制作initramfs文件
使用从initramfs.img文件解压出来的目录重新打包制作initramfs
`find .|cpio -o -H newc|gzip > ~/myinitramfs.gz`

### 解压initramfs.img文件
```
cp initramfs.img initramfs.gz
gunzip initramfs.gz
cpio -i --make-directories < initramfs.img
```
