xsos - Instantly pull the most useful data about a system out of a sosreport
============================================================================

The goal of `xsos` is to make it easy to gather information about a system together in an easy-to-read-summary, whether that system is the localhost or a system for which you have an unpacked sosreport.

There is tons and tons of useful amazing instructive data available to normal users (not to mention root) on a Linux system, but by design, this information is spread out across myriad files and some of it practically requires specialized commands to parse through.

`xsos` makes it easy, parsing and calculating and formatting data from dozens of files to give you a detailed overview about a system.


INSTALLATION
-------

*"I'm sold"* you say. *"How do I install `xsos`?"* you say.  
Simply download [this one file](/ryran/xsos/master/xsos) and save it to a directory that's in your $PATH. You can even name the script whatever you want (like `x` or `z`). Make sure it's executable with a `chmod +x /PATH/TO/XSOS` and then run it with a `help` argument to get started!

Jump to ...

* [INSTALLATION](/ryran/xsos#installation)
* [EXAMPLES IN ACTION](/ryran/xsos#examples-in-action)
* [REQUIREMENTS](/ryran/xsos#requirements)
* [THINGS THAT MIGHT SURPRISE YOU](/ryran/xsos#things-that-might-surprise-you)
* [AUTHORS](/ryran/xsos#authors)
* [LICENSE](/ryran/xsos#license)


EXAMPLES IN ACTION
-------

The help page:

```
[rsaw@sawzall:~]$ xsos help
Usage: xsos [-xKMabocmdplenks] [SOSREPORT-ROOT]
  or:  xsos [--B FILE] [--C FILE] [--M FILE] [--D FILE] [--L FILE] [--N FILE]
  or:  xsos [-?|-h|--help]
Extract useful data about system from SOSREPORT-ROOT or else localhost

Display options:
 -x, --nocolor  disable coloring of output
 -K, --kb       show /proc/meminfo in KiB
 -M, --mb       show /proc/meminfo in MiB

Content options:
 -a, --all      show everything
 -b, --bios     show info from dmidecode
 -o, --os       show os/kernel release, hostname, uptime, loadavg
 -c, --cpu      show info from /proc/cpuinfo
 -m, --mem      show info from /proc/meminfo
 -d, --disks    show info from /proc/partitions
 -p, --mpath    show info from multipath
 -l, --lspci    show info from lspci
 -e, --ethtool  show info from ethtool
 -n, --net      show info from /proc/net/dev
 -k, --kern     show add'l /proc entries, e.g. stat, cmdline
 -s, --sysctl   show important kernel sysctls

Note that xsos argument parsing is performed by GNU getopt, so order of args
is not important (e.g. the SOSREPORT-ROOT could be the 1st argument).

If no content options are specified, xsos only shows some information,
equivalent to:

  xsos --os --cpu --mem --lspci
  
If SOSREPORT-ROOT isn't provided, the data will be gathered from the localhost;
however, bios, multipath, and ethtool output will only be displayed if running
as root (UID 0). When executing in this manner as non-root, those modules will
be silently skipped, even if explicitly requested.

Sometimes a full sosreport isn't available; sometimes you simply have a
dmidecode-dump or the contents of /proc/meminfo and you'd like a summary...

Special options:
 --B=FILE  FILE must contain dmidecode dump
 --C=FILE  FILE must contain /proc/cpuinfo dump
 --M=FILE  FILE must contain /proc/meminfo dump
 --D=FILE  FILE must contain /proc/partitions dump
 --L=FILE  FILE must contain lspci dump
 --N=FILE  FILE must contain /proc/net/dev dump

As is hopefully clear, each of these options requires a filename as an
argument. These options can be used together, but cannot be used in concert
with regular 'Content options' -- Content opts are ignored if Special options
are detected. Also note: the '=' can be replaced with a space if desired.
 
Version info: xsos v0.0.1 last mod 2012/08/26
Report bugs or suggestions to <rsaw@redhat.com>
Or see <github.com/ryran/xsos> for bug tracker & latest version
Alternatively, run xsos with '--update'
```

Run on a sosreport with no options:

```
[rsaw@sawzall:~]$ xsos aczx998pinkle/
OS
  Distro:   Red Hat Enterprise Linux Server release 5.8 (Tikanga)
  Kernel:   2.6.18-308.1.1.el5
  Hostname: aczx998pinkle
  Sys time: 12:27:51
  Uptime:   12 days, 1:37, 5 users
  LoadAvg:  1.23 (2%), 1.11 (2%), 0.77 (1%)
-------------------------------------------------------------------------------
CPUs
  64 logical cpus (ht,lm,pae,vmx)
  8 Intel Xeon CPU E7- 2830 @ 2.13GHz, 8 cores/ea
-------------------------------------------------------------------------------
MEMORY
  RAM:
    252.0g total [14.6g (6%) used]
    1.6g (1%) used excluding buffers/cache
    0.24g (0%) dirty
  HugePages:
    ZERO ram pre-allocated to HugePages
  LowMem/Slab/PageTables/Shmem:
    14.6g (6%) of 252.0g LowMem in-use
    0.58g (0%) of total ram used for Slab
    0.66g (0%) of total ram used for PageTables
  Swap:
    0.0g (0%) used of 32.0g total
-------------------------------------------------------------------------------
LSPCI
  Netdevs:
    8 NetXen Incorporated NX3031 Multifunction 1/10-Gigabit Server Adapter (rev 42) {2,4-port}
  Graphics:
    Advanced Micro Devices [AMD] nee ATI ES1000 (rev 02)
-------------------------------------------------------------------------------
```

Run on localhost with a couple options:

```
[rsaw@sawzall:~]$ sudo xsos --kern --ethtool
PROC/
  stat
    b[oot]time: Thu Aug 23 20:22:57 EDT 2012
    cpu [time distribution since last boot]:
      us 8%, ni 0%, sys 2%, idle 89%, iowait 1%, irq 0%, sftirq 0%, steal 0%
    procs_running: 1
    procs_blocked: 0
  cmdline
    BOOT_IMAGE=/vmlinuz-3.5.2-3.fc17.x86_64 root=/dev/mapper/vg_sawzall-root ro rd.lvm.lv=vg_sawzall/root rd.md=0 rd.dm=0 SYSFONT=True KEYTABLE=us rd.lvm.lv=vg_sawzall/swap rd.luks.uuid=luks-8adf0ec9-441a-4099-9b07-215904d0e431 LANG=en_US.UTF-8 rhgb quiet
-------------------------------------------------------------------------------
ETHTOOL
  em1         link=DOWN                        drv e1000e v2.0.0-k / fw 0.13-3
  tun0        link=UP 10Mb/s full (autoneg=N)  drv tun v1.6
  virbr0      link=DOWN                        drv bridge v2.3 / fw N/A
  virbr0-nic  link=DOWN                        drv tun v1.6
  wlan0       link=UP                          drv iwlwifi v3.5.2-3.fc17.x86_64 / fw 9.221.4.1
-------------------------------------------------------------------------------
```

Run on another sosreport with some options:

```
[rsaw@sawzall:~]$ xsos -p 8308201prodserv -mM 
MEMORY
  RAM:
    16051m total [15691m (97.8%) used]
    11955m (74.5%) used excluding buffers/cache
    45m (0.3%) dirty
  HugePages:
    8704m pre-allocated to HugePages (54.2% of total ram)
    1862m of HugePages (21.4%) in-use by applications
  LowMem/Slab/PageTables/Shmem:
    15691m (97.8%) of 16051m LowMem in-use
    101m (0.6%) of total ram used for Slab
    60m (0.4%) of total ram used for PageTables
  Swap:
    349m (8.3%) used of 4192m total
-------------------------------------------------------------------------------
STORAGE
  Multipath:
    archive    2.0 T
    dbbackup   850 G
    dgspool    20 G
    miscspool  100 G
    proddb     200 G
    prodstage  100 G
    produpgdb  150 G
    reportdb   30 G
    tsttrndev  150 G
  Disks:
    (Multipath and/or software raid components hidden)
    1 disks, totaling 272 GiB (0.27 TiB)
    sda   271.9 G
-------------------------------------------------------------------------------
```

Run on localhost again, showing sysctls

```
[rsaw@sawzall:~]$ xsos --sysctl
SYSCTLS
  kernel.
    osrelease: 3.5.2-3.fc17.x86_64
    tainted: 512 
      Taint on warning
    random.boot_id: 91a39257-fe26-4ae3-a9e7-e7b30a4bc083
    random.entropy_avail: 2984
    panic (seconds til reboot): 0
    hung_task_panic: 
    panic_on_oops: 0
    panic_on_unrecovered_nmi: 0
    sysrq: 0
    sem: 250 32000 32 128
      max semaphores per array = 250
      max sems system-wide     = 32000
      max ops per semop call   = 32
      max number of arrays     = 128
    shmall (pages): 2097152 (8192.0 G max total shared memory)
    shmmax (bytes): 33554432 (0.03 G max segment size)
    shmmni (segments): 4096 (max number of segs)
  vm.
    nr_hugepages: 0
    overcommit_{memory,ratio}: 0 / 50
    panic_on_oom: 0
  net.
    ipv4.icmp_echo_ignore_all: 0
    ipv4.ip_forward: 1
    ipv4.tcp_max_orphans (sockets): 262144 (16384 M @ max 64 K per orphan)
    ipv4.tcp_mem (pages): 91380 121842 182760 (356.95 G, 475.95 G, 713.91 G)
    ipv4.udp_mem (pages): 93756 125009 187512 (366.23 G, 488.32 G, 732.47 G)
    ipv4.tcp_rmem (bytes): 4096 87380 6291456 (4 K, 85 K, 6144 K)
    ipv4.tcp_wmem (bytes): 4096 16384 4194304 (4 K, 16 K, 4096 K)
    ipv4.udp_rmem_min (bytes): 4096 (4 K)
    ipv4.udp_wmem_min (bytes): 4096 (4 K)
-------------------------------------------------------------------------------
```

Finally, here's example of using one of the Special options, which allow you to specify a specific file instead of a full sosreport or having `xsos` run on your local system:

```
[rsaw@sawzall:~]$ xsos --B /tmp/dmidecode.txt
BIOS
  System:
    HP ProLiant DL980 G7, version Not Specified
  CPUs:
    Intel Xeon @ 2133 MHz (max supported CPU frequency 4800 MHz)
    version  Intel(R) Xeon(R) CPU E7- 2830 @ 2.13GHz        
    8 of 8 CPU sockets populated, 8 cores/16 threads per CPU
    64 total cores, 128 total threads
  Memory:
    262144 MB (256 GB) total
    32 of 128 DIMMs populated (system max capacity 512 GB)
-------------------------------------------------------------------------------
```


REQUIREMENTS
-------

* Nothing special for command requirements -- `xsos` needs standard coreutils & util-linux commands, along with, of course ... `gawk` and `sed`
* The script doesn't use absolute paths for cmd names so no problems with Fedora
* The script will gracefully report as much of what's requested as possible when running as non-root


THINGS THAT MIGHT SURPRISE YOU
-------

* The script does some pretty intensive color-formatting to make the output more easily-readable.
* The script can update itself via the internet in 10 seconds if run with `--update`.
* When printing disk info with `-d/--disks`, the script automatically detects linux software raid (md) devices and hides their components.
* When run with `-m/--mpath`, the script consults the `multipath` command to print info about native multipathd devices. If using this option in concert with `-d/--disks`, the script also detects all multipath device slave paths and hides those device nodes from the disk output.
* When printing info on pci net devices (`-l/--lspci`), `xsos` simplifies the output in an intelligent way. Example:

```
[rsaw@server]$ lspci | grep Eth
03:00.0 Ethernet controller: Broadcom Corporation NetXtreme II BCM5708 Gigabit Ethernet (rev 12)
05:00.0 Ethernet controller: Broadcom Corporation NetXtreme II BCM5708 Gigabit Ethernet (rev 12)
0b:00.0 Ethernet controller: Intel Corporation 82571EB Gigabit Ethernet Controller (rev 06)
0b:00.1 Ethernet controller: Intel Corporation 82571EB Gigabit Ethernet Controller (rev 06)
0e:00.0 Ethernet controller: Intel Corporation 82571EB Gigabit Ethernet Controller (rev 06)
0e:00.1 Ethernet controller: Intel Corporation 82571EB Gigabit Ethernet Controller (rev 06)

[rsaw@server]$ xsos -l
LSPCI
  Netdevs:
    2 Broadcom Corporation NetXtreme II BCM5708 Gigabit Ethernet (rev 12) {2,1-port}
    4 Intel Corporation 82571EB Gigabit Ethernet Controller (rev 06) {2,2-port}
  Graphics:
    Advanced Micro Devices [AMD] nee ATI ES1000 (rev 02)
```


AUTHORS
-------

As far as direct contributions go, so far it's just me, [ryran](/ryran), aka rsaw, aka [Ryan Sawhill](http://b19.org).

However, people rarely accomplish things in a vacuum... I am very thankful to StackOverflow and a couple prolific users over there. [Dennis Williamson](http://stackoverflow.com/users/26428/dennis-williamson) and [ghostdog74](http://stackoverflow.com/users/131527/ghostdog74) have both offered answers containing pieces of code that were extremely instructive in helping me to advance my `awk` career.

Please contact me if you have ideas, suggestions, questions, or want to collaborate on this or something similar. For specific bugs and feature-requests, you can [post a new issue on the tracker](/ryran/xsos/issues).


LICENSE
-------

Copyright (C) 2012 [Ryan Sawhill](http://b19.org)

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU
General Public License @[gnu.org/licenses/gpl.html](http://gnu.org/licenses/gpl.html>) for more details.

