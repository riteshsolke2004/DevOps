# 🐧 Linux Volume Management (LVM) Cheat Sheet

## 🔹 What is LVM?
LVM = **Logical Volume Manager**  
It provides a flexible way to manage storage in Linux. Unlike fixed partitions, LVM allows you to:
- Resize volumes easily
- Combine multiple disks
- Add/remove storage dynamically
- Take snapshots for backup

---

## 🔹 Key Components
- **PV (Physical Volume)** → Actual disk/partition initialized for LVM (e.g., `/dev/sdb1`)
- **VG (Volume Group)** → Pool of storage made from PVs
- **LV (Logical Volume)** → Usable partition created from VG
- **PE (Physical Extent)** → Smallest unit of allocation (default: 4MB)

📌 Flow: **PV → VG → LV → File System**

---

## 🔹 Basic LVM Setup

### 1. Create Physical Volume
```bash
pvcreate /dev/sdb /dev/sdc


2. Create Volume Group
vgcreate my_vg /dev/sdb /dev/sdc

3. Create Logical Volume
lvcreate -L 10G -n my_lv my_vg

4. Format and Mount
mkfs.ext4 /dev/my_vg/my_lv
mkdir /mnt/mydata
mount /dev/my_vg/my_lv /mnt/mydata

🔹 Useful Commands
🔸 Display Information
pvdisplay     # Show physical volumes
vgdisplay     # Show volume groups
lvdisplay     # Show logical volumes

🔸 Extend Volume
lvextend -L +5G /dev/my_vg/my_lv
resize2fs /dev/my_vg/my_lv   # For ext4 filesystem

🔸 Reduce Volume (⚠ risky, backup first!)
resize2fs /dev/my_vg/my_lv 5G
lvreduce -L 5G /dev/my_vg/my_lv

🔸 Remove LVM
lvremove /dev/my_vg/my_lv
vgremove my_vg
pvremove /dev/sdb

🔸 Snapshot
lvcreate -L 1G -s -n snap1 /dev/my_vg/my_lv
