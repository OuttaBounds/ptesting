
Extract SquashFS
---
#binwalk #fs-extract #squashfs

```bash
binwalk $FIRMWARE_BIN
```

from output: copy offset in fw file ($SQFS_OFFSET)
copy image file size ($SQFS_IMAGE_SIZE)

```bash
dd if=$FIRMWARE_BIN of=squashfs.bin bs=1 skip=$SQFS_OFFSET count=$SQFS_IMAGE_SIZE
#sanity check
file squashfs.bin
unsquashfs squashfs.bin
cd squashfs-root
#same with JFFS
jefferson jffs.bin
```
now modify and prepare image:

#create-fw #squashfs

```bash
mksquashfs squashfs-root modified-sqfs.bin -comp xz
#make sure the compression is the same one eg. xz or zlib
cp $FIRMWARE_BIN $NEW_FW_BIN
dd if=modified-sqfs.bin of=$NEW_FW_BIN bs=1 seek=$SQFS_OFFSET count=$SQFS_IMAGE_SIZE conv=notrunc
```