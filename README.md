## systemd-swap
Script for manage swap on:
* [zswap](https://www.kernel.org/doc/Documentation/vm/zswap.txt) - Enable/Configure
* [zram](https://www.kernel.org/doc/Documentation/blockdev/zram.txt) - Autoconfigurating for swap
* files - (sparse files for saving space, support btrfs)
* block devices - auto find and do swapon

It is configurable in /etc/systemd/swap.conf.

Additional terms:
* SwapFC (File Chunked) - provide a dynamic swap file allocation/deallocation

## Files placed:
```
/etc/systemd/swap.conf
/usr/lib/systemd/system/systemd-swap.service
/usr/bin/systemd-swap
```

## Please not forget to enable by:
```
# systemctl enable systemd-swap
```
## Install
* ![logo](http://www.monitorix.org/imgs/archlinux.png "arch logo")Arch: in the [community](https://www.archlinux.org/packages/community/any/systemd-swap/).
* Debian: use [package.sh](https://raw.githubusercontent.com/Nefelim4ag/systemd-swap/master/package.sh) in git repo
```
$ git clone https://github.com/Nefelim4ag/systemd-swap.git
$ ./systemd-swap/package.sh debian
$ sudo dpkg -i ././systemd-swap/systemd-swap-*any.deb
```
* Fedora: use [package.sh](https://raw.githubusercontent.com/Nefelim4ag/systemd-swap/master/package.sh) $
```
$ git clone https://github.com/Nefelim4ag/systemd-swap.git
$ ./systemd-swap/package.sh fedora f28
$ sudo dnf install ./systemd-swap/systemd-swap-*noarch.rpm
```
* Manual
```
$ git clone https://github.com/Nefelim4ag/systemd-swap.git
$ sudo make install
```

## About configuration
```
Q: WTF?! Why you merge swapFC and swapFU?
A: That simplify testing of swapFC code and make code more generic

Q: How can i migrate config swapFU from 3.X to 4.X?
A: Most of switches are same, to get configuration like swapFU from swapFC, set swapfc_max_count to 1 and swapfc_chunk_size to size of swapFU.

Q: Do we need to activate both zram and zswap?
A: Nope, it's useless, as zram is a compressed RAM DISK, but zswap is a compressed "writeback" CACHE on swap file/disk.

Q: Do i need use swapfc_force_use_loop on swapFC?
A: Nope, as you wish really, native swapfile must work faster and it's more safe in OOM condition in comparison to loop backed scenario.

Q: When would we want a certain configuration?
A: In most cases (Notebook, Desktop, Server) it's enough to enable zswap + swapfc (On server tuning of swapfc can be needed).
In case where SSD used, and you care about flash wear, use only ZRam.

Q: Where is the swap file located?
A: Read carefully swap.conf

Q: Can we use this to enable hibernation?
A: Nope as hibernation wants a persistent fs blocks and wants access to swap data directly from disk, this will not work on: zram, swapfu, swapfc (without some magic of course).
```
## Note:
* For zram support: Dependence: util-linux >= 2.26
* If you use zram not for only swap, use kernel 4.2+ or please add rule for modprobe like:
```
options zram max_devices=32
```
