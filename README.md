# ScaleSwap: A Scalable OS Swap System for All-Flash Swap Arrays

**Kernel compile**
```
$ sudo su
# cd  ScaleSwap/linux-6.6.8/
# make menuconfig
# make -j $(nproc)
# make -j $(nproc) INSTALL_MOD_STRIP=1 modules_install
# make -j $(nproc) install
# reboot
```

**Module comile**
```
$ sudo su
# cd  ScaleSwap/module_swap/
# make -j $(nproc)
```

**setting All Flash Swap Arrays**

```
$ sudo su
# mdadm --stop /dev/md127
# cd ScaleSwap/scripts
# ./check_model (we only use 8 FireCuda)
# mdadm --create /dev/md127 --raid-devices=8 --level=0 /dev/nvme{}n1 (fill 8 FireCuda's nvme number)
# mkfs -t ext4 -E lazy_itable_init=0,lazy_journal_init=0 -O ^has_journal /dev/md127
# mount /dev/md127 /mnt/test
# ./mkmulswap.sh 128 (# of cores)
```

**Swap on**
```
$ sudo su
# cd ScaleSwap/module_swap/
#. /insert_mod.sh
# mount /dev/md127 /mnt/test
# cd ScaleSwap/scripts
# ./mkmulswap.sh 128 (# of cores)
```

**Swap off**
```
$ sudo su
# swapoff -a
# cd ScaleSwap/module_swap
# ./remove_mod.sh
# umount /mnt/test
```
