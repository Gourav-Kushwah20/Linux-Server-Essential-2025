# 📂 Disk Management in Linux

Disk management involves **viewing, partitioning, formatting, mounting, and monitoring disks**.

---

## 1. 🔍 Check Disk Information
- View all disks:
```bash
lsblk
```
- Detailed info:
```bash
sudo fdisk -l
```
- Disk usage:
```bash
df -h
```
- I/O statistics:
```bash
iostat
```

---

## 2. 🧩 Partition Management
- Open partition tool:
```bash
sudo fdisk /dev/sdX
```
(Replace `sdX` with your disk, e.g., `/dev/sda`)

- Inside `fdisk`:
  - `n` → ➕ Create new partition  
  - `d` → ❌ Delete partition  
  - `p` → 📋 Print partition table  
  - `w` → 💾 Write changes  

- For GPT disks:
```bash
sudo gdisk /dev/sdX
```

---

## 3. 💽 Format a Partition
- Ext4 filesystem:
```bash
sudo mkfs.ext4 /dev/sdX1
```
- XFS filesystem:
```bash
sudo mkfs.xfs /dev/sdX1
```
- FAT32:
```bash
sudo mkfs.vfat /dev/sdX1
```

---

## 4. 📂 Mounting & Unmounting
- Create mount point:
```bash
sudo mkdir /mnt/data
```
- Mount partition:
```bash
sudo mount /dev/sdX1 /mnt/data
```
- Unmount:
```bash
sudo umount /mnt/data
```
- Permanent mount (via `/etc/fstab`):
```bash
/dev/sdX1   /mnt/data   ext4   defaults   0 2
```

---

## 5. 🛠️ Check & Repair File Systems
- Ext4 filesystem check:
```bash
sudo fsck /dev/sdX1
```
- XFS filesystem check:
```bash
sudo xfs_repair /dev/sdX1
```

---

## 6. 📊 Disk Usage & Monitoring
- Human-readable usage:
```bash
du -sh /path/*
```
- Find large files:
```bash
find / -type f -size +500M
```
- Real-time monitoring:
```bash
iotop
```

---

## 7. 📦 LVM (Logical Volume Manager)
- Create physical volume:
```bash
sudo pvcreate /dev/sdX1
```
- Create volume group:
```bash
sudo vgcreate vg_data /dev/sdX1
```
- Create logical volume:
```bash
sudo lvcreate -L 20G -n lv_storage vg_data
```
- Format & mount LV:
```bash
sudo mkfs.ext4 /dev/vg_data/lv_storage
sudo mount /dev/vg_data/lv_storage /mnt/data
```

---

## 8. 🔄 Swap Management
- Create swap file:
```bash
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```
- Make permanent (edit `/etc/fstab`):

```
/swapfile none swap sw 0 0
```

---

⚡ **Pro Tip:** Always **backup your data** before partitioning or formatting.