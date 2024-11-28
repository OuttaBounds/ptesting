
Extract/repack SquashFS / JFFS2
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
#same with JFFS2
jefferson jffs2.bin
#create the file
#mkfs.jffs2 -lqn -e128 -s2048 -p0x10000000 -r jffs2_dir -o $JFFS2MOD.bin
mkfs.jffs2 -l -r jffs2_dir -o $JFFS2MOD.bin -e 0x10000 --pad=$PREV_SIZE
#or without compression and with no-clean marker
mkfs.jffs2 -l -r jffs2_dir -o $JFFS2MOD.bin --pad=$PREV_SIZE --pagesize=4096 --disable-compressor=zlib --no-cleanmarkers
#without compression
mkfs.jffs2 -l -r jffs2_dir -o $JFFS2MOD.bin -e 0x2000 -s 0x1000 --pad=$PREV_SIZE --disable-compressor=zlib
```
now modify and prepare image:

#create-fw #squashfs

```bash
mksquashfs squashfs-root modified-sqfs.bin -comp xz
#make sure the compression is the same one eg. xz or zlib
cp $FIRMWARE_BIN $NEW_FW_BIN
dd if=modified-sqfs.bin of=$NEW_FW_BIN bs=1 seek=$SQFS_OFFSET count=$SQFS_IMAGE_SIZE conv=notrunc
```