### ����initramfs�ļ�
ʹ�ô�initramfs.img�ļ���ѹ������Ŀ¼���´������initramfs
`find .|cpio -o -H newc|gzip > ~/myinitramfs.gz`

### ��ѹinitramfs.img�ļ�
```
cp initramfs.img initramfs.gz
gunzip initramfs.gz
cpio -i --make-directories < initramfs.img
```
