---
description: 某一次不小心重啟，然後硬盤沒掛載上所以系統沒啟動
---

# 硬碟掛載

<pre><code><strong>fdisk -l
</strong></code></pre>

查看系統識別的硬碟

### 硬碟分區

```
fdisk /dev/sdb
```

對sdb這塊磁盤分區

m，不查看幫助，一路回車（分得是整塊盤），最後w保存！

```
mkfs.ext4 /dev/sdb1
```

### 格式化硬碟

格式化這塊磁盤為ext4格式

```
mkdir /data1
```

在根目錄下創建 data1這個文件夾

### 掛載硬碟

```
mount /dev/sdb1 /data1
```

使用mount命令將次塊硬碟掛在到data1文件夾下

### 將硬碟寫入開機啟動

```
lsblk -f
```

查看這塊硬碟的UUID

```
vim /etc/fstab
```

```
UUID=4787465c-dee7-4de6-bbde-3f00931b2fa3 /data1 ext4 defaults 0      0
```

整理成此格式  後面是掛載到跟目錄的文件夾。 然後是 硬盤分區格式

\
