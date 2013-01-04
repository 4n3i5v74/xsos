xsos & rsar - Summarize system info from sosreports
===================================================

See the **[rsar README](/ryran/xsos/blob/master/RSAR_README.md)** for details on `rsar`.

The goal of `xsos` is to make it easy to instantaneously gather information about a system together in an easy-to-read-summary, whether that system is the localhost on which xsos is being run or a system for which you have an unpacked sosreport.

There is tons of useful amazing instructive data available to normal users (not to mention root) on a Linux system, but by design, this information is spread out across myriad files. Some of it even requires commands to parse through.

`xsos` will attempt to make it easy, parsing and calculating and formatting data from dozens of files (and commands) to give you a detailed overview about a system, or -- if requested -- `xsos` will only parse one file (e.g. with `--mem` or `--cpu`) or output from one command (e.g. with `--ip` or `--ps`).

New features are being added all the time -- see [the tracker](/ryran/xsos/issues) to have a look at some of the things that are already in line to be worked on.

**[Why another tool? Why not add stuff to `sxconsole`?](https://github.com/ryran/xsos/issues/9)**


INSTALLATION
-------

*"I'm sold! How do I install xsos?"*

Simply download [this one file](https://raw.github.com/ryran/xsos/master/xsos) and save it to a directory that's in your `$PATH`. Make it executable and you'll be good to go!  

**Example:**

```
$ su -
# wget -O /usr/local/bin/xsos bit.ly/xsos-direct
# chmod +x /usr/local/bin/xsos
# exit
$ xsos -h
```

**Jump to ...**

* [INSTALLATION](/ryran/xsos#installation)
* [EXAMPLES IN ACTION](/ryran/xsos#examples-in-action)
* [REQUIREMENTS](/ryran/xsos#requirements)
* [THINGS THAT MIGHT SURPRISE YOU](/ryran/xsos#things-that-might-surprise-you)
* [AUTHORS](/ryran/xsos#authors)
* [LICENSE](/ryran/xsos#license)


EXAMPLES IN ACTION
-------
The lovely thing that isn't going to be captured here is all the coloring done to make things easier to read. There's actually quite a bit of color-logic to warn of various conditions...

**Run on a sosreport with no options:**

```
[rsaw]$ xsos aczx998pinkle/
OS
  Hostname: aczx998pinkle
  Distro:   [redhat-release] Red Hat Enterprise Linux Server release 5.5 (Carthage)
            [enterprise-release] Enterprise Linux Enterprise Linux Server release 5.5 (Carthage)
  RHN:      serverURL=https://linux-update.oracle.com/XMLRPC
  Runlevel: N 3  (default 3)
  SELinux:  permissive  (default enforcing)
  Arch:     mach=x86_64  cpu=x86_64  platform=x86_64
  Kernel:
    Booted kernel:  2.6.18-238.12.2.0.2.el5
    GRUB default:   2.6.18-238.12.2.0.2.el5
    Build version:
      Linux version 2.6.18-238.12.2.0.2.el5 (mockbuild@ca-build9.us.oracle.com) (gcc version 4.1.2 
      20080704 (Red Hat 4.1.2-50)) #1 SMP Tue Jun 28 05:21:19 EDT 2011
    Booted kernel cmdline:
      root=/dev/md6 ro bootarea=c0d0 loglevel=7 panic=60 debug rhgb numa=off console=ttyS0,115200n8 
      console=tty1 crashkernel=128M@16M bootfrom=CELLBOOT audit=1 processor.max_cstate=1 nomce
    GRUB default kernel cmdline:
      root=/dev/md6 ro bootarea=c0d0 loglevel=7 panic=60 debug rhgb numa=off console=ttyS0,115200n8 
      console=tty1 crashkernel=128M@16M bootfrom=BOOT audit=1 processor.max_cstate=1 nomce
    Kernel taint-check: 536870912 512 64 16 1
      Technology Preview code is loaded
      Taint on warning
      Userspace-defined naughtiness
      System experienced a machine check exception
      Proprietary module has been loaded
    - - - - - - - - - - - - - - - - - - -
  Sys time:  Mon Oct 29 10:55:02 CDT 2012
  Boot time: Sat Apr 28 03:29:56 CDT 2012  (1335583796)
  Uptime:    184 days, 12:25,  1 user
  LoadAvg:   [24 CPU] 2.34 (10%), 1.27 (5%), 0.95 (4%)
  /proc/stat:
    procs_running: 4    procs_blocked: 1    processes: 248052571
    cpu:  [Break-down of CPU time since boot]
      us 1%, ni 0%, sys 1%, idle 96%, iowait 2%, irq 0%, sftirq 0%, steal 0%
```


**While xsos is always changing, here's the minimal help page from v0.1.0rc5:**

```
[rsaw]$ xsos -h
Usage: xsos [DISPLAY OPTIONS] [-abocmdleinsp] [SOSREPORT ROOT]
  or:  xsos [DISPLAY OPTIONS] {--B|--C|--M|--D|--L|--I|--N|--P FILE}...
  or:  xsos [-?|-h|--help]
  or:  xsos [-U|--update]

Display system info from localhost or extracted sosreport

Content options:
 -a, --all      show everything
 -b, --bios     show info from dmidecode
 -o, --os       show release, hostname, uptime, loadavg, cmdline
 -c, --cpu      show info from /proc/cpuinfo
 -m, --mem      show info from /proc/meminfo
 -d, --disks    show info from /proc/partitions + dm-multipath
 -l, --lspci    show info from lspci
 -e, --ethtool  show info from ethtool
 -i, --ip       show info from ip addr (BASH v4+ required)
 -n, --net      show info from /proc/net/dev
 -s, --sysctl   show important kernel sysctls
 -p, --ps       inspect running processes via ps

Display options:
 -u, --unit=P       change byte display for /proc/meminfo & /proc/net/dev,
                    where P is "b" for byte, or else "k", "m", "g", or "t"
 -v, --verbose=NUM  specify ps verbosity level (0-4, default: 1)
 -w, --width=NUM    change fold-width (w autodetects, 0 disables, default: 94)
 -x, --nocolor      disable output colorization
 -y, --less         send output to `less -SR`
 -z, --more         send output to `more`

Special options (BASH v4+ required):
 --B=FILE  read from FILE containing `dmidecode` dump
 --C=FILE  read from FILE containing /proc/cpuinfo dump
 --M=FILE  read from FILE containing /proc/meminfo dump
 --D=FILE  read from FILE containing /proc/partitions dump
 --L=FILE  read from FILE containing `lspci` dump
 --I=FILE  read from FILE containing `ip addr` dump
 --N=FILE  read from FILE containing /proc/net/dev dump
 --P=FILE  read from FILE containing `ps aux` dump

Run with "--help" to see full help page

Version info: xsos v0.1.0rc5c last mod 2012/12/27
See <github.com/ryran/xsos> to report bugs or suggestions
```


**Here it is run on my laptop as root, with specific options:**

```
[rsaw]$ sudo xsos --bios --ip --lspci --ethtool --net --ps  # would be same as 'xsos -bilenp'
DMIDECODE
  BIOS:
    LENOVO, version 8CET50WW (1.30 ), 11/01/2011
  System:
    Mfr:  LENOVO
    Prod: 4174AQ5
    Vers: ThinkPad T420s
    Ser:  R9XXXXXXXX
    UUID: 56EABD01-XXXX-11CB-9DE6-XXXXXXXXXX
  CPU:
    1 of 1 CPU sockets populated, 2 cores/4 threads per CPU
    2 total cores, 4 total threads
    Mfr:  Intel(R) Corporation
    Fam:  Core i5
    Freq: 2600 MHz
    Vers: Intel(R) Core(TM) i5-2540M CPU @ 2.60GHz
  Memory:
    8192 MB (8 GB) total
    2 of 4 DIMMs populated (system max capacity 16 GB)

LSPCI
  Net/Storage:
    (1) Intel Corporation Centrino Ultimate-N 6300 (rev 3e)
    (1) Intel Corporation 82579LM Gigabit Network Connection (rev 04)
  VGA:
    Intel Corporation 2nd Generation Core Processor Family Integrated Graphics Controller (rev 09)

ETHTOOL
  em1         link=DOWN                        drv e1000e v2.0.0-k / fw 0.13-3
  tun0        link=up 10Mb/s full (autoneg=N)  drv tun v1.6
  virbr0      link=up                          drv bridge v2.3 / fw N/A
  virbr0-nic  link=DOWN                        drv tun v1.6
  vnet0       link=up 10Mb/s full (autoneg=N)  drv tun v1.6
  wlan0       link=up                          drv iwlwifi v3.6.10-2.fc17.x86_64 / fw 9.221.4.1

IP
  Interface   Slave Of  IPv4 Address        State  MAC Address
  =========   ========  ==================  =====  =================
  lo          -         127.0.0.1/8         up     -
  em1         -         -                   up     f0:de:f1:ba:cf:c4
  wlan0       -         10.7.7.111/24       up     24:77:03:41:41:18
                        192.168.17.17/32            
  virbr0      -         192.168.122.1/24    up     52:54:00:97:e3:ee
  virbr0-nic  -         -                   DOWN   52:54:00:97:e3:ee
  tun0        -         10.10.49.81/32      up     -
  vnet0       -         -                   up     fe:54:00:f9:3d:ec

NETDEV
  Interface   RxMiBytes  RxPackets  RxErrs  RxDrop  TxMiBytes  TxPackets  TxErrs  TxDrop
  =========   =========  =========  ======  ======  =========  =========  ======  ======
  em1         1729       1974 k     0       0       1729       705 k      0       0 
  tun0        9          12 k       0       0       9          9 k        0       0 
  virbr0      0          0 k        0       0       0          0 k        0       0 
  virbr0-nic  0          0 k        0       0       0          0 k        0       0 
  vnet0       0          0 k        0       0       0          0 k        0       0 
  wlan0       194        194 k      0       0       194        140 k      0       0 

PS CHECK
  Top users of CPU & MEM: 
    USER  %CPU   %MEM   RSS 
    rsaw  34.1%  46.0%  3.81 GiB
    qemu  27.4%  4.9%   0.38 GiB
    root  2.7%   1.0%   0.20 GiB
  Uninteruptible sleep & Defunct processes: 
    USER     PID    %CPU  %MEM  VSZ-MiB  RSS-MiB  TTY    STAT   START  TIME    COMMAND  
    rsaw     5287   0.0   0.0   0        0        pts/0  D+     17:19  0:00    [cat]  
    rsaw     5125   0.0   0.0   0        0        ?      Z      Dec16  0:48    [chromium-browse] <defunct> 
    rsaw     8307   0.2   0.0   0        0        ?      Z      Dec17  3:07    [chromium-browse] <defunct> 
    rsaw     8867   0.0   0.0   0        0        ?      Z      Dec17  1:20    [xchat] <defunct> 
    rsaw     9533   0.0   0.0   0        0        ?      Z      Dec17  0:32    [gedit] <defunct> 
  Top CPU-using processes: 
    USER     PID    %CPU  %MEM  VSZ-MiB  RSS-MiB  TTY    STAT   START  TIME    COMMAND  
    qemu     613    27.4  4.9   3094     393      ?      Sl     00:38  0:29    /usr/bin/qemu-kvm -S -M 
    rsaw     23059  13.7  2.3   1091     185      ?      Sl     Dec26  21:34   /proc/self/exe --type=plugin --plugin-path=/usr/lib64/flash-plugin/libflashplayer.so 
    rsaw     621    5.9   0.8   964      65       ?      Ss     00:38  0:06    python /usr/share/virt-manager/virt-manager.py 
    rsaw     2137   5.3   18.7  2705     1472     ?      Sl     Dec24  204:37  /usr/lib64/firefox/firefox 
    root     1226   2.2   0.5   135      42       tty1   Ss+    Dec24  87:57   /usr/bin/Xorg :0 -background 
    rsaw     11901  2.1   4.2   2319     335      ?      SLl    Dec24  74:54   /usr/bin/gnome-shell 
    rsaw     22866  2.0   1.6   898      128      ?      Sl     Dec26  3:11    /usr/lib64/chromium-browser/chromium-browser --enable-plugins --enable-extensions 
    rsaw     29408  1.3   0.5   1218     39       ?      Sl     Dec26  0:37    gedit 
    rsaw     23084  0.8   1.2   1140     99       ?      Sl     Dec26  1:18    /usr/lib64/chromium-browser/chromium-browser --type=renderer --lang=en-US 
    rsaw     22952  0.8   1.7   1168     141      ?      Sl     Dec26  1:22    /usr/lib64/chromium-browser/chromium-browser --type=renderer --lang=en-US 
  Top MEM-using processes: 
    USER     PID    %CPU  %MEM  VSZ-MiB  RSS-MiB  TTY    STAT   START  TIME    COMMAND  
    rsaw     2137   5.3   18.7  2705     1472     ?      Sl     Dec24  204:37  /usr/lib64/firefox/firefox 
    qemu     613    27.4  4.9   3094     393      ?      Sl     00:38  0:29    /usr/bin/qemu-kvm -S -M 
    rsaw     11901  2.1   4.2   2319     335      ?      SLl    Dec24  74:54   /usr/bin/gnome-shell 
    rsaw     23059  13.7  2.3   1091     185      ?      Sl     Dec26  21:34   /proc/self/exe --type=plugin --plugin-path=/usr/lib64/flash-plugin/libflashplayer.so 
    rsaw     22952  0.8   1.7   1168     141      ?      Sl     Dec26  1:22    /usr/lib64/chromium-browser/chromium-browser --type=renderer --lang=en-US 
    rsaw     22866  2.0   1.6   898      128      ?      Sl     Dec26  3:11    /usr/lib64/chromium-browser/chromium-browser --enable-plugins --enable-extensions 
    rsaw     23214  0.6   1.5   1141     121      ?      Sl     Dec26  0:59    /usr/lib64/chromium-browser/chromium-browser --type=renderer --lang=en-US 
    rsaw     23004  0.0   1.2   1130     100      ?      Sl     Dec26  0:06    /usr/lib64/chromium-browser/chromium-browser --type=renderer --lang=en-US 
    rsaw     22959  0.0   1.2   1127     100      ?      Sl     Dec26  0:05    /usr/lib64/chromium-browser/chromium-browser --type=renderer --lang=en-US 
    rsaw     23084  0.8   1.2   1140     99       ?      Sl     Dec26  1:18    /usr/lib64/chromium-browser/chromium-browser --type=renderer --lang=en-US 
```



**Run on another sosreport with some options:**

```
[rsaw]$ xsos 8308201prodserv --disks --mem --unit=m
MEMORY
  RAM:
    64363 MiB total [61815 MiB (96.0%) used]
    49451 MiB (76.8%) used excluding buffers/cache
    5 MiB (0.0%) dirty
  HugePages:
    38912 MiB pre-allocated to HugePages (60.5% of total ram)
    38146 MiB of HugePages (98.0%) in-use by applications
  LowMem/Slab/PageTables/Shmem:
    61815 MiB (96.0%) of 64363 MiB LowMem in-use
    1192 MiB (1.9%) of total ram used for Slab
    349 MiB (0.5%) of total ram used for PageTables
  Swap:
    464 MiB (2.8%) used of 16381 MiB total

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

[rsaw]$ xsos --net --unit k 8308201prodserv
NETDEV
  Interface  RxKiBytes     RxPackets     RxErrs  RxDrop     TxKiBytes     TxPackets     TxErrs  TxDrop
  =========  =========     =========     ======  ======     =========     =========     ======  ======
  eth0       23569277      300080406     0       0          23569277      480926218     0       0 
  eth1       144484269105  342207919183  0       4494 (0%)  144484269105  357978804158  0       0 
  eth2       428438229206  392560553366  0       373 (0%)   428438229206  330736051661  0       0 
  eth4       37184228554   55232280858   0       2350 (0%)  37184228554   35535848225   0       0 


[rsaw]$ xsos -nug 8308201prodserv
NETDEV
  Interface  RxGiBytes  RxPackets  RxErrs  RxDrop     TxGiBytes  TxPackets  TxErrs  TxDrop
  =========  =========  =========  ======  ======     =========  =========  ======  ======
  eth0       22.5       300 M      0       0          22.5       481 M      0       0 
  eth1       137791     342208 M   0       4494 (0%)  137791     357979 M   0       0 
  eth2       408590     392561 M   0       373 (0%)   408590     330736 M   0       0 
  eth4       35461.6    55232 M    0       2350 (0%)  35461.6    35536 M    0       0 
```



**Run on a sosreport again, showing sysctls:**

```
[rsaw]$ xsos --sysctl boomyaow/
SYSCTLS
  kernel.
    osrelease:  2.6.32-274.el6.x86_64
    tainted: 536870912 512 64 16 1
      Technology Preview code is loaded
      Taint on warning
      Userspace-defined naughtiness
      System experienced a machine check exception
      Proprietary module has been loaded
    random.boot_id:  3d888986-XXXX-4eee-a064-XXXXXXXXXXXX
    random.entropy_avail:  175
    panic:  0  (seconds til reboot after panic)
    hung_task_panic:  0
    panic_on_oops:  1
    panic_on_unrecovered_nmi:  0
    sysrq:  0
    sem:  250  32000  32  128
      max semaphores per array =  250
      max sems system-wide     =  32000
      max ops per semop call   =  32
      max number of arrays     =  128
    shmall (4-KiB pages):  4294967296  (16384.0 GiB max total shared memory)
    shmmax (bytes):  68719476736  (64.00 GiB max segment size)
    shmmni (segments):  4096  (max number of segs)
  net.
    core.rmem_max (bytes):  16777216  (16384 KiB)
    core.wmem_max (bytes):  16777216  (16384 KiB)
    core.rmem_default (bytes):  262144  (256 KiB)
    core.wmem_default (bytes):  262144  (256 KiB)
    ipv4.icmp_echo_ignore_all:  0
    ipv4.ip_forward:  0
    ipv4.tcp_window_scaling:  1
    ipv4.tcp_max_orphans (sockets):  131072  (8192 MiB @ max 64 KiB per orphan)
    ipv4.tcp_mem (4-KiB pages):  496300800  661734400  992601600  (1893.24 GiB, 2524.32 GiB, 3786.47 GiB)
    ipv4.udp_mem (4-KiB pages):  496300800  661734400  992601600  (1893.24 GiB, 2524.32 GiB, 3786.47 GiB)
    ipv4.tcp_rmem (bytes):  4096  1048574  16777216  (4 KiB, 1023 KiB, 16384 KiB)
    ipv4.tcp_wmem (bytes):  4096  1048574  16777216  (4 KiB, 1023 KiB, 16384 KiB)
    ipv4.udp_rmem_min (bytes):  4096  (4 KiB)
    ipv4.udp_wmem_min (bytes):  4096  (4 KiB)
  vm.
    dirty_ratio (percent):  5
    dirty_background_ratio (percent):  2
    dirty_bytes:  0
    dirty_background_bytes:  0
    dirty_expire_centisecs:  600
    dirty_writeback_centisecs:  100
    nr_hugepages (2-MiB pages):  262144  (512.0 GiB total)
    overcommit_memory:  0
    overcommit_ratio (percent):  50
    panic_on_oom:  0
    swappiness:  15
```


**Finally, here's example of using some of the Special options, which allow you to specify a specific file instead of a full sosreport or having `xsos` run on your local system:**

```
[rsaw]$ xsos -v0 --B /tmp/dmidecode.txt --I=/tmp/ipaddr.dump --P /tmp/psaux.txt
DMIDECODE
  BIOS:
    HP, version P66, 06/24/2011
  System:
    Mfr:  HP
    Prod: ProLiant DL980 G7
    Vers: Not Specified
    Ser:  CZ3XXXXXXXX      
    UUID: 35344D41-XXXXXX-5A43-3331-XXXXXXXXXXX
  CPU:
    8 of 8 CPU sockets populated, 8 cores/16 threads per CPU
    64 total cores, 128 total threads
    Mfr:  Intel
    Fam:  Xeon
    Freq: 2133 MHz
    Vers: Intel(R) Xeon(R) CPU E7- 2830 @ 2.13GHz 
  Memory:
    262144 MB (256 GB) total
    32 of 128 DIMMs populated (system max capacity 512 GB)

IP
  Interface  Slave Of  MAC Address        State  IPv4 Address
  =========  ========  =================  =====  ==================
  lo         -         -                  up     127.0.0.1/8
  lo:0       -         -                  up     192.168.160.20/32
  eth0       -         ZZ:xy:56:01:12:qr  up     192.168.160.11/24
  eth1       -         ZZ:xy:9a:1a:19:qr  up     10.79.4.68/23
  eth1:1     -         ZZ:xy:9a:1a:19:qr  up     10.79.4.70/23
  eth2       -         ZZ:xy:28:fd:19:qr  up     10.10.10.8/28
  eth4       -         ZZ:xy:9e:0b:19:qr  up     192.168.254.8/28
  eth5       bond0     ZZ:xy:b5:0b:19:qq  up     -
  eth6       bond0     ZZ:xy:b5:0b:1a:qq  up     -
  eth7       -         ZZ:xy:35:00:99:qq  DOWN   -
  bond0      -         ZZ:zz:ZZ:zz:ZZ:zz  up     10.180.162.22/24

PS CHECK
  Top users of CPU & MEM: 
    USER    %CPU    %MEM      RSS 
    oracle  909.8%  60649.2%  38137.57 GiB
    root    130.8%  2.7%      1.93 GiB
  Uninteruptible sleep & Defunct processes: 
    USER     PID    %CPU  %MEM  VSZ-MiB  RSS-MiB  TTY    STAT  START  TIME      COMMAND  
    oracle   7883   0.3   59.3  141      38177    ?      Ds    Jul31  2:33      oracleCRELSP4 
    oracle   11028  0.1   59.3  154      38170    ?      Ds    Jun30  49:08     ora_arc8_CRELSP4 
    oracle   13816  0.0   59.3  140      38171    ?      Ds    Jul31  0:12      oracleCRELSP4 
    oracle   15184  0.4   59.3  141      38176    ?      Ds    Jul31  3:01      oracleCRELSP4 
    oracle   15423  0.5   59.3  143      38179    ?      Ds    Jul31  3:32      oracleCRELSP4 
    oracle   17334  0.0   59.3  143      38176    ?      Ds    Jul31  0:14      oracleCRELSP4 
    oracle   18502  0.3   59.3  141      38176    ?      Ds    Jul31  2:39      oracleCRELSP4 
    root     18514  2.0   0.0   0        0        ?      D<    10:33  0:00      /bin/grep 
    oracle   20751  0.8   59.3  143      38178    ?      Ds    Jul31  5:21      oracleCRELSP4 
    oracle   24385  0.6   59.3  142      38175    ?      Ds    Jul31  4:05      oracleCRELSP4 
    oracle   24557  0.4   59.3  140      38173    ?      Ds    10:22  0:02      oracleCRELSP4 
    oracle   24718  1.3   59.3  143      38178    ?      Ds    Jul31  8:57      oracleCRELSP4 
    oracle   29008  2.8   59.3  143      38176    ?      Ds    09:38  1:34      oracleCRELSP4  
  Top CPU-using processes: 
    USER     PID    %CPU  %MEM  VSZ-MiB  RSS-MiB  TTY    STAT  START  TIME      COMMAND  
    root     18329  104   0.0   50       2        pts/6  R+    10:33  0:03      /bin/netstat 
    oracle   11160  19.7  59.3  146      38174    ?      Ss    Jun30  9012:36   oracleCRELSP4 
    oracle   14014  10.6  59.3  140      38173    ?      Ss    10:32  0:05      oracleCRELSP4 
    root     6455   5.9   0.1   415      83       ?      Sl    Jan25  16265:29  /space/java/jdk1.5.0_20/bin/java 
    root     6515   5.3   0.0   0        0        ?      R     Jan14  15483:17  [rpciod] 
  Top MEM-using processes: 
    USER     PID    %CPU  %MEM  VSZ-MiB  RSS-MiB  TTY    STAT  START  TIME      COMMAND  
    oracle   7847   0.0   59.4  249      38257    ?      Ss    Jun30  0:52      ora_diag_CRELSP4 
    oracle   1912   0.0   59.3  160      38185    ?      Ss    Jul22  12:29     ora_j001_CRELSP4 
    oracle   10937  0.1   59.3  167      38185    ?      Ss    Jun30  53:14     ora_arc3_CRELSP4 
    oracle   10920  0.1   59.3  167      38185    ?      Ss    Jun30  58:05     ora_arc1_CRELSP4 
    oracle   11037  0.1   59.3  167      38184    ?      Ss    Jun30  53:33     ora_arc9_CRELSP4 
```


REQUIREMENTS
-------

* As detailed in the help page, BASH version 4 (rhel 6+) is required for -i/--ip and for any of the "Special options" like `--C` or `--B`. Colorization is also not done unless BASH v4+ is detected.
* Other than that, nothing special for command requirements -- `xsos` uses standard coreutils & util-linux commands, along with, of course ... `gawk` and `sed`.
* No absolute paths used for commands.
* When running on localhost (i.e. not a sosreport), `xsos` gracefully reports as much of what's requested as possible when running as non-root (`--bios` and `--ethtool` require root privileges; `--disks` won't be able to print dm-multipath output without root privs).


THINGS THAT MIGHT SURPRISE YOU
-------

* There are a bunch of environment variables that you can use to tweak behavior. See [the original commit comment](/ryran/xsos/commit/0c05168d3729d44f4ddf07269b33105f85a306de#commitcomment-2133859) for documentation.
* xsos does some pretty intensive color-formatting to make the output more easily-readable (can be disabled with `-x` or `--nocolor`).
  * If you like the color but need to use a pager, use the `--less` (`-y`) or `--more` (`-z`) options to auto-pipe output to `less -SR` or `more`.
  * There are multiple environment variables for managing the colors. You don't need to modify the script itself. See the above link for documentation.
* xsos can update itself via the internet in 10 seconds if run with `--update` or `-U`.
  * Set environment variable `XSOS_UPDATE_CONFIRM` to "`n`" if you don't want `--update` to prompt for confirmation.
* When printing disk info with `-d/--disks`, xsos automatically detects linux software raid (md) devices and hides their components. The `multipath` command is also consulted to print info about native multipathd devices. All LUNs that are part of a dm-multipath map are also hidden from the disk output.
* You can use the `--unit` (`-u`) option to change how `/proc/meminfo` and `/proc/net/dev` are parsed -- displaying units in anything from bytes up to tebibytes. Note that this option does *not* affect display of the `VSZ` & `RSS` fields in the ps output. (For that, manually set the `XSOS_PS_UNIT` variable to `k`, `m`, or `g`.)
* When printing info on pci net devices (`-l/--lspci`), `xsos` simplifies the net output in an intelligent way. Example:

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
  Net/Storage:
    2 single-port (2) Broadcom Corporation NetXtreme II BCM5708 Gigabit Ethernet (rev 12)
    2 dual-port (4) Intel Corporation 82571EB Gigabit Ethernet Controller (rev 06)
  Graphics:
    Advanced Micro Devices [AMD] nee ATI ES1000 (rev 02)
```


AUTHORS
-------

As far as direct contributions go, so far it's just me, [ryran](/ryran), aka rsaw, aka [Ryan Sawhill](http://b19.org).

When I was clueless in how to further my awk career early on, two prolific users over on StackOverflow -- [Dennis Williamson](http://stackoverflow.com/users/26428/dennis-williamson) and [ghostdog74](http://stackoverflow.com/users/131527/ghostdog74) -- each provided an answer with code that was brilliantly instructive. I've since moved far beyond what I was trying to do back then, and thanks definitely goes to both of them for inspiration.

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

