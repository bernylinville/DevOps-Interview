# DevOps-Interview

## to-do

IO 性能调优

## Linux

> [TLDR](https://github.com/tldr-pages/tldr)
> [Linux Performance](http://www.brendangregg.com/linuxperf.html)

![avatar](http://www.brendangregg.com/Perf/linux_observability_tools.png)
![avatar](http://www.brendangregg.com/BPF/bpf_performance_tools_book.png)

### command

#### 1. sed

```shell
 - Replace the first occurrence of a regular expression in each line of a file, and print the result:
   sed 's/{{regex}}/{{replace}}/' {{filename}}

 - Replace all occurrences of an extended regular expression in a file, and print the result:
   sed -r 's/{{regex}}/{{replace}}/g' {{filename}}

 - Replace all occurrences of a string in a file, overwriting the file (i.e. in-place):
   sed -i 's/{{find}}/{{replace}}/g' {{filename}}

 - Replace only on lines matching the line pattern:
   sed '/{{line_pattern}}/s/{{find}}/{{replace}}/' {{filename}}

 - Delete lines matching the line pattern:
   sed '/{{line_pattern}}/d' {{filename}}

 - Print the first 11 lines of a file:
   sed 11q {{filename}}

 - Apply multiple find-replace expressions to a file:
   sed -e 's/{{find}}/{{replace}}/' -e 's/{{find}}/{{replace}}/' {{filename}}

 - Replace separator / by any other character not used in the find or replace patterns, e.g., #:
   sed 's#{{find}}#{{replace}}#' {{filename}}

```

```shell
# sed 在第一行添加信息
sed -i '1i\{{text}}' {{filename}}
# -i edit files in place

# 在最后一行行前添加
sed -i '$i\{{text}}' {{filename}}

# 在最后一行行后添加
sed -i '$a\{{text}}' {{filename}}

```

#### 2. awk

```shell
 - Print the fifth column (a.k.a. field) in a space-separated file:
   awk '{print $5}' {{filename}}

 - Print the second column of the lines containing "foo" in a space-separated file:
   awk '/{{foo}}/ {print $2}' {{filename}}

 - Print the last column of each line in a file, using a comma (instead of space) as a field separator:
   awk -F ',' '{print $NF}' {{filename}}

 - Sum the values in the first column of a file and print the total:
   awk '{s+=$1} END {print s}' {{filename}}

 - Print every third line starting from the first line:
   awk 'NR%3==1' {{filename}}

 - Print different values based on conditions:
   awk '{if ($1 == "foo") print "Exact match foo"; else if ($1 ~ "bar") print "Partial match bar"; else print "Baz"}' {{filename}}

 - Print all lines where the 10th column value equals the specified value :
   awk '($10 == value)'

 - Print all the lines which the 10th column value is between a min and a max :
   awk '($10 >= min_value && $10 <= max_value)'

```

#### 3. top

```shell
- Start top:
   top

 - Do not show any idle or zombie processes:
   top -i

 - Show only processes owned by given user:
   top -u {{username}}

 - Sort processes by a field:
   top -o {{field_name}}

 - Show the individual threads of a given process:
   top -Hp {{process_id}}

 - Show only the processes with the given PID(s), passed as a comma-separated list. (Normally you wouldn't know PIDs off hand. This example picks the PIDs from the process name):
   top -p $(pgrep -d ',' {{process_name}})

 - Get help about interactive commands:
   ?

```

```shell
# 按 1 后会展开每个核的使用信息
top - 22:23:55 up 54 min,  0 users,  load average: 0.00, 0.00, 0.00
Tasks:  22 total,   1 running,  21 sleeping,   0 stopped,   0 zombie
%Cpu0  :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu1  :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu2  :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu3  :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu4  :  1.0 us,  0.0 sy,  0.0 ni, 99.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu5  :  0.3 us,  0.0 sy,  0.0 ni, 99.7 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu6  :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu7  :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu8  :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu9  :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu10 :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu11 :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu12 :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu13 :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu14 :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu15 :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st

# c 展开进程 command 信息
 1505 root      20   0    1252    348     16 S   0.0   0.0   0:00.00 /init
 1506 kchou     20   0    2608    596    528 S   0.0   0.0   0:00.00 sh -c "$VSCODE_WSL_EXT_LOCATION/scripts/wslServer.sh" 622cb03f7e070a9670c94bae1a45d78d7181fbd4 stable .vscode-server 0
 1507 kchou     20   0    2608   1864   1752 S   0.0   0.0   0:00.00 sh /mnt/c/Users/kylec/.vscode/extensions/ms-vscode-remote.remote-wsl-0.53.4/scripts/wslServer.sh 622cb03f7e070a9670c94bae1a45d78d7181fbd4 stable .vscode-server 0
 1512 kchou     20   0    2608    540    468 S   0.0   0.0   0:00.00 sh /home/kchou/.vscode-server/bin/622cb03f7e070a9670c94bae1a45d78d7181fbd4/server.sh  --port=0 --use-host-proxy --enable-remote-auto-shutdown --print-ip-address
 1514 kchou     20   0  979152  65940  31608 S   0.0   0.3   0:01.86 /home/kchou/.vscode-server/bin/622cb03f7e070a9670c94bae1a45d78d7181fbd4/node /home/kchou/.vscode-server/bin/622cb03f7e070a9670c94bae1a45d78d7181fbd4/out/vs/server/main.js  --port=0 --use-host-proxy -+
 1542 kchou     20   0  856364  41412  28592 S   0.0   0.2   0:00.19 /home/kchou/.vscode-server/bin/622cb03f7e070a9670c94bae1a45d78d7181fbd4/node /home/kchou/.vscode-server/bin/622cb03f7e070a9670c94bae1a45d78d7181fbd4/out/bootstrap-fork --type=watcherService
 1808 kchou     20   0    6036    460      0 S   0.0   0.0   0:00.00 ssh-agent -s
 1874 kchou     20   0    9576   3996   3748 S   0.0   0.0   0:00.00 git fetch
 1875 kchou     20   0   12048   6292   5564 S   0.0   0.0   0:00.00 /usr/bin/ssh git@github.com git-upload-pack 'bernylinville/DevOps-Interview.git'
 1877 kchou     20   0  559556  37632  26416 S   0.0   0.1   0:00.15 /home/kchou/.vscode-server/bin/622cb03f7e070a9670c94bae1a45d78d7181fbd4/node /home/kchou/.vscode-server/bin/622cb03f7e070a9670c94bae1a45d78d7181fbd4/extensions/json-language-features/server/dist/node+
 1933 root      20   0     896     84     16 S   0.0   0.0   0:00.00 /init

# shift + c 按 cpu 排序

# m 切换内存显示
MiB Mem :  2.5/25621.4  [||                                                                                                  ]
MiB Swap:  0.0/7168.0   [                                                                                                    ]

# shift + m 按内存排序

```

##### 进程状态有哪几种

running/sleeping/stopped/zombie

##### 第三行 cpu 参数解释

* %us 用户空间占用CPU百分比
* %sy 内核空间占用CPU百分比
* %ni 用户进程空间内改变过优先级的进程占用CPU百分比
* %id 空闲CPU百分比
* %wa 等待输入输出（I/O）的CPU时间百分比
* %hi 指的是cpu处理硬件中断的时间
* %si指的是cpu处理软中断的时间
* %st 用于有虚拟cpu的情况，用来指示被虚拟机偷掉的cpu时间

> 通常id%值可以反映一个系统cpu的闲忙程度。

##### 横栏说明

PID(进程号)、 USER（运行用户）、PR（优先级）、NI（任务nice值）、VIRT（虚拟内存用量）VIRT=SWAP+RES 、RES（物理内存用量）、SHR（共享内存用量）、S（进程状态）、%CPU（CPU占用比）、%MEM（物理内存占用比）、TIME+（累计CPU占 用时间)、　COMMAND 命令名/命令行。

##### top 命令

* d 指定每两次屏幕信息刷新之间的时间间隔。当然用户可以使用s交互命令来改变之。
* p 通过指定监控进程ID来仅仅监控某个进程的状态。
* q 该选项将使top没有任何延迟的进行刷新。如果调用程序有超级用户权限，那么top将以尽可能高的优先级运行。
* S 指定累计模式。
* s 使top命令在安全模式中运行。这将去除交互命令所带来的潜在危险。
* i 使top不显示任何闲置或者僵死进程。
* c 显示整个命令行而不只是显示命令名。

##### 交互命令

> 从使用角度来看，熟练的掌握这些命令比掌握选项还重要一些。
> 这些命令都是单字母的，如果在命令行选项中使用了s选项，则可能其中一些命令会被屏蔽掉。

* Ctrl+L 擦除并且重写屏幕。
* h或者? 显示帮助画面，给出一些简短的命令总结说明。
* k 终止一个进程。系统将提示用户输入需要终止的进程PID，以及需要发送给该进程什么样的信号。一般的终止进程可以使用15信号；如果不能正常结束那就使用信号9强制结束该进程。默认值是信号15。在安全模式中此命令被屏蔽。
* i 忽略闲置和僵死进程。这是一个开关式命令。
* q 退出程序。
* r 重新安排一个进程的优先级别。系统提示用户输入需要改变的进程PID以及需要设置的进程优先级值。输入一个正值将使优先级降低，反之则可以使该进程拥有更高的优先权。默认值是10。
* s 改变两次刷新之间的延迟时间。系统将提示用户输入新的时间，单位为s。如果有小数，就换算成m s。输入0值则系统将不断刷新，默认值是5 s。需要注意的是如果设置太小的时间，很可能会引起不断刷新，从而根本来不及看清显示的情况，而且系统负载也会大大增加。
* f或者F 从当前显示中添加或者删除项目。
* o或者O 改变显示项目的顺序。
* l 切换显示平均负载和启动时间信息。
* m 切换显示内存信息。
* t 切换显示进程和CPU状态信息。
* c 切换显示命令名称和完整命令行。
* M 根据驻留内存大小进行排序。
* P 根据CPU使用百分比大小进行排序。
* T 根据时间/累计时间进行排序。
* W 将当前设置写入~/.toprc文件中。这是写top配置文件的推荐方法。
* Shift+M 可按内存占用情况进行排序。

#### 4. ps

```shell
To see every process on the system using standard syntax:
          ps -e
          ps -ef
          ps -eF
          ps -ely

ps -ef
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 Feb10 ?        00:00:51 /sbin/init
root           2       0  0 Feb10 ?        00:10:00 [kthreadd]
root           3       2  0 Feb10 ?        00:00:00 [rcu_gp]
root           4       2  0 Feb10 ?        00:00:00 [rcu_par_gp]
root           6       2  0 Feb10 ?        00:00:00 [kworker/0:0H-events_highpri]
root           9       2  0 Feb10 ?        00:00:00 [mm_percpu_wq]
root          10       2  0 Feb10 ?        00:00:00 [rcu_tasks_rude_]
root          11       2  0 Feb10 ?        00:00:00 [rcu_tasks_trace]
root          12       2  0 Feb10 ?        00:01:14 [ksoftirqd/0]
root          13       2  0 Feb10 ?        00:40:11 [rcu_sched]
```

```shell
To see every process on the system using BSD syntax:
          ps ax
          ps axu

ps aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.0 167632 12656 ?        Ss   Feb10   0:51 /sbin/init
root           2  0.0  0.0      0     0 ?        S    Feb10  10:00 [kthreadd]
root           3  0.0  0.0      0     0 ?        I<   Feb10   0:00 [rcu_gp]
root           4  0.0  0.0      0     0 ?        I<   Feb10   0:00 [rcu_par_gp]
root           6  0.0  0.0      0     0 ?        I<   Feb10   0:00 [kworker/0:0H-events_highpri]
root           9  0.0  0.0      0     0 ?        I<   Feb10   0:00 [mm_percpu_wq]
root          10  0.0  0.0      0     0 ?        S    Feb10   0:00 [rcu_tasks_rude_]
root          11  0.0  0.0      0     0 ?        S    Feb10   0:00 [rcu_tasks_trace]
root          12  0.0  0.0      0     0 ?        S    Feb10   1:14 [ksoftirqd/0]
root          13  0.1  0.0      0     0 ?        I    Feb10  40:11 [rcu_sched]
root          14  0.0  0.0      0     0 ?        S    Feb10   0:10 [migration/0]
root          15  0.0  0.0      0     0 ?        S    Feb10   0:00 [cpuhp/0]
root          16  0.0  0.0      0     0 ?        S    Feb10   0:00 [cpuhp/1]
root          17  0.0  0.0      0     0 ?        S    Feb10   0:11 [migration/1]
root          18  0.0  0.0      0     0 ?        S    Feb10   2:01 [ksoftirqd/1]
root          20  0.0  0.0      0     0 ?        I<   Feb10   0:00 [kworker/1:0H-events_highpri]

```

```shell
To print a process tree:
          ps -ejH
          ps axjf

```

```shell
To get security info:
          ps -eo euser,ruser,suser,fuser,f,comm,label
          ps axZ
          ps -eM

To see every process running as root (real & effective ID) in user format:
          ps -U root -u root u

To see every process with a user-defined format:
          ps -eo pid,tid,class,rtprio,ni,pri,psr,pcpu,stat,wchan:14,comm
          ps axo stat,euid,ruid,tty,tpgid,sess,pgrp,ppid,pid,pcpu,comm
          ps -Ao pid,tt,user,fname,tmout,f,wchan

Print only the process IDs of syslogd:
          ps -C syslogd -o pid=

Print only the name of PID 42:
          ps -q 42 -o comm=

```

```shell
-e     Select all processes.  Identical to -A.
-f     Do full-format listing.  This option can be combined with many other UNIX-style options to add additional columns.  It also causes the command arguments to        be printed.  When used with -L, the NLWP (number of threads) and LWP (thread ID) columns will be added.  See the c option, the format keyword args, and the        format keyword comm.

```

```tldr
- List all running processes:
    ps aux

- List all running processes including the full command string:
    ps auxww

- Search for a process that matches a string:
    ps aux | grep string

- List all processes of the current user in extra full format:
    ps --user $(id -u) -F

- List all processes of the current user as a tree:
    ps --user $(id -u) f

- Get the parent pid of a process:
    ps -o ppid= -p pid

- Sort processes by memory consumption:
    ps --sort size

```

#### 5. netstat

```tldr
- List all ports:
    netstat -a

- List all listening ports:
    netstat -l

- List listening TCP ports:
    netstat -t

- Display PID and program names for a specific protocol:
    netstat -p protocol

- Print the routing table:
    netstat -nr
```

```shell
netstat -nlpt
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:8080            0.0.0.0:*               LISTEN      4000109/docker-prox
tcp        0      0 0.0.0.0:6800            0.0.0.0:*               LISTEN      2464597/docker-prox
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      46611/nginx: master
tcp        0      0 0.0.0.0:10001           0.0.0.0:*               LISTEN      3755365/docker-prox
tcp        0      0 0.0.0.0:16881           0.0.0.0:*               LISTEN      2388957/docker-prox
tcp        0      0 0.0.0.0:16882           0.0.0.0:*               LISTEN      2389935/docker-prox
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      2354669/sshd: /usr/
tcp        0      0 0.0.0.0:16888           0.0.0.0:*               LISTEN      2464571/docker-prox
tcp        0      0 0.0.0.0:443             0.0.0.0:*               LISTEN      46611/nginx: master
tcp        0      0 0.0.0.0:445             0.0.0.0:*               LISTEN      457985/smbd
tcp        0      0 127.0.0.1:8125          0.0.0.0:*               LISTEN      996/netdata
tcp        0      0 0.0.0.0:19999           0.0.0.0:*               LISTEN      996/netdata
tcp        0      0 0.0.0.0:6880            0.0.0.0:*               LISTEN      2404858/docker-prox
tcp        0      0 0.0.0.0:18080           0.0.0.0:*               LISTEN      2388945/docker-prox
tcp        0      0 0.0.0.0:18081           0.0.0.0:*               LISTEN      2389923/docker-prox
tcp        0      0 0.0.0.0:139             0.0.0.0:*               LISTEN      457985/smbd
tcp6       0      0 :::80                   :::*                    LISTEN      46611/nginx: master
tcp6       0      0 :::22                   :::*                    LISTEN      2354669/sshd: /usr/
tcp6       0      0 ::1:8125                :::*                    LISTEN      996/netdata
tcp6       0      0 :::445                  :::*                    LISTEN      457985/smbd
tcp6       0      0 :::139                  :::*                    LISTEN      457985/smbd

netstat -nlpu
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
udp        0      0 127.0.0.1:8125          0.0.0.0:*                           996/netdata
udp        0      0 172.17.255.255:137      0.0.0.0:*                           457973/nmbd
udp        0      0 172.17.0.1:137          0.0.0.0:*                           457973/nmbd
udp        0      0 192.168.100.255:138     0.0.0.0:*                           457973/nmbd
udp        0      0 192.168.100.201:138     0.0.0.0:*                           457973/nmbd
udp        0      0 0.0.0.0:138             0.0.0.0:*                           457973/nmbd
udp        0      0 0.0.0.0:16881           0.0.0.0:*                           2388970/docker-prox
udp        0      0 0.0.0.0:16882           0.0.0.0:*                           2389949/docker-prox
udp        0      0 0.0.0.0:16888           0.0.0.0:*                           2464583/docker-prox
udp6       0      0 ::1:8125                :::*                                996/netdata
```

#### 6. tail

```shell
tail -fn 500 /var/logs/nginx/access.log
```

```tldr
- Show last 'num' lines in file:
    tail -n num file

- Show all file since line 'num':
    tail -n +num file

- Show last 'num' bytes in file:
    tail -c num file

- Keep reading file until `Ctrl + C`:
    tail -f file

- Keep reading file until `Ctrl + C`, even if the file is rotated:
    tail -F file

- Show last 'num' lines in 'file' and refresh every 'n' seconds:
    tail -n num -s n -f file
```

#### 7. cat

```tldr
- Print the contents of a file to the standard output:
    cat file

- Concatenate several files into the target file:
    cat file1 file2 > target_file

- Append several files into the target file:
    cat file1 file2 >> target_file

- Number all output lines:
    cat -n file

- Display non-printable and whitespace characters (with `M-` prefix if non-ASCII):
    cat -v -t -e file
```

```shell
-v, --show-nonprinting   use ^ and M- notation, except for LFD and TAB
-t                       equivalent to -vT
-e                       equivalent to -vE
```

#### 8. less

```tldr
- Open a file:
    less source_file

- Page down / up:
    <Space> (down), b (up)

- Go to end / start of file:
    G (end), g (start)

- Forward search for a string (press `n`/`N` to go to next/previous match):
    /something

- Backward search for a string (press `n`/`N` to go to next/previous match):
    ?something

- Follow the output of the currently opened file:
    F

- Open the current file in an editor:
    v

- Exit:
    q
```

#### 9. df

```tldr
- Display all filesystems and their disk usage:
    df

- Display all filesystems and their disk usage in human readable form:
    df -h

- Display the filesystem and its disk usage containing the given file or directory:
    df path/to/file_or_directory

- Display statistics on the number of free inodes:
    df -i

- Display filesystems but exclude the specified type:
    df -x squashfs -x tmpfs
```

#### 10. du

```tldr
- List the sizes of a directory and any subdirectories, in the given unit (KB/MB/GB):
    du -k|m|g path/to/directory

- List the sizes of a directory and any subdirectories, in human-readable form (i.e. auto-selecting the appropriate unit for each size):
    du -h path/to/directory

- Show the size of a single directory, in human readable units:
    du -sh path/to/directory

- List the human-readable sizes of a directory and of all the files and directories within it:
    du -ah path/to/directory

- List the human-readable sizes of a directory and any subdirectories, up to N levels deep:
    du -h -d N path/to/directory

- List the human-readable size of all `.jpg` files in subdirectories of the current directory, and show a cumulative total at the end:
    du -ch */*.jpg
```

#### 11. lsof

```tldr
- Find the processes that have a given file open:
    lsof path/to/file

- Find the process that opened a local internet port:
    lsof -i :port

- Only output the process ID (PID):
    lsof -t path/to/file

- List files opened by the given user:
    lsof -u username

- List files opened by the given command or process:
    lsof -c process_or_command_name

- List files opened by a specific process, given its PID:
    lsof -p PID

- List open files in a directory:
    lsof +D path/to/directory

- Find the process that is listening on a local TCP port:
    lsof -iTCP:port -sTCP:LISTEN
```

#### 12. lspid

#### 13. dmesg

```tldr
- Show kernel messages:
    dmesg

- Show how much physical memory is available on this system:
    dmesg | grep -i memory

- Show kernel messages 1 page at a time:
    dmesg | less
```

#### 14. iotop

```tldr
- Start top-like I/O monitor:
    sudo iotop

- Show only processes or threads actually doing I/O:
    sudo iotop --only

- Show I/O usage in non-interactive mode:
    sudo iotop --batch

- Show only I/O usage of processes (default is to show all threads):
    sudo iotop --processes

- Show I/O usage of given PID(s):
    sudo iotop --pid=PID

- Show I/O usage of a given user:
    sudo iotop --user=user

- Show accumulated I/O instead of bandwidth:
    sudo iotop --accumulated
```

#### 15. pidstat

#### 16. iostat

#### 17. mpstat

#### 18. free

```shell
free -h
              total        used        free      shared  buff/cache   available
Mem:           31Gi        17Gi       1.6Gi        32Mi        11Gi        12Gi
Swap:         975Mi       0.0Ki       975Mi
```

#### 19. vmstat

#### 20. find

```shell
# 在指定目录删除 7 天之前的文件
find {{dir}}/ -type f -mtime +7 -exec rm -f {} \;
# 7 天之内
find {{dir}}/ -type f -mtime -7 -exec rm -f {} \;

# 把 /data 目录及其子目录下所有以扩展名 .txt 结尾的文件中包含 magedu 的字符串全部替换为 magestudy
# {} 表示找到的文件
find /data -type f -name "*.txt" -exec sed -i s'@magedu@magestudy@' {} \;

```

#### 21. ulimit

> core dump ???
> [using_ulimt](https://wiki.archlinux.org/index.php/Core_dump#Using_ulimit)

#### 22. tcpdump

```tldr
- List available network interfaces:
    tcpdump -D

- Capture the traffic of a specific interface:
    tcpdump -i eth0

- Capture all TCP traffic showing contents (ASCII) in console:
    tcpdump -A tcp

- Capture the traffic from or to a host:
    tcpdump host www.example.com

- Capture the traffic from a specific interface, source, destination and destination port:
    tcpdump -i eth0 src 192.168.1.1 and dst 192.168.1.2 and dst port 80

- Capture the traffic of a network:
    tcpdump net 192.168.1.0/24

- Capture all traffic except traffic over port 22 and save to a dump file:
    tcpdump -w dumpfile.pcap port not 22

- Read from a given dump file:
    tcpdump -r dumpfile.pcap
```

```shell
-i <DEV>: 抓取指定网络接口的数据包
-c <COUNT>: 抓取多少个数据包
-w <FILE>: 抓取数据包保存在文件中,而不是直接输出

expression: tcpdump 的表达式

type: 指定抓取类型.host,net,port,portrange,默认host
dir: 指定抓取传输方向.src,dst,src and dst,
proto: 指定抓取数据包协议.ip,tcp,udp

tcpdump [src | dst]host <HOST>: 指定主机
tcpdump host <HOST1> and <HOST2>: 抓取 HOST1 和 HOST2之间的数据包
tcpdump [tcp | udp] port <PORT>: 抓取执行端口数据包
tcpdump host <HOST> and port <PORT>: 抓取主机和端口数据包
tcpdump net <NET>: 抓取网络数据包
```

#### 23. sar

```shell
sar [m [n]]: 每隔 m 秒报告一次,共报告 n 次,默认是 CPU 状态信息
-b 报告 IO 和传输速率
-f [filename] 从历史文件中报告数据
-n [DEV,IP,TCP,UDP] 报告网络状态信息
-o [filename] 将统计结果保存到二进制文件中
-P [0..ALL] 查看 CPU 相关信息
-r 报告内存利用率相关信息
-S 报告 Swarp 利用率相关信息
-u 报告 CPU 利用率状态信息
-v 报告 inode 相关信息
```

### 1. 什么是并发、并行、阻塞、异步、同步

* 并发/并行: CPU在执行多个任务时的方式，并发表示同一段时间里面有多个进程在同一CPU执行，在极短的时间互相切换使人不会发觉。并行只会出现在多个CPU的情况下，表示同一时刻之内有多个进程在执行
例子： 在开车的时候有个电话，必须停下车才可以接电话，接完电话继续开车，这就是并发。 如果一边开车一遍接电话，这就是并行

* 同步/异步: 关注的是请求和响应的通讯机制，描述的是被调用方。当发出请求后，该请求是否等待结果后再返回，同步就是没有得到结果前不会返回，返回即得到请求结果。异步就是得到发出请求后就直接返回，也可能不会立即得到结果，服务得到结果后再通过通知或者回调函数等方法通知调用者
例子: 去买咖啡，付了钱在前台等待咖啡制作完毕，就是同步，付了钱不在前台等待，找位置坐下，服务员送过来就是异步

* 阻塞/非阻塞: 关注的是请求在等待结果时的状态，描述的是调用方。 阻塞就是在等待结果的时候，当前线程会被挂起，在得到结果之后返回；非阻塞则是没有得到结果之前也不会阻塞当前线程
例子: 阻塞的情况就是卖咖啡的时候什么都不能做，只能挂起；非阻塞的时候就是在等咖啡的时候可以玩着手机，过一会检查咖啡是否好了

### 2. shell $ 解释

```shell
$$
Shell本身的PID（ProcessID）
$!
Shell最后运行的后台Process的PID
$?
最后运行的命令的结束代码（返回值）
$-
使用Set命令设定的Flag一览
$*
所有参数列表。如"$*"用「"」括起来的情况、以"$1 $2 … $n"的形式输出所有参数。
$@
所有参数列表。如"$@"用「"」括起来的情况、以"$1" "$2" … "$n" 的形式输出所有参数。
$#
添加到Shell的参数个数
$0
Shell本身的文件名
${#str}
str 变量字符长度
$1～$n
添加到Shell的各参数值。$1是第1参数、$2是第2参数…。
# 在脚本中添加set -x可以进行每一行的调试
```

### 3. 怎么防止个人history操作泄露问题

* ```history -c``` 清空当前命令历史, ```history -w``` 将已经清空的命令历史写入到命令历史文件 ```~/.bash_history``` 中 (或直接清空该文件), 下次登录边不再有命令历史
* ```unset HISTFILE```
* ```export HISTIGNORE=*```

### 4. 怎么解决一些不小心的命令操作

```shell
# 误执行 rm
rm -f test
# 使用 lsof 命令查看当前是否有进程打开 test
lsof |grep /root/test
lsof: no pwd entry for UID 101
less       2545           root    4r      REG                8,3         5  268637282 /root/test (deleted)
# 查看是否存在恢复数据
# /proc/2545/fd：进程操作的文件描述符目录。
# 4：文件描述符。
cat /proc/2545/fd/4
test
# 使用I/O重定向恢复文件
cat /proc/2545/fd/4 > /root/test
```

#### 刨根问底

通过前面的模拟场景演示了恢复文件的整个过程，那么原理是什么，在什么情况下，文件才是可恢复的。

在Linux系统中，每个运行中的程序都有一个宿主进程彼此隔离，以/proc/进程号来体现（Linux本质上就是一个文件系统），比如：ls -l /proc/13067 查看进程PID为13067的进程信息；当程序运行时，操作系统会专门开辟一块内存区域，提供给当前进程使用，对于依赖的文件，操作系统会发放一个文件描述符，以便读写文件，当我们执行 rm -f 删除文件时，其实只是删除了文件的目录索引节点，对于文件系统不可见，但是对于打开它的进程依然可见，即仍然可以使用先前发放的文件描述符读写文件，正是利用这样的原理，所以我们可以使用I/O重定向的方式来恢复文件。

#### 总结

如果不小心误删了文件，不要着急，首先使用 lsof 查看打开该文件的进程，然后再使用 cat /proc/进程号/fd/文件描述符 查看恢复数据，最后使用I/O重定向的方式来恢复文件。

### 5. linux调用命令后系统内部又是怎么做的

### 6. linux命令怎么被调用的

strace常用来跟踪进程执行时的系统调用和所接收的信号。 在Linux世界，进程不能直接访问硬件设备，当进程需要访问硬件设备(比如读取磁盘文件，接收网络数据等等)时，必须由用户态模式切换至内核态模式，通过系统调用访问硬件设备。strace可以跟踪到一个进程产生的系统调用,包括参数，返回值，执行消耗的时间。

[linux命令—— strace命令（跟踪进程中的系统调用）](https://www.cnblogs.com/kongzhongqijing/articles/4913192.html)
[在 Linux 上用 strace 来理解系统调用](https://linux.cn/article-11545-1.html)
[Understanding system calls on Linux with strace](https://opensource.com/article/19/10/strace)

### 7. 讲述你如何做系统优化,提高系统性能，充分利用资源？

1. 优化内存，把不需要的服务关掉
2. 定期清理备份文件，加大磁盘使用空间
3. sysctl.conf文件做内核优化
4. 文件句柄数（打开最大文件数）调到65535
5. 通过修改应用软件的配置文件，对服务进行优化，提高内存、CPU的使用率
6. systemctl disable 不需要的服务（/sbin/chkconfig off）
7. 根据实际监控情况进行调整

### 8. IO 性能不足，你如何调优？

使用 ssd，不在乎数据丢失可用 raid0

简单的就是加硬件罗raid卡加大缓存，用更快的硬盘软件的话磁盘写入用deadline，禁止文件系统的日志优化性能oracle的话还尽量用裸磁盘做数据盘，不同业务还可以分开写到不同硬盘

### 9. 面对大并发，LNMP 架构 如何优化 特别影响性能那些参数?

### 10. 面对大并发，如何 MySQL 优化？

数据库方面双主带多从，读写分离罗改应用代码，把不常修改的数据全部读入memcache中（比如用户登录用的帐号数据），这样基本把mysql的读压力分担走优化mysql语句，该用myiasm表的用myiasm表（比如不太太重要的用户帐号数据表），数据库设置concurrent_insert直接从表尾并发插入，这样可以有效降低大量注册与登录的锁竞争

* 基于redis做mysql读写分离
* mysql自身的优化

总的来说还是自身因素影响的比较多，我们可以通过修改my.cnf配置文件来对mysql进行进一步的优化。我们可以通过修改mysql的参数使得mysql拥有更可靠的性能。下面是我的数据库配置，自己通过百度谷歌，找很多配置选项的解析（配置适合mysql5.5以上的版本），然后总结。希望对你有帮助。（注意一下优化配置均在【mysqld】选项下配置，不要搞错成【mysql】）

```conf
[mysqld]
back_log = 300
binlog_format = MIXED
character-set-server=utf8mb4
long_query_time = 1
log-bin=/databack/data_logbin/mysql_binlog
innodb_log_file_size=2G
innodb_log_buffer_size=4M
innodb_buffer_pool_size=4G
#innodb_file_per_table = ON
innodb_thread_concurrency=8
innodb_flush_logs_at_trx_commit=2
#innodb_additional_mem_pool_size=4M
join_buffer_size = 8M
key_buffer_size=256M
max_connections = 1000
max_allowed_packet = 4M
max_connect_errors = 10000
myisam_sort_buffer_size = 64M
port = 3306
query_cache_type=1
query_cache_size = 64M
read_buffer_size=4M
read_rnd_buffer_size=4M
server-id = 1
skip-external-locking
slow_query_log = 1
#skip-name-resolve
#skip-networking
sort_buffer_size = 8M
socket = /tmp/mysql.sock
table_open_cache=1024
thread_cache_size = 64
thread_stack = 256K
tmp_table_size=64M
wait_timeout = 10
```

下面是对上面配置的解析：

```back_log = 300```：该参数的值表示在MySql的连接数据达到#max_connections 时，在它暂时停止响应新请求之前的短时间内有300个请求可以被存在堆栈中，即新来的请求将会被存在堆栈中，以等待某一连接释放资源，该堆栈的数量即back_log，等 mysql处理完其他请求之后会对其作出响应，如果等待连接的数量超过#back_log，将不被授予连接资源。你可以合理的设置你的back_log，但是该值不要高于操作系统的限制的值。系统的默认值为50。Linux系统一般设置小于512的整数。

```binlog_format = MIXED```：配置主从模式下，选取同步的模式，Mysql主从的复制可以有三种复制类型，分别是:语句的复制STATEMEN,行的复制ROW和混合类型的复制MIXED，语句的复制顾名思义就是在主服务器上执行的SQL语句，在从服务器上执行同样的语句，行的复制就是把改变的内容复制过去，而不是把命令在从服务器上执行一遍。默认采用基于语句的复制，一旦发现基于语句的无法精确的复制时，就会采用基于行的复制,配置，复制类型可以通过binlog_format =在配置文件上配置

```character-set-server=utf8mb4``` ：utf-8编码可能2个字节、3个字节、4个字节的字符，但是MySQL的utf8编码只支持3字节的数据，而移动端的表情数据是4个字节的字符。如果直接往采用utf-8编码的数据库中插入表情数据，Java程序中将报SQL异常utf8mb4编码是utf8编码的超集，兼容utf8，并且能存储4字节的表情字符。 采用utf8mb4编码的好处是，存储与获取数据的时候，不用再考虑表情字符的编码与解码问题。

```long_query_time = 1```：设置慢查询响应的时间，记录超过1秒的SQL执行语句。

```log-bin=/databack/data_logbin/mysql_binlog``` ：设置二进制日志的存放路径，如果不设置系统会默认存放到mysql的目录下，建议创建新的目录来存放二进制日志，且该目录不要同数据库同个目录，存放目录拥有者为mysql。

```innodb_log_file_size=2G``` ：在高写入负载尤其是大数据集的情况下很重要。这个值越大则性能相对越高，跟据服务器大小而异。这是redo日志的大小。redo日志被用于确保写操作快速而可靠并且在崩溃时恢复。在MySQL 5.5，redo日志的总尺寸被限定在4GB(默认可以有2个log文件)。而MySQL 5.6里可以设置允许大于4G。你可以一开始就把它设置成4G。这个值的设置其实是可以计算的 你可以通过命令SHOW GLOBAL STATUS的输出看Innodb_os_log_written的值，把该值除以1024*1024 得到的结果是每分钟处理的redo日志大小，然后再乘以60得到每小时处理的日志大小，因为在5.5以上版本都是默认有两个日志重做日志文件ib_logfile0和ib_logfile1，所得到结果再除以2，再取整就是你的redo该设置大小了。

```innodb_log_buffer_size=4M```：默认为1M，在默认的设置在中等强度写入负载以及较短事务的情况下，服务器性能还可以。如果存在更新操作峰值或者负载较大，就应该考虑加大它的值了。在 InnoDB在事务提交前，并不将改变的日志写入到磁盘中，因此在大事务中，可以减轻磁盘I/O的压力。通常情况下，如果不是写入大量的超大二进制数据，4MB-8MB已经足够了。

```innodb_buffer_pool_size=4G```：这配置对Innodb表来说非常重要。该参数主要作用是缓存innodb表的索引，数据，插入数据时的缓冲由于Innodb把数据和索引都缓存起来，因此在配置该参数时，可以设置它高达60-80% 的可用内存（官网是建议的也是系统内存的80%左右）。缓冲池是数据和索引缓存的地方这能保证你在大多数的读取操作时使用的是内存而不是硬盘。一般配置的值是5-6GB(8GB内存)，19-25GB(32GB内存)，38-50GB(64GB内存)仅供参考。

```#innodb_file_per_table = ON```：在5.6中，该选项属性默认值是ON，由于对新建的表有影响，所以在之前的版本中你需要把它设置成ON。这项设置告知InnoDB是否需要将所有表的数据单独放在一个.ibd文件，这样做的好处是使得每个表都有自已独立的表空间。每个表的数据和索引都会存在自已的表空间中。也实现单表在不同的数据库中移动，且空间可以回收。

```innodb_thread_concurrency=8```：指服务器逻辑线程数可以设置成与系统一样数量，参数可配置成逻辑CPU数量的两倍。

系统CPU查看命令如下：

```shell
查看逻辑CPU个数：

#cat /proc/cpuinfo |grep "processor"|sort -u|wc –l


查看物理CPU个数：

# cat /proc/cpuinfo | grep "physical id" |sort -u|wc -l


查看每个物理CPU内核个数：

# cat /proc/cpuinfo |  grep "cpu cores" |uniq
```

```innodb_flush_logs_at_trx_commit=2```：系统默认值是 1，但是这样设置会使得提交更新事务都会刷新到磁盘中，会造成资源耗费。所以需要值设置为 2，这样就不用不把日志刷新到磁盘上，而只刷新到操作系统的缓存上。但然啦也可以设置为0， 这样设置是很快，但也造成了相对的不安全，会导致MySQL服务器崩溃时就会丢失一些事务。而设置为 2 刚好尼补了。

```#innodb_additional_mem_pool_size=4M```：该参数默认为1M适当调整该参数的大小以确保所有数据都能存放在内存中提高访问效率的，主要用来存放Innodb的内部目录，这个值不用分配太大，系统可以自动调。在mysql5.6.3可以忽略。

```join_buffer_size = 8M```：表示#联合查询操作所能使用的缓冲区大小。

```key_buffer_size=256M```：指定索引缓冲区的大小，它决定索引处理的速度，你可以设置成系统的物理内存的1/4，它主要针对的是MyISAM引擎，但是设置大少不要超过4G，不然会出现问题。

```max_connections = 1000```：设置置MySQL的最大连接，按你实际情况适当设置就好。如果你经常看到‘Too many connections'错误，是因为max_connections的值太低了，所以需要设置更高的链接数，如果max_connection值被设高之后的缺陷是当服务器运行超过设置阈值或更高的活动事务时会变的没有响应。

```max_allowed_packet = 4M```：这个参数mysql消息缓冲区的大小，如果这个过小可能会影响到部分操作，默认是1M，一般设置成4-16M就可以了。

```max_connect_errors = 10000```：表示如果有同一个主机访问的参数值超出该参数值个数的中断错误连接，则该主机将被禁止连接。如需对该主机进行解禁，执行：FLUSH HOST。

```myisam_sort_buffer_size = 64M```：这个参数默认是8M，表示MyISAM表发生变化时重新排序所需的缓冲，一般64M就已经足够了。

```port = 3306```：表示使用3306来做mysql启动端口

```query_cache_type=1```：表示控制缓存的类型，有三个参数可选（0、1、2）设置为0，表示缓存没有应用，也就相当于禁用了，设置为1，表示缓存所有的结果，设置为2表示只缓存在select语句中通过SQL_CACHE指定需要缓存的查询。

```query_cache_size=32M```：参数表示mysql查询结果的缓冲区大小，一般不建议设置太大，因为设置太大会增加开销，一般设置成32M-256M左右即可，设置参数一般为2的倍数。

```read_buffer_size=4M```：表示按顺序查询操作包括读、查询等操作所能使用的缓冲区大小，和sort_buffer_size一样，该参数对应的分配内存也是每连接独享，一般不建议太大，对于4G到16G内存的服务器2M-8M就可以了。

```read_rnd_buffer_size=4M```：表示是MySQL的随机读缓冲区大小。当任意顺序读取行时将分配一个随机读取缓冲区，进行排序查询时，便分配随机缓冲作为该操作的缓冲区大小，同样的对于4G到16G内存的服务器2M-8M就可以了。

```server-id = 1```：表示做主从同步所定义的serverid，作为master的server_id必须必slave端的要小，越小表示优先级越高，但是在同个网段内的mysql服务，不允许设置同样的sever_id。参数可设参考范围（1-200）。

```skip-external-locking```：开启该选项表示避免MySQL的外部锁定，减少出错几率增强稳定性，适用于单服务器环境。

```slow_query_log = 1```：开启慢查询日志，作用于慢查询日志,顾名思义,就是查询慢的日志。

```skip-name-resolve```：禁止MySQL对外部连接进行DNS解析，使用这一选项可以消除MySQL进行DNS解析的时间。但需要注意，如果开启该选项，则所有远程主机连接授权都要使用IP地址方式，否则MySQL将无法正常处理连接请求。

```skip-networking```：开启该选项可以彻底关闭MySQL的TCP/IP连接方式，如果WEB服务器是以远程连接的方式访问MySQL数据库服务器则不要开启该选项，否则将无法正常连接。

```sort_buffer_size = 8M```：表示查询排序时所能使用的缓冲区大小。它直接与实时连接的个数 有关，实时连接的个数乘以sort_buffer_size的大小就是实际分配的总共排序缓冲区大小。所以，对于内存在4GB-8G左右的服务器可以设置为6-16M。

```socket = /tmp/mysql.sock：mysql.sock``` 文件作用主要是server和client在同一台服务器，当使用本地连接时，就会使用socket进行连接，该文件一般是放在/var/lib/mysql/mysql.sock下，也常常使用ln –s 在/tmp目录下做软连接。

```table_open_cache=1024```：table_cache主要用于设置table高速缓存的数量。由于每个客户端连接都会至少访问一个表，因此此参数的值与max_connections有关。你可以通过命令show variables like '%open%'; 查看open_files_limit参数,大量使用MyISAM的环境里，应该保证open_files_limit表类型至少是table_cache的二到三倍，调到512-1024最佳。

```thread_cache_size = 64``` ：这个变量值表示的是可以重新利用保存在缓存中线程的数量,当断开连接时如果缓存中还有空间,那么客户端的线程将被放到缓存中,如果线程重新被请求，那么请求将从缓存中读取,如果缓存中是空的或者是新的请求，那么这个线程将被重新创建,如果有很多新的线程，增加这个值可以改善系统性能.通过比较 Connections 和 Threads_created 状态的变量，可以看到这个变量的作用 根据物理内存设置规则可以做以下配置2G-4G可以设置为16-64左右，当然大于4G的服务器，设置64也已经足够了。

```thread_stack = 256K```：表示每个连接线程被创建时，MySQL给它分配的内存大小，对于8-16G的服务器设置成256K就可以了，再大一点的，可以适当增加呢。

```tmp_table_size=64M```：表示定义一个临时表的大小，该值默认为16M，可调到64-256最佳，线程独占，太大可能内存不够造成I/O堵塞，如果动态页面可以适当调大点。

```wait_timeout = 100```：表示指定一个请求的最大连接时间，该值过大会导致，MySQL里大量的SLEEP进程无法及时释放，拖累系统性能，不过也不能把这个指设置的过小，否则你可能会遭遇到“MySQL has gone away”之类的问题。  系统默认是8个小时，感觉太大，可以设置小点。

### 11. CDN工作原理和优缺？

优点就是分流罗，可以有效分担静态资源的压力最大缺点是各地数据同步需要一段时间，更新一个重要静态文件的话，生效时间急死人，而且价格也不便宜

假设通过CDN加速的域名为www.a.com，接入CDN网络，开始使用加速服务后，当终端用户（北京）发起HTTP请求时，处理流程如下：

1. 当终端用户（北京）向www.a.com下的指定资源发起请求时，首先向LDNS（本地DNS）发起域名解析请求。
2. LDNS检查缓存中是否有www.a.com的IP地址记录。如果有，则直接返回给终端用户；如果没有，则向授权DNS查询。
3. 当授权DNS解析www.a.com时，返回域名CNAME www.a.tbcdn.com对应IP地址。
4. 域名解析请求发送至阿里云DNS调度系统，并为请求分配最佳节点IP地址。
5. LDNS获取DNS返回的解析IP地址。
6. 用户获取解析IP地址。
7. 用户向获取的IP地址发起对该资源的访问请求。

* 如果该IP地址对应的节点已缓存该资源，则会将数据直接返回给用户，例如，图中步骤7和8，请求结束。
* 如果该IP地址对应的节点未缓存该资源，则节点向源站发起对该资源的请求。获取资源后，结合用户自定义配置的缓存策略，将资源缓存至节点，例如，图中的北京节点，并返回给用户，请求结束。

* CDN的加速资源是跟域名绑定的。
* 通过域名访问资源，首先是通过DNS分查找离用户最近的CDN节点（边缘服务器）的IP
* 通过IP访问实际资源时，如果CDN上并没有缓存资源，则会到源站请求资源，并缓存到CDN节点上，这样，用户下一次访问时，该CDN节点就会有对应资源的缓存了。

#### 使用CDN的优点

* 更快地传递内容

由于CDN最靠近用户放置，因此当您的内容需要移动的距离更短时，可以减少延迟。CDN可以使您的网站加载速度更快。例如，如果您的网站位于英国并且您从美国获得流量，则您的CDN提供商可能在美国拥有服务器并将该服务器用于您的网站。

* 更多同步用户

CDN可以确保网络具有高数据阈值。因此，大量用户可以在没有延迟的情况下同时访问网络。通过实现高流量，CDN允许来自世界各地的人们同时访问您的网站。

* 持续可用性

CDN中的服务器始终在运行，即使服务器已关闭，您的网站也可以访问。如果没有CDN，您的服务器有时可能会关闭，这意味着您必须等到主机解决问题。如果您使用CDN，则不会发生这种情况。如果服务器崩溃，CDN将使用您的缓存页面。

* 可靠的内容传递

如果您使用CDN，则内容的传送更加一致。特别是，这涉及高分辨率内容，如视频和图像。CDN提供高质量的内容交付，因此对您网站的性能产生重大影响。由于54％的客户希望在您的网站上看到视频，因此您可以以高分辨率提供这些视频对您的业务至关重要。

* 控制资产交付

当CDN监控您的资产交付时，运营商可以根据实时统计数据确定需要额外容量的位置。如果某个地区的服务器出现过载，运营商可以提供额外的带宽，以确保一切顺利运行。

* 防止流量高峰

如果您的网站突然出现大量流量，您可以从CDN服务中受益。大型服务器网络可确保这些资源在所有情况下都可用且可扩展。许多企业的噩梦是他们将获得大幅增加的流量，但他们的网站无法处理它。

#### 使用CDN的缺点

* 成本

这可能是使用CDN的最重要的缺点。开始使用CDN服务的成本很高，而且它们也有许多隐藏成本。其中包括每次数据传输和千兆字节的成本。高成本来自第三方网络。启动新的CDN网络要求服务器公司从另一家公司获得帮助以安装此类网络。请务必仔细阅读所有条款和条件。

鉴于此，CDN网络往往是能够负担这些成本的大公司的更好选择。在全球范围内维护无用的复制服务器也是不切实际的。

* 服务地点

如果您的大多数受众群体位于CDN没有服务器的国家/地区，则您网站上的数据可能需要比不使用任何CDN更进一步。

* 限制

一些组织和国家已阻止流行CDN的域或IP地址。在这种情况下，来自这些组织或国家/地区的受众群体无法访问您的网站，您最终会失去部分流量。

* 支持可用性

当第三方供应商负责运行CDN时，会出现支持问题。如果出现技术问题，即使很少见，您也无法知道操作员需要多长时间来解决问题并防止再次发生问题。

* 失去控制

您是否愿意将您的网站文件移交给另一家公司？在决定是否使用CDN之前，您必须考虑这一点。使用CDN意味着第三方会收到有关您的网站和系统的信息。

结论

是否应该使用CDN的问题取决于贵公司的需求。如果你有一个拥有高流量和足够资源的热门网站，使用CDN是非常不错的选择。

### 12. 进程/线程

* 进程就是处于执行期的程序（目标码存放在某种存储介质上）。但是进程并不仅仅局限于一段可执行代码段。通常进程还要包含其他资源，例如：打开的文件，挂起的信号，内核内部数据，处理器状态，一个或多个具有内存映射的的内存地址空间及一个或者多个执行线程(thread of execution)等。实际上，进程就是正在执行的程序代码的实时结果。
* 线程(thread)，或者成为执行线程，是在进程中活动的对象。每个线程都拥有一个独立的程序计数器、进程栈和一组进程寄存器。内核调度的对象是线程而不是进程。在传统的Unix/Linux系统中，一个进程只包含一个线程，但现代操作系统中，包含多个线程的多线程程序司空见惯。(Linux系统的线程实现非常特别：它对线程和进程并不是特别区分。对Linux而言，线程只不过是一种特殊的进程罢了)

> 进程——资源分配的最小单位 线程——程序执行的最小单位

进程有独立的堆栈和局部变量，但线程没有单独的地址空间，一个线程死掉就等于整个进程死掉，所以多进程的程序要比多线程的程序健壮，但是进程切换时，耗费资源较大，效率要差一些(据统计是线程的30倍左右)。但是对于一些要求同时进行并且又要共享某些变量的并发操作，只能用线程，不能用进程。

#### 为什么使用多线程而不是多进程

线程间方便的通信机制。对于不同进程来说，它们具有独立的数据空间，要进行数据的传递只能通过进程间通信的方式进行，这种方式不仅费时，而且很不方便。线程则不然，由于同一进程下的线程之间共享数据空间，所以一个线程的数据可以直接为其他线程所用。

#### 并发和并行

并发又称共行，是指能处理多个同时性活动的能力，并发事件之间不一定要同一时刻发生。 比如，现代计算机系统可在同一段时间内以进程的形式将多个程序加载到存储器中，并借由处理器的时分复用， 以在一个处理器上表现出同时运行的感觉。
并行是指同时发生的两个并发事件，具有并发的含义，而并发则不一定并行。
并发和并行的区别就是一个处理器同时处理多个任务和多个处理器或者是多核的处理器同时处理多个不同的任务。 前者是逻辑上的同时发生（simultaneous），而后者是物理上的同时发生。

### 13. 进程间通信方式（管道的应用场景）

### 14. Linux文件权限755（文件目录x权限区别）

三个数字分别对应 u g o，也就是 user group others

* r 4
* w 2
* x 1

755 也就是 rwx rx rx

### 15. cpu load x （x这个值代表什么意思）

Linux load averages are "system load averages" that show the running thread (task) demand on the system as an average number of running plus waiting threads. This measures demand, which can be greater than what the system is currently processing. Most tools show three averages, for 1, 5, and 15 minutes:

```shell
uptime
 12:17:33 up 35 days, 19:24,  4 users,  load average: 0.20, 0.17, 0.23

top - 12:17:59 up 35 days, 19:24,  4 users,  load average: 0.20, 0.18, 0.23

cat /proc/loadavg
0.14 0.16 0.22 1/812 3627
```

* If the averages are 0.0, then your system is idle.
* If the 1 minute average is higher than the 5 or 15 minute averages, then load is increasing.
* If the 1 minute average is lower than the 5 or 15 minute averages, then load is decreasing.
* If they are higher than your CPU count, then you might have a performance problem (it depends).

[Linux Load Averages: Solving the Mystery](http://www.brendangregg.com/blog/2017-08-08/linux-load-averages.html)

### 16. 如何查看进程使用到的文件

```shell
sudo lsof -p 4000236
COMMAND     PID             USER   FD      TYPE DEVICE SIZE/OFF     NODE NAME
nginx   4000236 systemd-timesync  cwd       DIR   0,72     4096  8782388 /
nginx   4000236 systemd-timesync  rtd       DIR   0,72     4096  8782388 /
nginx   4000236 systemd-timesync  txt       REG   0,72  1176936  4982148 /usr/sbin/nginx
nginx   4000236 systemd-timesync  mem       REG   8,82           4982148 /usr/sbin/nginx (path inode=5777766)
nginx   4000236 systemd-timesync  DEL       REG   0,18          11165307 /[aio]
nginx   4000236 systemd-timesync  mem       REG   8,82           4325965 /lib/libz.so.1.2.11 (stat: No such file or directory)
nginx   4000236 systemd-timesync  mem       REG   8,82           4325962 /lib/libcrypto.so.1.1 (stat: No such file or directory)
nginx   4000236 systemd-timesync  mem       REG   8,82           4325963 /lib/libssl.so.1.1 (stat: No such file or directory)
nginx   4000236 systemd-timesync  mem       REG   8,82           4982107 /usr/lib/libpcre.so.1.2.12 (stat: No such file or directory)
nginx   4000236 systemd-timesync  mem       REG   8,82           4325960 /lib/ld-musl-x86_64.so.1 (stat: No such file or directory)
nginx   4000236 systemd-timesync  DEL       REG    0,1              7789 /dev/zero
nginx   4000236 systemd-timesync    0u      CHR    1,3      0t0        5 /dev/null
nginx   4000236 systemd-timesync    1w     FIFO   0,12      0t0 11163212 pipe
nginx   4000236 systemd-timesync    2w     FIFO   0,12      0t0 11163213 pipe
nginx   4000236 systemd-timesync    3w     FIFO   0,12      0t0 11163212 pipe
nginx   4000236 systemd-timesync    4u     sock    0,8      0t0 11159464 protocol: UNIX
nginx   4000236 systemd-timesync    5w     FIFO   0,12      0t0 11163213 pipe
nginx   4000236 systemd-timesync    6w     FIFO   0,12      0t0 11163212 pipe
nginx   4000236 systemd-timesync    7u     sock    0,8      0t0 11159461 protocol: TCP
nginx   4000236 systemd-timesync    8u     sock    0,8      0t0 11159463 protocol: UNIX
nginx   4000236 systemd-timesync    9u  a_inode   0,13        0     9096 [eventpoll]
nginx   4000236 systemd-timesync   10u  a_inode   0,13        0     9096 [eventfd]
nginx   4000236 systemd-timesync   11u  a_inode   0,13        0     9096 [eventfd]
nginx   4000236 systemd-timesync   12u     sock    0,8      0t0 11159466 protocol: UNIX
nginx   4000236 systemd-timesync   13u     sock    0,8      0t0 11159468 protocol: UNIX
nginx   4000236 systemd-timesync   14u     sock    0,8      0t0 11159470 protocol: UNIX
nginx   4000236 systemd-timesync   15u     sock    0,8      0t0 11159472 protocol: UNIX
nginx   4000236 systemd-timesync   16u     sock    0,8      0t0 11159474 protocol: UNIX
nginx   4000236 systemd-timesync   17u     sock    0,8      0t0 11159476 protocol: UNIX
```

### 17. 软硬链接区别（实现机制）

* 什么是软链接

符号链接Symbolic Link（symlink），又称软链接Soft Link，是一种特殊的文件，它指向 Linux 系统上的另一个文件或目录。

这和 Windows 系统中的快捷方式有点类似，链接文件中记录的只是原始文件的路径，并不记录原始文件的内容。

符号链接通常用于对库文件进行链接，也常用于链接日志文件和网络文件系统Network File System（NFS）上共享的目录。

* 什么是硬链接

硬链接是原始文件的一个镜像副本。创建硬链接后，如果把原始文件删除，链接文件也不会受到影响，因为此时原始文件和链接文件互为镜像副本。

为什么要创建链接文件而不直接复制文件呢？

当你需要将同一个文件保存在多个不同位置，而且还要保持持续更新的时候，硬链接的重要性就体现出来了。

如果你只是单纯把文件复制到另一个位置，那么另一个位置的文件只会保存着复制那一刻的文件内容，后续也不会跟随着原始文件持续更新。

而使用硬链接时，各个镜像副本的文件内容都会同时更新。

[Linux 中软链接和硬链接的区别](https://linux.cn/article-12270-1.html)
[Difference between Soft link vs Hard link](https://www.2daygeek.com/difference-between-soft-link-vs-hard-link-linux/)

### 18. kill和kill -9的区别，有没有更优雅的方式kill进程

kill pid的作用是向进程号为pid的进程发送SIGTERM（这是kill默认发送的信号），该信号是一个结束进程的信号且可以被应用程序捕获。若应用程序没有捕获并响应该信号的逻辑代码，则该信号的默认动作是kill掉进程。这是终止指定进程的推荐做法。

kill -9 pid则是向进程号为pid的进程发送SIGKILL（该信号的编号为9），从本文上面的说明可知，SIGKILL既不能被应用程序捕获，也不能被阻塞或忽略，其动作是立即结束指定进程。通俗地说，应用程序根本无法“感知”SIGKILL信号，它在完全无准备的情况下，就被收到SIGKILL信号的操作系统给干掉了，显然，在这种“暴力”情况下，应用程序完全没有释放当前占用资源的机会。事实上，SIGKILL信号是直接发给init进程的，它收到该信号后，负责终止pid指定的进程。在某些情况下（如进程已经hang死，无法响应正常信号），就可以使用kill -9来结束进程。

若通过kill结束的进程是一个创建过子进程的父进程，则其子进程就会成为孤儿进程（Orphan Process），这种情况下，子进程的退出状态就不能再被应用进程捕获（因为作为父进程的应用程序已经不存在了），不过应该不会对整个linux系统产生什么不利影响。

应用程序如何优雅退出

Linux Server端的应用程序经常会长时间运行，在运行过程中，可能申请了很多系统资源，也可能保存了很多状态，在这些场景下，我们希望进程在退出前，可以释放资源或将当前状态dump到磁盘上或打印一些重要的日志，也就是希望进程优雅退出（exit gracefully）。

从上面的介绍不难看出，优雅退出可以通过捕获SIGTERM来实现。具体来讲，通常只需要两步动作：

1. 注册SIGTERM信号的处理函数并在处理函数中做一些进程退出的准备。信号处理函数的注册可以通过signal()或sigaction()来实现，其中，推荐使用后者来实现信号响应函数的设置。信号处理函数的逻辑越简单越好，通常的做法是在该函数中设置一个bool型的flag变量以表明进程收到了SIGTERM信号，准备退出。
2. 在主进程的main()中，通过类似于while(!bQuit)的逻辑来检测那个flag变量，一旦bQuit在signal handler function中被置为true，则主进程退出while()循环，接下来就是一些释放资源或dump进程当前状态或记录日志的动作，完成这些后，主进程退出。

### 19. buffer和cache的区别,如何清理 cache 缓存

* buffer 是即将要被写入磁盘的，buffer 能够使分散的写操作集中进行，减少磁盘碎片和硬盘的反复寻道，从而提高系统性能
* cache 是被从磁盘中读出来的.cached 是把读取过的数据保存起来，重新读取时若命中就不要去读硬盘，若没有命中就读硬盘

```shell
# 清理缓存
sync
echo 1 > /proc/sys/vm/drop_caches  # 释放页面缓存
echo 2 > /proc/sys/vm/drop_caches  # 释放索引,inodes 节点缓存,可能会降低磁盘索引效率
echo 3 > /proc/sys/vm/drop_caches  # 释放页面缓存,索引,inode 缓存
```

### 20. Shell 脚本中的 return 和 exit 作用及 exit 的取值范围

From ```man bash``` on ```return [n]```;

Causes a function to stop executing and return the value specified by n to its caller. If n is omitted, the return status is that of the last command executed in the function body.

on ```exit [n]```:

Cause the shell to exit with a status of n. If n is omitted, the exit status is that of the last command executed. A trap on EXIT is executed before the shell terminates.

exit 取值范围 0～255

### 21. Linux启动流程

1. 加载内核 /boot
2. 启动初始化进程 init进程
3. 确定运行级别 runlevel Linux预置七种运行级别（0-6）。一般来说，0是关机，1是单用户模式（也就是维护模式），6是重启。运行级别2-5，各个发行版不太一样，对于Debian来说，都是同样的多用户模式（也就是正常模式）
4. 加载开机启动程序 /etc/rc2.d
5. 用户登录
6. 进入 login shell 首先读入 /etc/profile 依次寻找 ~/.bash_profile ~/.bash_login ~/.profile（找到一个就不读后面的文件），图形界面登录：只加载 /etc/profile 和 ~/.profile
7. 打开 non-login shell

[Linux 的启动流程](https://www.ruanyifeng.com/blog/2013/08/linux_boot_process.html)

### 22. 子网掩码的作用

子网掩码是一个32位地址，是与IP地址结合使用的一种技术。

它的主要作用有两个，一是用于屏蔽IP地址的一部分以区别网络标识和主机标识，并说明该IP地址是在局域网上，还是在远程网上。二是用于将一个大的IP网络划分为若干小的子网络。

网络标识就是用以区分不同的网段，主机号是用以区分同一网段内的不同主机。

### 23. 如何找出日志中 “www.baidu.com” 出现的次数

```shell
grep -c "www.baidu.com" test.log # -c or --count

grep "www.baidu.com" test.log| wc -l

# 压缩日志查询
zgrep -c "www.baidu.com" test.log.gz

```

### 24. 服务器CPU负载很高，但是使用率不高

### 25. 系统调优

### 26. 如果腾讯视频有大量用户觉得卡

### 27. 创建0-99个空文件

```shell
touch test{1..99}
```

### 28. 判断一个文件中的重复IP地址

### 29. 你认为什么是SRE

### 30. awk 打印第 10 行

### 31. Bash 中两个数做运算的几种方式

```shell
sum=$[ ${v1} + ${v2} ]
(( sum=${v1} + ${v2} ))
let sum=${v1}+${v2}     # 这里运算符两端必须没有空格
`expr ${v1} + ${v2}`    # 这里运算符号两端必须要有空格
```

### 32. read 命令从管道中读取字节流

```shell
echo "sinaops" | read a ; echo $a
echo "sinaops" | while read a ; do echo $a ; done
```

### 33. 统计域名出现次数 "http://hi.baidu.com/browse/"

```shell
# 与统计 IP 访问次数基本一致
awk -F '/' '{num[$3]++}END{for (name in num){print  name,num[name]}}' file
```

### 34. 打印奇数/偶数行

```shell
# n 表示读取模式空间的下一行; N 表示追加下一行到模式空间
sed -n 'p;n' file # 奇数行
sed -n 'n;p' file # 偶数行
# 奇数行和偶数行合并
sed 'N;s/\n/ /g' file

# 奇数行与偶数行交换
sed -r 'N;s@(.*)\n(.*)@\2\n\1@g' file
```

### 35. 文件中两列数据,分别为 ip,status_code.统计状态码为 200 中,出现次数最多的 IP

```shell
awk '/200$/{ip_num[$1]++}END{for(ip in ip_num){print ip,ip_num[ip]}}' ip_code | awk '/NR==1/{print $1,"出现次数最多,为",$2}' | sort -nrk 2 | awk '{if (FNR==1){print $1,"出现次数最多,为",$2}}'
```

### 36. 进程的有效用户与实际用户

Linux 系统中某个可执行文件属于 root 并且有 setid, 当一个普通用户 mike 运行这个程序时，产生的进程的有效用户和实际用户分别是 root 和 mike

### 37. fork() 创建进程

```c
/*
* 在父进程中,fork 返回新创建子进程的进程 ID
* 在子进程中,fork 返回 0
* 如果出现错误,fork 返回一个负值
*/



int main()
{
    fork() || fork();
    sleep(100);
    retrun 0;
}
/*
* 共创建 3 个
* main() -> fork() -> fork()
*/

int main()
{
    fork() && fork();
    sleep(100);
    retrun 0;
}
/*
* 共创建 3 个
* main() -> fork()
*        -> fork()
*/
int main()
{
    fork() && fork() || fork();
    sleep(100);
    return 0;
}
/*
* 共创建 5 个
* main() -> fork() -> fork()
*        -> fork() -> fork()
*/

int main()
{
    fork(); // 新创建 1 个
    fork() && fork() || fork(); // 新创建 (1+1)*2+(1+1)*2 = 8 个
    fork(); // 新创建 1+8+1=10 个
    sleep(100);
    return 0;
}
// 共创建 20 个进程
```

### 38. ln -s 与 mv 对某文件操作时，对 inode 和 block 有什么影响

```shell
# 理解 blocks 为磁盘存储块, inode 为指向该存储块的地址
ln -s afile bfile
# 原始文件不变
# 新文件 inode 与原始文件不同,且 block 为 0
# 原因: 创建符号链接,系统为符号链接分配 inode,符号链接不存储数据,只是作为引用,故 block 为 0

mv afile bfile
# 原始文件不存在
# 新文件与原始文件 inode 与 block 相同
# 原因: 修改文件名后,系统只是将文件名做改变,inode 与存储 block 不变

ln afile bfile
# 原始文件不变
# 新文件 inode 和 block 与原始文件相同
# 原因: 创建硬链接,相当于为原始文件指定一个别名,inode 与存储 block 不变,这两个文件名指向系统底层存储是一样的

cp afile bfile
# 原始文件不变
# 新文件 block 与 原始文件相同,inode 不同
# 原因: 拷贝文件,相当于对系统底层存储做拷贝,系统为新文件重新分配 inode,文件内容一直,block不变
```

### 39. top 页面含义

当前时间及运行时长, 登录用户数, 平均负载
任务数: 总数,运行中,休眠中,停止,僵尸进程
CPU占比: 用户空间, 内核空间,, 空闲进程,等待进程
内存: 总物理内存,使用,剩余,buffer 缓冲
Swap: 总,使用,剩余,cache 缓存

进程ID 用户 优先级 nice值 虚拟内存 物理内存 共享内存 进程状态 CPU 内存 运行时长 命令

### 40. kill 信号

```shell
1 HUP 终端断线
2 INT 中断,同 Ctrl + C
3 QUIT 退出
9 KILL 强制终止
15 TERM 优雅的终止
18 CONT 继续(bg/fg命令)
19 STOP 暂停,同 Ctrl + Z
```

### 41. 文件描述符

文件描述符是一个非负的索引值，指向内核中的” 文件记录表”.

* 当打开一个现存文件或创建一个新文件时，内核就向进程返回一个文件描述符
* 当需要读写文件时，文件描述符可作为参数传递给相应的函数
* Linux 下所有对设备和文件的操作都使用文件描述符来进行

常见文件描述符如下

* 0: 表示标准输入，对应宏为: STDIN_FILENO, 函数 scanf () 使用的是标准输入
* 1: 表示标准输出，对应宏为: STDOUT_FILENO, 函数 printf () 使用的是标准输出
* 2: 表示标准出错处理，对应的宏为: STDERR_NO

## 安全

### 1. 如何防止DDOS 攻击？如提供足够资源给你，要保证用户访问不影响

首先确定攻击源范围，如果是处于公司内部，那么暂时性的将这一区域的内部网络封掉，如果是外部IP 那么通过防火墙或者软件进行IP过滤，这样能够一定程度上减缓承受的攻击压力。其次，开启备用服务器，如果攻击流量过大，限制大流量访问，保证大多数用户的正常使用。

### 2. 简述CC攻击原理？如何防止CC 攻击？受到CC攻击如何处理？

CC攻击的原理就是攻击者

控制某些主机

不停地发大量数据包

给对方服务器造成服务器资源耗尽，一直到宕机崩溃。CC主要是用来攻击页面的，每个人都有这样的体验：当一个网页访问的人数特别多的时候，打开网页就慢了，CC就是模拟多个用户（多少线程就是多少用户）不停地进行访问那些需要大量数据操作（就是需要大量CPU时间）的页面，造成服务器资源的浪费，CPU长时间处于100%，永远都有处理不完的连接直至就网络拥塞，正常的访问被中止。

防御CC攻击可以通过多种方法，禁止网站代理访问，尽量将网站做成静态页面，限制连接数量，修改最大超时时间等。

写一个脚本，抓取日志文件中非正常访问的IP，因为这类的攻击不会如同正常用户一样读取网站的所以信息，一般是比较有针对性的访问，将这类IP屏蔽掉。为了防止可能的误伤，可以在对这个屏蔽做一个有效期。

### 3. 介绍一下你是如何做黑客入侵的安全防护？

## Python

### 一个列表，找出三个数的和等于19，代码实现

### Python打开一个文件，找出某个字符串最快的方法

### 常用的内置函数

| 函数名     | 参数            | 介绍                         | 返回值   | 举例                               |
| ---------- | --------------- | ---------------------------- | -------- | ---------------------------------- |
| abs        | Number          | 返回数字绝对值               | 正数     | abs(-10)                           |
| all        | List            | 判断列表内容是否全是 true    | Bool     | all(['', '123']                    |
| help       | Object          | 打印对象的帮助用法           | 无       | help(list)                         |
| enumerate  | Iterable        | 迭代时记录索引               | 无       | for index, item in enumerate(list) |
| input      | Str             | 命令行输入消息               | Str      | input('请输入信息：')              |
| isinstance | Object, type    | 判断对象是否是某种类型       | Bool     | isinstance('a', str)               |
| type       | Object          | 判断对象的类型               | Str      | type(10)                           |
| vars       | instance        | 返回实例化的字典信息         | dict     |                                    |
| dir        | Object          | 返回对象中所有可用方法和属性 | List     | dir('type')                        |
| hasattr    | Obj, key        | 判断对象中是否有某个属性     | Bool     | hasttr('1', 'upper')               |
| setattr    | Obj, key, value | 为实例化对象添加属性与值     | 无       | setattr(instance, 'run', 'go')     |
| getattr    | Obj, key        | 通过对象获取属性             | 任何类型 | getattr(Obj, key)                  |
| any        | Iterable        | 判断内容是否有 true 值       | Bool     | any([1, 0, ''])                    |

### 装饰器

* 也是一种函数
* 可以接受函数作为参数
* 可以返回函数
* 接受一个函数，内部对其处理，然后返回一个新函数，动态的增强函数功能
* 将 c 函数在 a 函数中执行，在 a 函数中可以选择执行或不执行 c 函数，也可以对 c 函数的结果进行二次加工处理

```python
def out(func_args): # 外围函数
    def inter(*args, **kwargs): # 内嵌函数
        return func_args(*args, **kwargs)
    return inter # 外围函数返回内嵌函数

```

#### 用法

* 将被调用的函数直接作为参数传入装饰器的外围函数括弧

```python
def a(func):
    def b(*args, **kwargs):
        return func(*args, **kwargs)
    return b

def c(name):
    print(name)

a(c('berny')) # berny

```

* 将装饰器与被调用函数绑定在一起

@符号 + 装饰器函数放在被调用函数的上一行，被调用的函数正常定义，只需要直接调用被执行函数即可

```python
@a
def c(name):
    print(name)

c('berny') # berny

```

#### classmethod

* 将类函数可以不经过实例化而直接被调用

```python
@classmethod
def func(cls, ...)
    do

```

```python
class Test(object):
    @classmethod
    def add(cls, a, b):
        return a + b

Test.add(1, 2)

```

#### staticmethod

* 将类函数可以不经过实例化而直接被调用，被该装饰器调用的函数不许传递 self 或 cls 参数，且无法在该函数内调用其他类函数或类变量

```python
@staticmethod
def func(...)
    do

```

```python
class Test(object):
    @staticmethod
    def add(a, b):
        return a + b

Test.add(1, 2)

```

#### property

将类函数的执行免去括弧，类似于调用属性（变量）

```python
@property
def funcself
    do

```

```python
class Test(object):
    def __init__(self, name):
        self.name = name

    @propperty
    def call_name(self):
        return 'hello {}'.format(self.name)

test = Test('berny')
result = test.call_name
print(result) # berny

```

```python
class Test(object):
    def __init__(self, __name):
        self.__name = name

    @propperty
    def name(self):
        return self.__name

    @name.setter
    def name(self, value):
        self.__name = value

test = Test('berny')
print(test.name) # berny
test.name = 'test'
print(test.name) # test

```

### 一些类中的函数说明

#### __str__

返回对于该类的描述

```python
class Test(object):
    def __str__(self):
        return '描述'

test = Test()
print(test) # 描述

```

#### __getattr__

当调用的属性或者方法不存在时，会返回该方法定义的信息

```python
class Test(object):
    def __getattr__(self, key):
        print('key: {}不存在'.format(key))

test = Test()
test.a

```

#### __setattr__

拦截当前类中不存在的属性与值

```python
class Test(object):
    def __setattr__(self, key, value):
        if key not in self.__dict__:
            self.__dict__[key] = value

test = Test()
test.name = 'berny'
t.name # 'berny'

```

#### __call__

本质是将一个类变成一个函数

```python
class Test(object):
    def __call__(self, **kwargs):
        print('args is {}'.format(kwargs))

test = Test()
test(name='berny') # args is {'name': 'berny'}

```

### python 有哪些设计模式

[python36种设计模式](https://github.com/ydf0509/python36patterns)

### 浅拷贝和深拷贝区别

#### 浅拷贝

b = a.copy()

a b 列表内部数据发生变化后，会相互影响

```shell
In [7]: a = [[1,2,3],[4,5,6]]

In [8]: b = a.copy()

In [9]: b[1].append(7)

In [10]: b
Out[10]: [[1, 2, 3], [4, 5, 6, 7]]

In [11]: a
Out[11]: [[1, 2, 3], [4, 5, 6, 7]]

In [12]: a[0].append(8)

In [13]: a
Out[13]: [[1, 2, 3, 8], [4, 5, 6, 7]]

In [14]: b
Out[14]: [[1, 2, 3, 8], [4, 5, 6, 7]]

```

#### 深拷贝

对深层数据也进行了拷贝，原是变量和新变量完全不共享数据

```b = copy.deepcopy(a)```

### 迭代器(Iterator)与生成器(Generator)的区别

迭代器是一个更抽象的概念，任何对象，如果它的类有next方法（__next__ python3)和__iter__方法返回自己本身。

每个生成器都是一个迭代器，但是反过来不行。通常生成器是通过调用一个或多个yield表达式构成的函数s生成的。同时满足迭代器的定义。

当你需要一个类除了有生成器的特性之外还要有一些自定义的方法时，可以使用自定义的迭代器，一般来说生成器更方便，更简单。

```python
    def squares(start, stop):
        for i in xrange(start, stop):
            yield i*i
```

等同于生成器表达式：

```python
    （i*i for i in xrange(start, stop))```

列表推倒式是：

```python
    [i*i for i in xrange(start, stop)]```

如果是构建一个自定义的迭代器：

```python
    class Squares(object):
        def __init__(self, start, stop):
            self.start = start
            self.stop = stop
        def __iter__(self):
            return self
        def next(self):
            if self.start >= self.stop:
                raise StopIteration
            current = self.start * self.start
            self.start += 1
            return current
```

此时，你还可以定义自己的方法如：

```python
    def current(self):
        return self.start
```

两者的相同点：对象迭代完后就不能重写迭代了。

#### Iterables, Iterators, Genrators

==============================

热身一下

--------------
如果你是来自其它语言比如c，很自然想到的方式是创建一个计数器，然后以自增的方式迭代list。

```python
    my_list = [17  23  47  51  101  173  999  1001]

    i = 0
    while i < len(my_list):
        v = my_list[i]
        print v,
        i += 1
输出：

    17 23 47 51 101 173 999 1001
```

也有可能会借用range，写一个类C语言的风格的for循环：

```python
    for i in range(len(my_list)):
        v = my_list[i]
        print v,
输出：

    17 23 47 51 101 173 999 1001
```

上面两种方法都不是Pythonic方式，取而代之的是：

```python
    for v in my_list:
        print v,
输出：

    17 23 47 51 101 173 999 1001
```

很多类型的对象都能通过这种方式来迭代，迭代字符串会生成单个字符：

```python
    for v in "Hello":
        print v,
输出：

    H e l l o
```

迭代字典，生成字典的key（以无序的方式）：

```python
    d = {
        'a': 1,
        'b': 2,
        'c': 3,
        }

    for v in d:
        print v,
    # 注意这里是无序的

输出：

    a c b
```

迭代文件对象，产生字符串行，包括换行符：

```python
    f = open("suzuki.txt")
    for line in f:
        print ">", line
输出：

    > On education

    > "Education has failed in a very serious way to convey the most important lesson science can teach: skepticism."

    > "An educational system isn't worth a great deal if it teaches young people how to make a living but doesn't teach them how to make a life."
```

以上可以看出列表、元祖、字符串、字典、文件都可以迭代，能被迭代的对象都称为可迭代对象（Iteratbles)，for循环不是唯一接收Iteratbles的东东，还有：

list构造器接收任何类型的Iteratbles，可以使用list()接收字典对象返回只有key的列表：

```python
    list(d)
输出：

    ['a', 'c', 'b']
还可以：

    list("Hello")
输出：

    ['H', 'e', 'l', 'l', 'o']

还可以用在列表推倒式中：

    ascii = [ord(x) for x in "Hello"]
    ascii
输出：

    [72, 101, 108, 108, 111]
```

sum()函数接收任何数字类型的可迭代对象:

```python
    sum(ascii)

输出：

    500
```

str.join()方法接收任何字符类型的可迭代对象 （这里的说法不严谨，总之原则是迭代的元素必须是str类型的)：

```python
    "-".join(d)
输出：

    ‘a-c-b'
```

[difference-between-python-generators-vs-iterators](http://stackoverflow.com/questions/2776829/difference-between-python-generators-vs-iterators)
[itergen1](http://excess.org/article/2013/02/itergen1/)

### python 3 和 2 的区别

#### print 函数

print 语句没有了，取而代之的是 print() 函数。 Python 2.6 与 Python 2.7 部分地支持这种形式的 print 语法。在 Python 2.6 与Python 2.7 里面，以下三种形式是等价的：

```python
print "fish"
print ("fish") # 注意print后面有个空格
print("fish") # print()不能带有任何其它参数
```

然而，Python 2.6 实际已经支持新的 print() 语法，实例如下：

```python
from __future__ import print_function
print("fish", "panda", sep=', ')
```

如果 Python2.x 版本想使用使用 Python3.x 的 print 函数，可以导入 __future__ 包，该包禁用 Python2.x 的 print 语句，采用 Python3.x 的 print 函数：

```python
>>> list =["a", "b", "c"]
>>> print list    # python2.x 的 print 语句
['a', 'b', 'c']
>>> from __future__ import print_function  # 导入 __future__ 包
>>> print list     # Python2.x 的 print 语句被禁用，使用报错
  File "<stdin>", line 1
    print list
             ^
SyntaxError: invalid syntax
>>> print (list)   # 使用 Python3.x 的 print 函数
['a', 'b', 'c']
>>>
```

Python3.x 与 Python2.x 的许多兼容性设计的功能可以通过 __future__ 这个包来导入。

#### Unicode

Python 2 有 ASCII str() 类型，unicode() 是单独的，不是 byte 类型。

现在， 在 Python 3，我们最终有了 Unicode (utf-8) 字符串，以及一个字节类：byte 和 bytearrays。

由于 Python3.x 源码文件默认使用 utf-8 编码，所以使用中文就更加方便了：

```python
>>> 中国 = 'china' 
>>>print(中国) 
china
```

Python 2.x

```python
>>> str = "我爱北京天安门"
>>> str
'\xe6\x88\x91\xe7\x88\xb1\xe5\x8c\x97\xe4\xba\xac\xe5\xa4\xa9\xe5\xae\x89\xe9\x97\xa8'
>>> str = u"我爱北京天安门"
>>> str
u'\u6211\u7231\u5317\u4eac\u5929\u5b89\u95e8'
```

Python 3.x

```python
>>> str = "我爱北京天安门"
>>> str
'我爱北京天安门'
```

#### 除法运算

Python 中的除法较其它语言显得非常高端，有套很复杂的规则。Python 中的除法有两个运算符，/ 和 //

首先来说 / 除法:

在 Python 2.x 中 / 除法就跟我们熟悉的大多数语言，比如 Java 和 C ，整数相除的结果是一个整数，把小数部分完全忽略掉，浮点数除法会保留小数点的部分得到一个浮点数的结果。

在 Python 3.x 中 / 除法不再这么做了，对于整数之间的相除，结果也会是浮点数。

Python 2.x:

```python
>>> 1 / 2
0
>>> 1.0 / 2.0
0.5
```

Python 3.x:

```python
>>> 1/2
0.5
```

而对于 // 除法，这种除法叫做 floor 除法，会对除法的结果自动进行一个 floor 操作，在 Python 2.x 和 Python 3.x 中是一致的。

python 2.x:

```python
>>> -1 // 2
-1
```

python 3.x:

```python
>>> -1 // 2
-1
```

注意的是并不是舍弃小数部分，而是执行 floor 操作，如果要截取整数部分，那么需要使用 math 模块的 trunc 函数

python 3.x:

```python
>>> import math
>>> math.trunc(1 / 2)
0
>>> math.trunc(-1 / 2)
0
```

#### 异常

在 Python 3 中处理异常也轻微的改变了，在 Python 3 中我们现在使用 as 作为关键词。

捕获异常的语法由 except exc, var 改为 except exc as var。

使用语法except (exc1, exc2) as var 可以同时捕获多种类别的异常。 Python 2.6 已经支持这两种语法。

1. 在 2.x 时代，所有类型的对象都是可以被直接抛出的，在 3.x 时代，只有继承自BaseException的对象才可以被抛出。
2. 2.x raise 语句使用逗号将抛出对象类型和参数分开，3.x 取消了这种奇葩的写法，直接调用构造函数抛出对象即可。
在 2.x 时代，异常在代码中除了表示程序错误，还经常做一些普通控制结构应该做的事情，在 3.x 中可以看出，设计者让异常变的更加专一，只有在错误发生的情况才能去用异常捕获语句来处理。

#### xrange

在 Python 2 中 xrange() 创建迭代对象的用法是非常流行的。比如： for 循环或者是列表/集合/字典推导式。

这个表现十分像生成器（比如。"惰性求值"）。但是这个 xrange-iterable 是无穷的，意味着你可以无限遍历。

由于它的惰性求值，如果你不得仅仅不遍历它一次，xrange() 函数 比 range() 更快（比如 for 循环）。尽管如此，对比迭代一次，不建议你重复迭代多次，因为生成器每次都从头开始。

在 Python 3 中，range() 是像 xrange() 那样实现以至于一个专门的 xrange() 函数都不再存在（在 Python 3 中 xrange() 会抛出命名异常）。

```python
import timeit

n = 10000
def test_range(n):
    return for i in range(n):
        pass

def test_xrange(n):
    for i in xrange(n):
        pass  
```

Python 2

```python
print 'Python', python_version()

print '\ntiming range()' 
%timeit test_range(n)

print '\n\ntiming xrange()' 
%timeit test_xrange(n)

Python 2.7.6

timing range()
1000 loops, best of 3: 433 µs per loop


timing xrange()
1000 loops, best of 3: 350 µs per loop
```

Python 3

```python
print('Python', python_version())

print('\ntiming range()')
%timeit test_range(n)

Python 3.4.1

timing range()
1000 loops, best of 3: 520 µs per loop
```

```python
print(xrange(10))
---------------------------------------------------------------------------
NameError                                 Traceback (most recent call last)
<ipython-input-5-5d8f9b79ea70> in <module>()
----> 1 print(xrange(10))

NameError: name 'xrange' is not defined
```

#### 八进制字面量表示

八进制数必须写成0o777，原来的形式0777不能用了；二进制必须写成0b111。

新增了一个bin()函数用于将一个整数转换成二进制字串。 Python 2.6已经支持这两种语法。

在Python 3.x中，表示八进制字面量的方式只有一种，就是0o1000。

python 2.x

```python
>>> 0o1000
512
>>> 01000
512
```

python 3.x

```python
>>> 01000
  File "<stdin>", line 1
    01000
        ^
SyntaxError: invalid token
>>> 0o1000
512
```

#### 不等运算符

Python 2.x中不等于有两种写法 != 和 <>

Python 3.x中去掉了<>, 只有!=一种写法，还好，我从来没有使用<>的习惯

#### 去掉了repr表达式``

Python 2.x 中反引号``相当于repr函数的作用

Python 3.x 中去掉了``这种写法，只允许使用repr函数，这样做的目的是为了使代码看上去更清晰么？不过我感觉用repr的机会很少，一般只在debug的时候才用，多数时候还是用str函数来用字符串描述对象。

```python
def sendMail(from_: str, to: str, title: str, body: str) -> bool:
    pass
```

#### 多个模块被改名（根据PEP8）

| 旧的名字        | 新的名字     |
| --------------- | ------------ |
| _winreg         | winreg       |
| ConfigParser    | configparser |
| copy_regcopyreg |
| Queue           | queue        |
| SocketServer    | socketserver |
| repr            | reprlib      |

StringIO模块现在被合并到新的io模组内。 new, md5, gopherlib等模块被删除。 Python 2.6已经支援新的io模组。

httplib, BaseHTTPServer, CGIHTTPServer, SimpleHTTPServer, Cookie, cookielib被合并到http包内。

取消了exec语句，只剩下exec()函数。 Python 2.6已经支援exec()函数。

#### 数据类型

1）Py3.X去除了long类型，现在只有一种整型——int，但它的行为就像2.X版本的long

2）新增了bytes类型，对应于2.X版本的八位串，定义一个bytes字面量的方法如下：

```python
>>> b = b'china' 
>>> type(b) 
<type 'bytes'> 
```

str 对象和 bytes 对象可以使用 .encode() (str -> bytes) 或 .decode() (bytes -> str)方法相互转化。

```python
>>> s = b.decode() 
>>> s 
'china' 
>>> b1 = s.encode() 
>>> b1 
b'china'
```

3）dict的.keys()、.items 和.values()方法返回迭代器，而之前的iterkeys()等函数都被废弃。同时去掉的还有 dict.has_key()，用 in替代它吧 。

#### 打开文件

原：

```python
file( ..... )
```

或

```python
open(.....)
```

改为只能用

```python
open(.....)
```

从键盘录入一个字符串

原:

```python
raw_input( "提示信息" )
```

改为:

```python
input( "提示信息" )
```

在python2.x中raw_input()和input( )，两个函数都存在，其中区别为：

* raw_input()---将所有输入作为字符串看待，返回字符串类型
* input()-----只能接收"数字"的输入，在对待纯数字输入时具有自己的特性，它返回所输入的数字的类型（int, float ）

在python3.x中raw_input()和input( )进行了整合，去除了raw_input()，仅保留了input()函数，其接收任意任性输入，将所有输入默认为字符串处理，并返回字符串类型。

#### map、filter 和 reduce

这三个函数号称是函数式编程的代表。在 Python3.x 和 Python2.x 中也有了很大的差异。

首先我们先简单的在 Python2.x 的交互下输入 map 和 filter,看到它们两者的类型是 built-in function(内置函数):

```python
>>> map
<built-in function map>
>>> filter
<built-in function filter>
>>>
```

它们输出的结果类型都是列表:

```python
>>> map(lambda x:x *2, [1,2,3])
[2, 4, 6]
>>> filter(lambda x:x %2 ==0,range(10))
[0, 2, 4, 6, 8]
>>>
```

但是在Python 3.x中它们却不是这个样子了：

```python
>>> map
<class 'map'>
>>> map(print,[1,2,3])
<map object at 0x10d8bd400>
>>> filter
<class 'filter'>
>>> filter(lambda x:x % 2 == 0, range(10))
<filter object at 0x10d8bd3c8>
>>>
```

首先它们从函数变成了类，其次，它们的返回结果也从当初的列表成了一个可迭代的对象, 我们尝试用 next 函数来进行手工迭代:

```python
>>> f =filter(lambda x:x %2 ==0, range(10))
>>> next(f)
0
>>> next(f)
2
>>> next(f)
4
>>> next(f)
6
>>>
```

对于比较高端的 reduce 函数，它在 Python 3.x 中已经不属于 built-in 了，被挪到 functools 模块当中。

### 如何在2G内存里实现10G文件的一个排序

### 写代码将/etc/profile中的path下的某一特定路径提到最前面

### 电影票抢座问题

### Python伪线程

### 线程池实现原理

### Python协程相关

### Python多进程

### 如何反转一个整数

## Ansible

## Docker

### 1. Dockerfile 中 add copy 区别

COPY 指令将从构建上下文目录中 <源路径> 的文件/目录复制到新的一层的镜像内的 <目标路径> 位置。

```shell
COPY abcdocker.json /usr/src/app/
```

ADD指令不仅能够将构建命令所在的主机本地的文件或目录，而且能够将远程URL所对应的文件或目录，作为资源复制到镜像文件系统。
所以，可以认为ADD是增强版的COPY，支持将远程URL的资源加入到镜像的文件系统。

### 2. docker 网络模式

* host模式，使用```—net=host```指定。
* container模式，使用```—net=container:NAME_or_ID```指定。
* none模式，使用```—net=none```指定。
* bridge模式，使用```—net=bridge```指定，默认设置。

* host模式

Docker使用的网络实际上和宿主机一样，在容器内看到的网卡ip是宿主机上的ip。

众所周知，Docker使用了Linux的Namespaces技术来进行资源隔离，如PID Namespace隔离进程，Mount Namespace隔离文件系统，Network Namespace隔离网络等。一个Network Namespace提供了一份独立的网络环境，包括网卡、路由、Iptable规则等都与其他的Network Namespace隔离。一个Docker容器一般会分配一个独立的Network Namespace。但如果启动容器的时候使用host模式，那么这个容器将不会获得一个独立的Network Namespace，而是和宿主机共用一个Network Namespace。容器将不会虚拟出自己的网卡，配置自己的IP等，而是使用宿主机的IP和端口。

* container模式

多个容器使用共同的网络看到的ip是一样的。

在理解了host模式后，这个模式也就好理解了。这个模式指定新创建的容器和已经存在的一个容器共享一个Network Namespace，而不是和宿主机共享。新创建的容器不会创建自己的网卡，配置自己的IP，而是和一个指定的容器共享IP、端口范围等。同样，两个容器除了网络方面，其他的如文件系统、进程列表等还是隔离的。两个容器的进程可以通过lo网卡设备通信。

* none模式

这种模式下不会配置任何网络。

这个模式和前两个不同。在这种模式下，Docker容器拥有自己的Network Namespace，但是，并不为Docker容器进行任何网络配置。也就是说，这个Docker容器没有网卡、IP、路由等信息。需要我们自己为Docker容器添加网卡、配置IP等。

* bridge模式

bridge模式是Docker默认的网络设置，此模式会为每一个容器分配Network Namespace、设置IP等，并将一个主机上的Docker容器连接到一个虚拟网桥上。

类似于Vmware的nat网络模式。同一个宿主机上的所有容器会在同一个网段下，相互之间是可以通信的。

### 3. docker 存储引擎

Docker最开始采用AUFS作为文件系统，也得益于AUFS分层的概念，实现了多个Container可以共享一个image。但是由于AUFS未并入Linux内核，且只支持Ubuntu，考虑到兼容性问题，在Docker 0.7 版本中引入了存储驱动，目前，Docker支持AUFS、Btrfs、Devicemapper、OverlayFS、ZFS五种存储驱动。

* 写时复制 (CoW)

所有驱动都用到的技术————写时复制，Cow全称copy-on-write，表示只是在需要写时才去复制，这个是针对已有文件的修改场景。比如基于一个image启动多个Container，如果每个Container都去分配一个image一样的文件系统，那么将会占用大量的磁盘空间。而CoW技术可以让所有的容器共享image的文件系统，所有数据都从image中读取，只有当要对文件进行写操作时，才从image里把要写的文件复制到自己的文件系统进行修改。所以无论有多少个容器共享一个image，所做的写操作都是对从image中复制到自己的文件系统的副本上进行，并不会修改image的源文件，且多个容器操作同一个文件，会在每个容器的文件系统里生成一个副本，每个容器修改的都是自己的副本，互相隔离，互不影响。使用CoW可以有效的提高磁盘的利用率。

* 用时分配 （allocate-on-demand）

用时分配是用在原本没有这个文件的场景，只有在要新写入一个文件时才分配空间，这样可以提高存储资源的利用率。比如启动一个容器，并不会因为这个容器分配一些磁盘空间，而是当有新文件写入时，才按需分配新空间。

#### 存储引擎介绍

* AUFS

AUFS (AnotherUnionFS)是一种UnionFS，是文件级的存储驱动。AUFS能透明覆盖一或多个现有文件系统的层状文件系统，把多层合并成文件系统的单层表示。简单来说就是支持将不同目录挂载到同一个虚拟文件下的文件系统。这种文件系统可以一层一层地叠加修改文件。无论底下有多少层都是只读的，只有最上层的文件系统是可写的。当需要修改一个文件时，AUFS创建该文件的一个副本，使用CoW将文件从只读层复制到可写层进行修改，结果也保存在科协层。在Docker中，只读层就是image，可写层就是Container

* OverlayFS

OverlayFS是一种和AUFS很类似的文件系统，与AUFS相比，OverlayFS有以下特性；

1. 更简单地设计;
2. 从Linux 3.18开始，就加入了Linux内核主线;
3. 速度更快

因此，OverlayFS在Docker社区关注提高很快，被很多人认为是AUFS的继承者。Docker的overlay存储驱动利用了很多OverlayFS特性来构建和管理镜像与容器的磁盘结构

从Docker1.12起，Docker也支持overlay2存储驱动，相比于overlay来说，overlay2在inode优化上更加高效，但overlay2驱动只兼容Linux kernel4.0以上的版本

注意: 自从OverlayFS加入kernel主线后，它的kernel模块中的名称就从overlayfs改为overlay了

* OverlayFS （overlay2）镜像分层与共享

overlay驱动只工作在一个lower OverlayFS层之上，因此需要硬链接来实现多层镜像，但overlay2驱动原生地支持多层lower OverlayFS镜像(最多128层)。因此overlay2驱动在合层相关的命令(如build何commit)中提供了更好的性能，与overlay驱动对比，减少了inode消耗

### 4. 容器和VM的区别

### 5. 资源隔离和资源限制相关问题

### 6. docker的namespace

ocker 使用一种称为 namespaces 的技术来为容器提供运行环境的隔离。运行容器时，docker 引擎会为该容器创建一组如下的名称空间

| 名称空间 | 描述                                                                                                |
| -------- | --------------------------------------------------------------------------------------------------- |
| PID      | 提供进程隔离。每个容器中都有独立的进程号                                                            |
| Network  | 提供网络资源隔离。每个容器都有独立的网络设备接口，IPv4, IPv6 协议栈，路由表，防火墙等               |
| IPC      | 提供进程间通信资源隔离。容器中进程间通信仍然使用 Linux 进程间通信方法，信号量，消息队列，共享内存等 |
| Mount    | 提供文件系统隔离。每个容器都有独立的 / 根文件系统                                                   |
| UTS      | 提供容器主机名和域名隔离。每个容器都有独立的主机名和域名，其主机名一般为容器 ID                     |
| User     | 提供用户及用户组隔离。每个容器都有独立的用户，用户组及其相关访问权限                                |

### 7. 常用参数配置

```shell
# 进程相关
--config-file string: 指定配置文件,默认是 `/etc/docker/daemon.json`
-p, --pidfile string: 指定 PID 文件.默认是 `/var/run/docker.pid`
--containerd string: 指定 grpc 地址
--data-root string: 指定 docker 镜像和容器相关文件保存位置.默认是 `/var/lib/docker`
--log-driver string: 指定日志驱动.默认为 `json-file`
--log-level string: 指定日志级别.可选值为 debug,info,warn,error,fatal.默认 `info`

log-opts: 指定日志选项,只能用于配置文件中.`max-size` 指定日志文件大小,`max-file` 指定日志文件个数

# 镜像相关
--insecure-registry list: 指定不安全的镜像仓库地址
--registry-mirror list: 指定安全的镜像仓库.配置文件中为 `registry-mirrors`

# 网络相关
--bip string: 指定 docker0 网桥 IP 地址
--default-gateway string: 子网 IPv4 默认网关
--default-gateway-v6 string: 子网 IPv6 默认网关
--fixed-cidr string: 指定子网 IPv4 地址
--fixed-cidr-v6 string: 指定子网 IPv6 地址
--mtu int: 指定容器最大传输单元
--dns list: 指定容器使用的 DNS 地址

# 调试
-D, --debug: 开启 debug 模式,多用于调试
```

### 8. Control groups

控制组 (cgroups) 以一组进程为目标进行系统资源分配和控制，它提供了如下功能:

* Resource limitation, 资源限制，如内存，CPU 等硬件资源
* Prioritization, 优先级控制
* Accounting, 审计或统计
* Controll, 进程控制，如进程挂起与恢复

系统管理员可更具体地控制对系统资源的分配，优先顺序，拒绝，管理和监控。可更好地根据任务和用户分配硬件资源，提高总体效率。在实践中，系统管理员一般会利用 CGroup 做下面这些事:

* 隔离进程集合，并限制他们所消耗的资源
* 为这组进程分配其足够使用的内存，网络带宽和磁盘存储限制
* 限制访问某些设备

### 9. Union file systems

Union file systems (联合文件系统) 是通过创建层级进行操作的文件系统，它将对文件系统的修改作为一次提交来一层层的叠加。常用的包含 overlay2 aufs

overlay2 采用三层结构:

* lowerdir: 只读层，镜像层
* uperdir: 读写层。创建容器时创建，所有对容器的改动发生在这里
* merged: 容器挂载点，将以上两层进行合并后看到的内容

### 10. docker 开发最佳实践

#### 保持镜像尽可能的小

* 尽量使用 ENV 和 ARG 让人不改或者少改 Dockerfile 即可做构建对应版本的镜像
* 尽量减少 Dockerfile 中单独的 RUN 命令的数量来减少镜像的层数.

> Dockerfile 中指令 RUN,COPY,ADD 会创建新的镜像层，之后镜像层的操作不会影响上一层。因此即便 Dockerfile 中包含 RUN rm -rf xxx 镜像大小也不会减小.

```shell
RUN mkdir /data
RUN touch /data/index.html
```

```shell
RUN mkdir /data && touch /data/index.html
```

* 从适当的基础镜像开始。例如，如果您需要 JDK, 请考虑基于正式的 openjdk 镜像，而不是基于 ubuntu 镜像开始，再将 openjdk 的安装作为 Dockerfile 的一部分
* 可以在官方 Dockerfile 里添加一些常见的排错命令，也可以将二进制及其依赖库添加到镜像中，参见为容器镜像定制安装 Linux 工具. 如

```shell
# 本示例仅作为示例演示,并没有实际意义
# 系统环境 CentOS 7.5.1804,发现 centos:centos7.5.1804 没有 lsof 工具.添加一下
# 首先在宿主机中安装 lsof 工具,并通过 ldd 查看其依赖库 `ldd $(which lsof)`
# 在 centos:centos7.5.1804 镜像启动的容器中查找 lsof 的依赖库,可看到都是存在的.因此直接将二进制文件复制进入即可
FROM centos:centos7.5.1804
ADD lsof /usr/sbin/
```

* 使用多阶段构建。例如，您可以使用 maven 镜像构建 Java 应用程序，然后使用 tomcat 镜像并将构建的 Java 程序复制到正确的位置。这意味着您最终构建的镜像不包括构建所引入的所有库和依赖项。见如下示例

```shell
FROM golang:1.13.6-alpine3.10 as builder
WORKDIR $GOPATH/src/demo
COPY . $GOPATH/src/demo
RUN CGO_ENABLED=0 GOOS=linux go build -o /demo
EXPOSE 8080
ENTRYPOINT ["./demo"]
# 此时我们构建的镜像包括 go 的运行环境,相关源码或依赖文件,二进制可执行文件.镜像较大
# 其中运行环境与源码或依赖对于容器的运行来说都是多余的.
```

```shell
FROM golang:1.13.6-alpine3.10 as builder
WORKDIR $GOPATH/src/demo
COPY . $GOPATH/src/demo
RUN CGO_ENABLED=0 GOOS=linux go build -o /demo

FROM alpine:3.6
COPY --from=builder /demo .
EXPOSE 8080
ENTRYPOINT ["./demo"]
# 此时我们构建镜像仅包含二进制可执行文件.可以理解为 builder 构建完成后就将其丢弃了.镜像较小
```

* 尝试将共享的运行环境或依赖构建为独立的镜像，然后在此基础上构建其它镜像.Docker 只需要加载一次公共层，然后将它们缓存，可以更快的构建。如多个应用都需要自定义的 Tomcat 环境，可以将自定义 Tomcat 构建为单独镜像，而不需要每次构建应用时从最初始自定义 Tomcat 环境开始构建
* 容器时区问题可以在构建镜像的时候安装 tzdate 包，然后声明变量 TZ 即可声明容器运行的时区，或者构建的时候复制宿主机的 /etc/localtime 或者运行的时候挂载宿主机的 /etc/localtime

#### 在何处以及如何保留数据

* 避免将数据存储在容器的可写层中，这会增加容器大小，且效率不如使用 volumes 或 bind 挂载
* bind 挂载多用于开发或测试过程中。对于生产环境，请使用 volumes

### 11. Dockerfile

```shell
docker build [OPTIONS] PATH | URL | -`
```

docker build 命令从 Dockerfile 及上下文构建镜像，构建的上下文是位于 PATH 或 URL 指定的位置的文件集合.PATH 是本地文件系统上的目录，URL 是一个 Git 仓库位置.

上下文是递归处理的。因此 PATH 包括任何子目录，URL 包括仓库及其子模块.

构建过程是 Docker 守护进程进行的。构建的第一件事就是将整个上下文目录及其子目录发送到守护进程中。因此，最好以空目录作为上下文，仅包含 Dockerfile 及 Dockerfile 构建过程中所需要的文件.

#### .dockerignore

通过在上下文中添加 .dockerignore 文件可以排除构建上下文中包含的文件或目录.

.dockerignore 文件使用 # 作为注释，每行包含一个忽略的文件或目录，支持使用 *,?,** 作为通配符匹配。分别表示所有文件，单个字符，目录递归.

在使用 * 忽略所有文件后，可以使用 ! 向上下文中添加被忽略的文件

#### 指令详解

##### ARG

ARG 定义一个变量，用户也可以在构建时使用 ```--build-arg <varname>=<value>``` 传入构建参数。如果该参数没有在 Dockerfile 中定义，则输出警告信息.

```shell
ARG <name>[=<default value>]
```

##### FROM

FROM 指定构建过程的基础镜像，一个有效的 Dockerfile 必须以 FROM 指令启动。它支持使用 ARG 定义的变量

```shell
FROM [--platform=<platform>] <image> [AS <name>]
FROM [--platform=<platform>] <image>[:<tag>] [AS <name>]
FROM [--platform=<platform>] <image>[@<digest>] [AS <name>]
```

##### RUN

RUN 指令从当前镜像最新层执行命令并提交结果。生成的镜像层用于 Dockerfile 中下一步.

```shell
RUN COMMAND
```

##### LABEL

LABEL 指令为镜像打标签

```shell
LABEL <key>=<value> <key>=<value> <key>=<value> ...
```

##### EXPOSE

EXPOSE 指令暴露 Docker 容器监听的端口

```shell
EXPOSE <port> [<port>/<protocol>...]
```

##### ENV

ENV 指令定义环境变量

```shell
ENV <key> <value>
ENV <key>=<value> <key>=<value>
```

##### WORKDIR

WORKDIR 指令定义工作目录，相当于 cd

```shell
WORKDIR /path/to/workdir
```

##### USER

USER 指令指定运行镜像时使用的用户名及可选组

```shell

USER <user>[:<group>]
USER <UID>[:<GID>]
```

##### VOLUME

VOLUME 指定挂载点

```shell
VOLUME ["/path"]
```

##### ADD

* ADD 指令支持拷贝压缩文件到镜像中，并自动解压
* ADD 指令支持从源文件来自指定 URL, 构建容器时会自动下载到指定目录

```shell
ADD [--chown=<user>:<group>] <src>... <dest>
ADD [--chown=<user>:<group>] ["<src>",... "<dest>"]
```

##### COPY

```shell
COPY [--chown=<user>:<group>] <src>... <dest>
COPY [--chown=<user>:<group>] ["<src>",... "<dest>"]
```

##### ENTRYPOINT

ENTRYPOINT 指令设置容器启动后要执行的命令。使用 --entrypoint 指令进行替换。它有两种形式

--entrypoint 指令指定的命令会覆盖原有所有命令及参数

```shell
# exec 形式,是推荐的形式
ENTRYPOINT ["executable", "param1", "param2"]

# shell 形式. 容器启动后默认执行 `/bin/sh -c command param1 param2`,这种方式启动的容器会自动回收孤儿进程与僵尸进程
ENTRYPOINT command param1 param2
```

##### CMD

CMD 指令一般用来设置容器启动后执行命令的默认参数。如果是参数，则必须指定 ENTRYPOINT 指令。如果包含多个 CMD 指令，以最后一个为准.

docker run [command] 时会覆盖 CMD 指令内容，对 ENTRYPOINT 无影响

CMD 指令有 3 种形式

```shell
# exec 形式,是推荐的形式.该形式不会支持管道或变量替换.容器中 `executable` 进程 ID 为 1
CMD ["executable","param1","param2"]

# shell 形式. 容器会默认使用 `/bin/sh -c command param1 param2` 启动容器,这种方式启动的容器会自动回收孤儿进程与僵尸进程
CMD command param1 param2

# 作为 ENTRYPOINT 的默认参数,此时 ENTRYPOINT 必须使用 exec 形式
CMD ["param1","param2"]
```

下表列出了不同 ENTRYPOINT 与 CMD 指令在容器启动时运行的命令:

|                          | No ENTRYPOINT  | ENTRYPOINT exec_entry p_entry | ENTRYPOINT ["exec_entry", "p_entry"]       |
| ------------------------ | -------------- | ----------------------------- | ------------------------------------------ |
| No CMD                   | error          | /bin/sh -c exec_entry p_entry | exec_entry p_entry                         |
| CMD ["exec_cmd p_cmd"]   | exec_cmd p_c   | /bin/sh -c exec_entry p_entry | exec_entry p_entry exec_cmd p_cmd          |
| CMD ["p1_cmd", "p2_cmd"] | p1_cmd p2_cmd  | /bin/sh -c exec_entry p_entry | exec_entry p_entry p1_cmd p2_cmd           |
| CMD exec_cmd p_cmd       | exec_cmd p_cmd | /bin/sh -c exec_entry p_entry | exec_entry p_entry /bin/sh -c exec_cmd_cmd |

## Kubernetes

### 1. Kubernetes Components

![avatar](https://d33wubrfki0l68.cloudfront.net/2475489eaf20163ec0f54ddc1d92aa8d4c87c96b/e7c81/images/docs/components-of-kubernetes.svg)

> [Kubernetes Components](https://kubernetes.io/docs/concepts/overview/components/)

#### Control Plane Components

* kube-apiserver

Kubernetes API，集群的统一入口，各组件协调者，以 RESTful API提供接口服务，所有对象资源的增删改查和监听操作都交给 APIServe 处理后再提交给 Etcd 存储。

* etcd

分布式键值存储系统。用于保存集群状态数据，比如 Pod、Service 等对象信息。

* kube-scheduler

根据调度算法为新创建的 Pod 选择一个 Node 节点，可以任意部署,可以部署在同一个节点上,也可以部署在不同的节点上。

* kube-controller-manager/cloud-controller-manager

处理集群中常规后台任务，一个资源对应一个控制器，而 ControllerManager 就是负责管理这些控制器的。

#### Node Components

* kubelet

kubelet 是 Master 在 Node 节点上的 Agent，管理本机运行容器的生命周期，比如创建容器、Pod 挂载数据卷、下载 secret、获取容器和节点状态等工作。kubelet 将每个 Pod 转换成一组容器。

* kube-proxy

在 Node 节点上实现 Pod 网络代理，维护网络规则和四层负载均衡工作。

* Container Runtime

#### 核心概念

* Pod
Pod是若干相关容器的组合，Pod包含的容器运行在同一台宿主机上，这些容器使用相同的网络命名空间、IP地址和端口，相互之间能通过localhost来发现和通信。另外这些容器可以共享一块存储卷空间。在kubernetes中创建、调度和管理的最小单位是Pod，而不是容器，Pod通过提供更高层次的抽象，提供了更加灵活的部署和管理模式
* RC
Replication Controller用来管理控制Pod副本（Replica，或者称为实例）Replication Controller确保任何时候kubernetes集群中有指定数量的Pod副本在运行。如果少于指定数量的副本，Replication Controller会启动新的Pod副本，反之会杀死多余的副本以保证数量不变，另外Replication Controller是弹性伸缩、滚动升级的实现核心
* Service
Service 是真实应用服务的抽象，定义了Pod的逻辑集合和访问Pod集合的策略。Service将代理Pod对外表现为一个单一访问接口，外部不需要了解后端Pod如何运行，并且提供了一套简化的服务代理和发现机制。
* Label
Label 是用于区分Pod、Service、Replication Controller的Key/Value对，实际上，kubernetes中的任意API对象都可以通过label进行标示。每个API对象可以有多个label，但是每个label的KEY只能对应一个value。label是service和replication Controller运行的基础，他们通过label来关系Pod，相比于强绑定模型，这是一种非常友好的耦合关系
* Node
kubernetes属于主从分布式集群架构，kubernetes NODE (简称为Node，早期版本叫做Minion)运行并管理容器。 Node操作Kubernetes的操作单元，用来分配Pod，Pod最终运行在Node上，node可以认为是Pod的宿主机
* kube-apiserver
作为kubernetes 系统的入口，其封装了核心对象的增删改查操作，以REST API接口方式提供给外部客户和内部组件调用，它维护的REST对象将持久化到ETCD中
* kube-scheduler
负责集群的资源调度，为新建的Pod分配机器
* kube-controller-manager
负责执行各个控制器，目前已经实现很多控制器来保证kubernetes的正常运行（主要是rc node service deployment等）
* kubelet
负责管控容器，kubelet会从kubernetes API Server接收Pod的创建请求，启动和停止容器，监控容器运行状态并汇报给kubernetes API Server
* Kubernetes Proxy
负责为Pod创建代理服务，kubernetes proxy会从kubernetes API Server获取所有的service，并根据Service信息创建代理服务，实现Service到Pod的请求和路由转发，从而实现Kubernetes层级的虚拟转发网络

### 2. Pod 的生命周期

> [Pod 的生命周期](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/)

* Pod 生命期: 和一个个独立的应用容器一样，Pod 也被认为是相对临时性（而不是长期存在）的实体。 Pod 会被创建、赋予一个唯一的 ID（UID）， 并被调度到节点，并在终止（根据重启策略）或删除之前一直运行在该节点。
* Pod 阶段: Pending -> Running -> Succeeded/Failed
* 容器状态: Waiting（等待）、Running（运行中）和 Terminated（已终止）
* 容器重启策略: Always、OnFailure 和 Never
* Pod 状况
  * PodScheduled：Pod 已经被调度到某节点；
  * ContainersReady：Pod 中所有容器都已就绪；
  * Initialized：所有的 Init 容器 都已成功启动；
  * Ready：Pod 可以为请求提供服务，并且应该被添加到对应服务的负载均衡池中。
* 容器探针
  * livenessProbe：指示容器是否正在运行。如果存活态探测失败，则 kubelet 会杀死容器， 并且容器将根据其重启策略决定未来。如果容器不提供存活探针， 则默认状态为 Success。
  * readinessProbe：指示容器是否准备好为请求提供服务。如果就绪态探测失败， 端点控制器将从与 Pod 匹配的所有服务的端点列表中删除该 Pod 的 IP 地址。 初始延迟之前的就绪态的状态值默认为 Failure。 如果容器不提供就绪态探针，则默认状态为 Success。
  * startupProbe: 指示容器中的应用是否已经启动。如果提供了启动探针，则所有其他探针都会被 禁用，直到此探针成功为止。如果启动探测失败，kubelet 将杀死容器，而容器依其 重启策略进行重启。 如果容器没有提供启动探测，则默认状态为 Success。
* Pod 的终止： 由于 Pod 所代表的是在集群中节点上运行的进程，当不再需要这些进程时允许其体面地 终止是很重要的。一般不应武断地使用 KILL 信号终止它们，导致这些进程没有机会 完成清理操作。

#### 面试回答

首先Pod被创建，紧接着Pod被调度到Node进行部署运行，一旦被分配到Node后，就不会离开这个Node，直到它被删除，生命周期完结
Pod声明周期有以下几个阶段

* Pending: Pod已经被创建，但是一个或者多个容器还未创建，包括Pod调度阶段，以及容器镜像的下载过程
* Running: Pod已经被调度到Node，所有容器已经创建，并且至少有一个容器在运行或者正在容器
* Succeeded: Pod中所有所有容器正常退出
* Failed: Pod中所有容器退出，至少有一个容器是一次退出的

### 3. pod 重启策略

```shell
Pod重启策略指的是当Pod中的容器停止退出后，重启容器的策略。
重启策略是通过Pod定义中的.spec.restartPolicy进行设置，支持一下三种策略
always  当容器停止退出后，总是从启容器，默认策略
OnFailure  当容器终止异常退出(非0时)，才重启容器
Never   当容器终止退出时，从不重启容器
Pod是Running状态，包含一个容器，容器正常退出；
如果重启策略是Always，那么会重启容器，Pod保持running状态
如果重启策略是OnFailure，Pod进入Successded阶段
如果重启策略是never，Pod进入Succeeded
Pod是Running阶段，含有一个容器，容器异常退出；
如果重启策略是Always，那么会重启容器，Pod保持Running阶段
如果重启策略是OnFailure，Pod保持Running阶段
如果重启策略是Never，Pod进入Failed阶段
Pod是Running阶段，含有2个容器，其中一个异常退出；
如果重启策略是Always，那么会重启容器，Pod保持running阶段
如果重启策略是OnFailure，Pod保持running阶段
如果重启策略是Never，Pod保持running阶段
Pod是Running阶段，含有2个容器，2个容器都异常退出；
如果重启策略是Always，那么会重启容器，Pod保持running阶段
如果重启策略是OnFailure，Pod保持running阶段
如果重启策略是Never，Pod进入Failed阶段
```

### 4. k8s 生命周期和回调函数

kubernetes提供了回调函数，在容器的生命周期的特定阶段执行调用，比如容器在停止前系统执行某项操作，就可以注册相应的钩子函数。目前提供的生命周期回调函数如下

* PostStart  在容器创建成功后调用该回调函数
* PreStop    在容器被终止前调用该回调函数

### 5. k8s flannel 通信原理

数据从源容器中出发后，经由所在主机的docker0虚拟网卡转发到flannel0虚拟网卡上，flannel服务监听在网卡的另一端。 Flannel通过ETCD服务维护了一张节点间的路由表，详细记录了各节点的子网网段
源主机的flanneld服务奖原本的数据包内容UDP封装后根据自己的路由表传递给目的节点的flannel服务，数据到达以后被解包，然后直接进入目的节点的flannel0虚拟网卡，然后被转发到目的主机的docker0虚拟网卡，最后就像本机容器通信一样，私用docker0路由到达目的容器
docker需要配置bip的一个配置引用

### 6. k8s 拉取镜像策略

* always 每次都下载最新的镜像
* never  只使用本地镜像，从不下载
* ifNotPresent 只有当本地没有的时候才下载镜像

### 7. 自定义检查Pod

* Liveness Probe 用于容器的自定义健康检查，如果Liveness Probe检查失败，kubernetes将杀死容器，然后根据Pod的重启策略来决定是否重启容器
* Readiness Probe 用于容器的自定义准备状况检查，如果Readiness Probe检查失败，Kubernetes将会把Pod从服务代理的分发后端移除，即不会分发请求给该Pod
* Probe支持以下三种健康检查
* ExecAction  在容器中执行检查的命令，当命令执行成功(返回码为0)则检查成功
* TCPSocketAction  对于容器中的指定TCP端口进行检查，当TCP端口被占用，检查成功
* HTTPGetAction  发生一个HTTP请求，当返回码介于200-400之间，则检查成功

### 8. Pod与容器

在Docker中，容器是最小处理单位，增删改查的对象是容器，容器是一种虚拟化技术，容器之间是隔离的，隔离是基于Linux Namespace实现的， Pod包含一个或多个相关容器，Pod可以认为是容器的一种延伸扩展，一个Pod也是隔离体，而Pod包含的一组容器又是共享的(当前共享的Linux Namespace包含PID、Network、IPC(消息队列和内存)和UTS（主机与域名）)。Pod中的容器可以访问相同的数据卷来实现文件系统的共享，所以kubernetes中国的数据卷是Pod级别的，而不是容器级别的
这样的设计有以下好处
1.透明性:将Pod内的容器向基础设施可见，底层系统能向容器提供如进程管理和资源监控等服务，这样能给用户带来极大便利
2.解绑软件的依赖: 这样但个容器可以独立的重建和重新部署，可以实现独立容器的实时更新
3.易用性: 用户不需要运行自己的进程管理器，也不需要负责信号量和退出码的传递等。
4.高效性: 因为底层设备负责更多的管理，容器因而能更轻量化

### 9. kube-proxy原理

kube-proxy是Kubernetes的核心组件，部署在每个Node节点上，它是实现Kubernetes Service的通信与负载均衡机制的重要组件; kube-proxy负责为Pod创建代理服务，从apiserver获取所有server信息，并根据server信息创建代理服务，实现server到Pod的请求路由和转发，从而实现K8s层级的虚拟转发网络。

在k8s中，提供相同服务的一组pod可以抽象成一个service，通过service提供的统一入口对外提供服务，每个service都有一个虚拟IP地址（VIP）和端口号供客户端访问。kube-proxy存在于各个node节点上，主要用于Service功能的实现，具体来说，就是实现集群内的客户端pod访问service，或者是集群外的主机通过NodePort等方式访问service。在当前版本的k8s中，kube-proxy默认使用的是iptables模式，通过各个node节点上的iptables规则来实现service的负载均衡，但是随着service数量的增大，iptables模式由于线性查找匹配、全量更新等特点，其性能会显著下降。从k8s的1.8版本开始，kube-proxy引入了IPVS模式，IPVS模式与iptables同样基于Netfilter，但是采用的hash表，因此当service数量达到一定规模时，hash查表的速度优势就会显现出来，从而提高service的服务性能。

kube-proxy负责为Service提供cluster内部的服务发现和负载均衡，它运行在每个Node计算节点上，负责Pod网络代理, 它会定时从etcd服务获取到service信息来做相应的策略，维护网络规则和四层负载均衡工作。在K8s集群中微服务的负载均衡是由Kube-proxy实现的，它是K8s集群内部的负载均衡器，也是一个分布式代理服务器，在K8s的每个节点上都有一个，这一设计体现了它的伸缩性优势，需要访问服务的节点越多，提供负载均衡能力的Kube-proxy就越多，高可用节点也随之增多。

service是一组pod的服务抽象，相当于一组pod的LB，负责将请求分发给对应的pod。service会为这个LB提供一个IP，一般称为cluster IP。kube-proxy的作用主要是负责service的实现，具体来说，就是实现了内部从pod到service和外部的从node port向service的访问。

简单来说:

> kube-proxy其实就是管理service的访问入口，包括集群内Pod到Service的访问和集群外访问service。
> kube-proxy管理sevice的Endpoints，该service对外暴露一个Virtual IP，也成为Cluster IP, 集群内通过访问这个Cluster IP:Port就能访问到集群内对应的serivce下的Pod。
> service是通过Selector选择的一组Pods的服务抽象，其实就是一个微服务，提供了服务的LB和反向代理的能力，而kube-proxy的主要作用就是负责service的实现。
> service另外一个重要作用是，一个服务后端的Pods可能会随着生存灭亡而发生IP的改变，service的出现，给服务提供了一个固定的IP，而无视后端Endpoint的变化。

### 10. 创建pod启动整个流程

1. 客户端提交创建请求，可以通过API Server的Restful API，也可以使用kubectl命令行工具。支持的数据类型包括JSON和YAML
2. API Server处理用户请求，存储Pod数据到ETCD
3. 调度器通过API Server查看未绑定的Pod。尝试为Pod分配主机
4. 过滤主机(调度预选):调度器用一组规则过滤到不符合要求的主机。比如Pod指定了所需要的的资源量，那么可用资源比Pod需要的资源量小的主机会被过滤掉
5. 主机打分(调度优选):对第一个筛选出符合要求的主机进行打分，在主机打分阶段，调度器会考虑一些整体优化策略，比如把一个Replication Controller的副本分布在不同的主机上，使用低负载的主机等
6. 选择主机: 选择打分最高的主机，进行binding操作，结果存储到etcd中
7. kubelet根据调度结果执行Pod创建操作: 绑定成功后，scheduler会调用API Server的API在etcd中创建一个boundpod对象，描述在一个工作节点上绑定运行的所有Pod信息。运行在每个工作节点上的kubelet也会定期与etcd同步boundpod信息，一旦发现应该在该工作节点上运行的boundpod对象没有更新，则调用Docker API创建并启动Pod内的容器

### 11. 滚动更新策略

当集群中的某个服务需要升级时，我们需要停止目前与该服务相关的所有Pod,然后重新拉取镜像并启动。如果集群规模比较大，则这个工作就变成了一个挑战，而且先全部停止然后逐步升级的方式会导致长时间的服务不可用。Kubernetes提供了rolling-update滚动升级功能来解决上述问题

滚动升级通过执行kubectl rolling-update命令一键完成，该命令创建了一个新的RC，然后自动控制旧的RC中的Pod副本数量逐渐减少到0，同时新的RC中的Pod副本的数量从0逐步增加到目标值，最终实现了Pod的升级。系统会要求新的RC和旧的RC在相同的命名空间下

### 12. master node 有哪些服务

#### k8s master上

kubectl
apiserver
kube-scheduler
kube-controller-manager
etcd
flannel

#### k8s node有哪些

kubelet
kube-proxy
Docker
flannel

## CI/CD

## Network

### 1. HTTP HTTPS 区别

* HTTP 三次握手建立连接
* HTTPS （三次握手中）客户端请求 HTTPS 连接后，服务器返回证书（公钥），客户端生成随机对称密钥，使用公钥对密钥加密，再发送给服务器加密后的对称密钥，最后两边通过对称密钥加密的密文通信
* HTTPS 是HTTP 协议的安全版本，HTTP 协议的数据传输是明文的，是不安全的，HTTPS 使用了 SSL/TLS 协议进行了加密处理。
* HTTP 和 HTTPS 使用连接方式不同，默认端口也不一样，HTTP 是80，HTTPS 是443

### 2. tcp 三次握手，状态

![avatar](https://camo.githubusercontent.com/02987d37d9ecf26ec08473c5b3f530836c490a06a598ee4b0d3cd6032e430513/68747470733a2f2f67697465652e636f6d2f6875696875742f696e746572766965772f7261772f6d61737465722f696d616765732f5443502545342542382538392545362541432541312545362538462541312545362538392538422545352542422542412545372541422538422545382542462539452545362538452541352e706e67)

1. 客户端发送 SYN 给服务器，说明客户端请求建立连接；
2. 服务端收到客户端发的 SYN，并回复 SYN+ACK 给客户端（同意建立连接）；
3. 客户端收到服务端的 SYN+ACK 后，回复 ACK 给服务端（表示客户端收到了服务端发的同意报文）；
4. 服务端收到客户端的 ACK，连接已建立，可以数据传输。

### 3. 四层和七层负载均衡区别

1. 四层负载均衡工作在OSI模型中的四层，即传输层。四层负载均衡只能根据报文中目标地址和源地址对请求进行转发，而无法修改或判断所请求资源的具体类型，然后经过负载均衡内部的调度算法转发至要处理请求的服务器。四层负载均衡单纯的提供了终端到终端的可靠连接，并将请求转发至后端，连接至始至终都是同一个。LVS就是很典型的四层负载均衡。
2. 七层负载均衡工作在OSI模型的第七层应用层，所以七层负载均衡可以基于请求的应用层信息进行负载均衡，例如根据请求的资源类型分配到后端服务器，而不再是根据IP和端口选择。七层负载均衡的功能更丰富更灵活，也能使整个网络更智能。如上图所示，在七层负载均衡两端(面向用户端和服务器端)的连接都是独立的。
3. 简言之，四层负载均衡就是基于IP+端口实现的。七层负载均衡就是通过应用层资源实现的。

### 4. OSI 七层模型

* 物理层：主要定义物理设备标准，如网线的接口类型、光纤的接口类型、各种传输介质的传输速率等。它的主要作用是传输比特流（就是由1、0转化为电流强弱来进行传输,到达目的地后在转化为1、0，也就是我们常说的数模转换与模数转换），这一层的数据叫做比特。
* 数据链路层：定义了如何让格式化数据以进行传输，以及如何让控制对物理介质的访问，这一层通常还提供错误检测和纠正，以确保数据的可靠传输。
* 网络层：在位于不同地理位置的网络中的两个主机系统之间提供连接和路径选择，Internet的发展使得从世界各站点访问信息的用户数大大增加，而网络层正是管理这种连接的层。
* 传输层：定义了一些传输数据的协议和端口号（WWW端口80等），如：TCP（传输控制协议，传输效率低，可靠性强，用于传输可靠性要求高，数据量大的数据），UDP（用户数据报协议，与TCP特性恰恰相反，用于传输可靠性要求不高，数据量小的数据，如QQ聊天数据就是通过这种方式传输的）， 主要是将从下层接收的数据进行分段和传输，到达目的地址后再进行重组，常常把这一层数据叫做段。
* 会话层：通过传输层（端口号：传输端口与接收端口）建立数据传输的通路，主要在你的系统之间发起会话或者接受会话请求（设备之间需要互相认识可以是IP也可以是MAC或者是主机名）。
* 表示层：可确保一个系统的应用层所发送的信息可以被另一个系统的应用层读取。例如，PC程序与另一台计算机进行通信，其中一台计算机使用扩展二一十进制交换码（EBCDIC），而另一台则使用美国信息交换标准码（ASCII）来表示相同的字符。如有必要，表示层会通过使用一种通格式来实现多种数据格式之间的转换。
* 应用层： 是最靠近用户的OSI层，这一层为用户的应用程序（例如电子邮件、文件传输和终端仿真）提供网络服务。

### 5. TCP/IP 四层

* 应用层：负责向用户提供应用程序，比如HTTP、FTP、Telnet、DNS、SMTP等。
* 传输层：负责对报文进行分组和重组，并以TCP或UDP协议格式封装报文。
* 网络层：负责路由以及把分组报文发送给目标网络或主机。
* 链路层：负责封装和解封装IP报文，发送和接受ARP/RARP报文等 下面的是两者之间的对比:

两者比较图：

![avatar](https://raw.githubusercontent.com/bernylinville/DevOps-Interview/main/image/tcp-ip.png)

### 6. 常见的http状态码

* 503 服务不可用
* 404 未找到
* 499 客户端主动断开了连接
* 500 服务器内部错误
* 504 网关超时
* 301 永久转移
* 302 临时转移

### 7. ssl 握手过程

基于RSA握手和密钥交换的客户端验证服务器为实例详解TLS/SSL握手过程

![avatar](https://raw.githubusercontent.com/bernylinville/DevOps-Interview/main/image/ssl1.png)
![avatar](https://raw.githubusercontent.com/bernylinville/DevOps-Interview/main/image/ssl2.png)

1.client_hello
客户端发起请求，以明文传输请求信息，包含版本信息，加密套件候选列表，压缩算法候选列表，随机数，扩展字段等信息

2.server_hello+server_certificate+server_hello_done

server_hello 服务器返回协商的信息结果，包括使用的协议版本version,选择的加密套件ciphersuite，选择的压缩算法compression method、随机数random_S等，其中随机数用于后续的密钥协商；
server_certificates 服务端配置对应的证书链，用于身份验证与密钥交换；
server_hello_done 通知客户端server_hello信息发送结束；
3.证书效验
客户端验证证书的合法性，如果验证通过才会进行后续通信，否则根据错情情况下不同做出提示和操作

4.client_key_exchange+change_cipher_spec+encrypted_handshake_message
(a) client_key_exchange，合法性验证通过之后，客户端计算产生随机数字 Pre-master，并用证书公钥加密，发送给服务器;
(b) 此时客户端已经获取全部的计算协商密钥需要的信息：两个明文随机数 random_C 和 random_S 与自己计算产生的 Pre-master，计算得到协商密钥;

```shell
    enc_key=Fuc(random_C, random_S, Pre-Master)
```

(c) change_cipher_spec，客户端通知服务器后续的通信都采用协商的通信密钥和加密算法进行加密通信;
(d) encrypted_handshake_message，结合之前所有通信参数的 hash 值与其它相关信息生成一段数据，采用协商密钥 session secret 与算法进行加密，然后发送给服务器用于数据与握手验证;

5.change_cipher_spec+encrypted_handshake_message
(a) 服务器用私钥解密加密的 Pre-master 数据，基于之前交换的两个明文随机数 random_C 和 random_S，计算得到协商密钥:enc_key=Fuc(random_C, random_S, Pre-Master);
(b) 计算之前所有接收信息的 hash 值，然后解密客户端发送的 encrypted_handshake_message，验证数据和密钥正确性;
(c) change_cipher_spec, 验证通过之后，服务器同样发送 change_cipher_spec 以告知客户端后续的通信都采用协商的密钥与算法进行加密通信;
(d) encrypted_handshake_message, 服务器也结合所有当前的通信参数信息生成一段数据并采用协商密钥 session secret 与算法加密并发送到客户端;

6.握手结束
客户端计算所有接收信息的 hash 值，并采用协商密钥解密 encrypted_handshake_message，验证服务器发送的数据和密钥，验证通过则握手完成;
7.加密通信
开始使用协商密钥与算法进行加密通信。

关于SSL加密协议可以参考[SSL/TLS握手过程](https://www.cnblogs.com/barrywxx/p/8570715.html)

### 8. tcp和udp的区别

* TCP 是面向连接的，通信之前需要进行三次握手，建立连接，通信结束后需要四次挥手，断开连接；UDP 是无连接的，即发送数据之前不需要建立连接
* TCP 提供可靠的服务，以收到确认，超时重传，重新排序等机制提供数据可靠性；UDP 不保证数据的可靠性
* TCP 面向字节流，实际上是 TCP 把数据看成一连串无结构的字节流；UDP 是面向报文的
* 每一条 TCP 连接只能是点到点的；UDP 支持一对一，一对多，多对一和多对多的交互通信
* TCP 的逻辑通信信道是全双工的可靠信道；UDP 则是不可靠信道
* TCP 相对于 UDP 来说要求系统资源较多

相同点
UDP协议和TCP协议都是传输层协议

TCP (Transmission Control Protocol，传输控制协议)提供的是面向连接，可靠的字节流服务。即客户端和服务器交换数据前，必须先在双方之间建立一个TCP连接，之后才能传输数据。并且提供超时重发，丢弃重复数据，校验数据，流量控制等功能，保证数据能从一端传到另外一端

UDP (User Data Protocol,用户数据报协议)是一个简单的面向数据报的传输层协议。它不提供可靠性，只是把应用程序传给IP层的数据报发送出去，但是不能保证它们能到达目的地。由于UDP在传输数据报前不用再客户端和服务器之间建立一个连接，且没有超时重发等机制，所以传输速度很快。

不同点

报头不同
特点不同
协议不同
UDP数据报最大长度64K(包含UDP头部)，如果数据长度超过64K就需要在应用层手动分包，UDP无法保证包序，需要在应用层进行编号

特点
1.无连接: 知道对端的IP和端口号就可以进行传输，不需要建立连接
2.不可靠：没有确认机制，没有重传机制；如果因为网络故障该段无法发到对方，UDP协议层也不会给应用层返回任何错误信息
3.面向数据报：不能够灵活的控制读写数据的次数和数量，应用层交给UDP多长的报文，UDP原样发送，既不会拆分，也不会合并
4.数据不够灵活，但是能够明确区分两个数据报，避免粘包问题

UDP相关协议
NFS
TFTP
DHCP
BOOTP
DNS

TCP则需要建立TCP三次握手，在断开请求时需要TCP四次断开

特点
1.可靠性传输
2.面向字节流

TCP相关协议
HTTP
HTTPS
SSH
Telnet
FTP
SMTP

### 9. tcp 三次握手

在TCP/IP协议中，TCP协议提供可靠的连接服务，采用三次握手建立一个连接

![avatar](https://raw.githubusercontent.com/bernylinville/DevOps-Interview/main/image/tcp3.jpeg)

握手之前主动打开连接的客户端结束CLOSED阶段，被动打开的服务器端也结束CLOSED阶段，并进入LISTEN阶段。随后开始“三次握手”：

(1) 首先客户端向服务器端发送一段TCP报文，其中：
标记位为SYN，表示“请求建立新连接”;序号为Seq=X（X一般为1）；随后客户端进入SYN-SENT阶段。

(2) 服务器端接收到来自客户端的TCP报文之后，结束LISTEN阶段。并返回一段TCP报文，其中：
标志位为SYN和ACK，表示“确认客户端的报文Seq序号有效，服务器能正常接收客户端发送的数据，并同意创建新连接”（即告诉客户端，服务器收到了你的数据）；
序号为Seq=y；确认号为Ack=x+1，表示收到客户端的序号Seq并将其值加1作为自己确认号Ack的值；随后服务器端进入SYN-RCVD阶段。

(3) 客户端接收到来自服务器端的确认收到数据的TCP报文之后，明确了从客户端到服务器的数据传输是正常的，结束SYN-SENT阶段。并返回最后一段TCP报文。其中：

标志位为ACK，表示“确认收到服务器端同意连接的信号”（即告诉服务器，我知道你收到我发的数据了）；
序号为Seq=x+1，表示收到服务器端的确认号Ack，并将其值作为自己的序号值；
确认号为Ack=y+1，表示收到服务器端序号Seq，并将其值加1作为自己的确认号Ack的值；
随后客户端进入ESTABLISHED阶段。服务器收到来自客户端的“确认收到服务器数据”的TCP报文之后，明确了从服务器到客户端的数据传输是正常的。结束SYN-SENT阶段，进入ESTABLISHED阶段。

在客户端与服务器端传输的TCP报文中，双方的确认号Ack和序号Seq的值，都是在彼此Ack和Seq值的基础上进行计算的，这样做保证了TCP报文传输的连贯性。一旦出现某一方发出的TCP报文丢失，便无法继续”握手”，以此确保了”三次握手”的顺利完成。

### 10. 为什么要进行第三次握手？

为了防止服务器端开启一些无用的连接增加服务器开销以及防止已失效的连接请求报文段突然又传送到了服务端，因而产生错误。

由于网络传输是有延时的(要通过网络光纤和各种中间代理服务器)，在传输的过程中，比如客户端发起了SYN=1创建连接的请求(第一次握手)。

如果服务器端就直接创建了这个连接并返回包含SYN、ACK和Seq等内容的数据包给客户端，这个数据包因为网络传输的原因丢失了，丢失之后客户端就一直没有接收到服务器返回的数据包。

客户端可能设置了一个超时时间，时间到了就关闭了连接创建的请求。再重新发出创建连接的请求，而服务器端是不知道的，如果没有第三次握手告诉服务器端客户端收的到服务器端传输的数据的话，

服务器端是不知道客户端有没有接收到服务器端返回的信息的。

### 11. TCP 四次挥手

所谓的四次挥手即TCP连接的释放(解除)。连接的释放必须是一方主动释放，另一方被动释放。以下为客户端主动发起释放连接的图解

![avatar](https://raw.githubusercontent.com/bernylinville/DevOps-Interview/main/image/tcp4.jpeg)

挥手之前主动释放连接的客户端结束ESTABLISHED阶段。随后开始“四次挥手”：
(1) 首先客户端想要释放连接，向服务器端发送一段TCP报文，其中：
标记位为FIN，表示“请求释放连接“；序号为Seq=U；随后客户端进入FIN-WAIT-1阶段，即半关闭阶段。并且停止在客户端到服务器端方向上发送数据，但是客户端仍然能接收从服务器端传输过来的数据。注意：这里不发送的是正常连接时传输的数据(非确认报文)，而不是一切数据，所以客户端仍然能发送ACK确认报文。

(2) 服务器端接收到从客户端发出的TCP报文之后，确认了客户端想要释放连接，随后服务器端结束ESTABLISHED阶段，进入CLOSE-WAIT阶段（半关闭状态）并返回一段TCP报文，其中：

标记位为ACK，表示“接收到客户端发送的释放连接的请求”；序号为Seq=V；确认号为Ack=U+1，表示是在收到客户端报文的基础上，将其序号Seq值加1作为本段报文确认号Ack的值；随后服务器端开始准备释放服务器端到客户端方向上的连接。客户端收到从服务器端发出的TCP报文之后，确认了服务器收到了客户端发出的释放连接请求，随后客户端结束FIN-WAIT-1阶段，进入FIN-WAIT-2阶段

前”两次挥手”既让服务器端知道了客户端想要释放连接，也让客户端知道了服务器端了解了自己想要释放连接的请求。于是，可以确认关闭客户端到服务器端方向上的连接了

(3) 服务器端自从发出ACK确认报文之后，经过CLOSED-WAIT阶段，做好了释放服务器端到客户端方向上的连接准备，再次向客户端发出一段TCP报文，其中：
标记位为FIN，ACK，表示“已经准备好释放连接了”。注意：这里的ACK并不是确认收到服务器端报文的确认报文。序号为Seq=W；确认号为Ack=U+1；表示是在收到客户端报文的基础上，将其序号Seq值加1作为本段报文确认号Ack的值。随后服务器端结束CLOSE-WAIT阶段，进入LAST-ACK阶段。并且停止在服务器端到客户端的方向上发送数据，但是服务器端仍然能够接收从客户端传输过来的数据。

(4) 客户端收到从服务器端发出的TCP报文，确认了服务器端已做好释放连接的准备，结束FIN-WAIT-2阶段，进入TIME-WAIT阶段，并向服务器端发送一段报文，其中：
标记位为ACK，表示“接收到服务器准备好释放连接的信号”。序号为Seq=U+1；表示是在收到了服务器端报文的基础上，将其确认号Ack值作为本段报文序号的值。确认号为Ack=W+1；表示是在收到了服务器端报文的基础上，将其序号Seq值作为本段报文确认号的值。随后客户端开始在TIME-WAIT阶段等待2MSL

为什么要客户端要等待2MSL呢？

服务器端收到从客户端发出的TCP报文之后结束LAST-ACK阶段，进入CLOSED阶段。由此正式确认关闭服务器端到客户端方向上的连接。

客户端等待完2MSL之后，结束TIME-WAIT阶段，进入CLOSED阶段，由此完成“四次挥手”。

后“两次挥手”既让客户端知道了服务器端准备好释放连接了，也让服务器端知道了客户端了解了自己准备好释放连接了。于是，可以确认关闭服务器端到客户端方向上的连接了，由此完成“四次挥手”。

与“三次挥手”一样，在客户端与服务器端传输的TCP报文中，双方的确认号Ack和序号Seq的值，都是在彼此Ack和Seq值的基础上进行计算的，这样做保证了TCP报文传输的连贯性，一旦出现某一方发出的TCP报文丢失，便无法继续”挥手”，以此确保了”四次挥手”的顺利完成。

### 12. TCP连接状态

* LISTEN 监听来自远方的TCP端口的连接请求
* SYN-SENT 再发送连接请求后等等匹配的连接请求(客户端)
* SYN-RECEIVED 再收到和发送一个连接请求后等等对方连接请求的确认 (服务器)
* ESTABLISHED 代表一个打开的连接
* FIN-WAIT-1 等待远程TCP连接中端请求，或先前的连接中断请求的确认
* FIN-WAIT-2 从远程TCP等待连接中断请求
* CLOSING 等待远程TCP对连接中断的确认
* LAST-ACK 等待原来的发向远程TCP的连接中断请求的确认
* TIME-WAIT 等待足够的实际以确保远程TCP连接收到中断请求的确认
* CLOSED 没有任何连接状态

```shell
# 查看服务器连接
ss -ant | awk 'NR>1 {++s[$1]} END {for(k in s) print k,s[k]}'
```

主动端可能出现的状态: FIN_WAIT1、FIN_WAIT2、CLOSING、TIME_WAIT
被动端可能出现的状态: CLOSE_WAIT LAST_ACK

### 13. 最后一次ACK包丢失会进入什么样的一个状态

### 14. 关于TIME_WAIT状态等待2MSL解决什么问题

### 15. DNS使用的到协议（TCP/UDP分别在什么情况下使用）

### 16. 广播风暴产生的原因及解决方法

### 17. TLS/SSL处于OSI哪一层

### 18. 为什么需要三次握手，两次不行吗

主要是为了防止已失效的请求报文段突然又传送到了服务端而产生连接的误判，造成服务端资源 / 时间的浪费.

### 19. 为什么建立连接是三次握手，而关闭连接却是四次挥手呢

* 建立连接
因为服务端在 LISTEN 状态下，收到建立连接请求的 SYN 报文后，把 ACK 和 SYN 放在一个报文里发送给客户端.

* 关闭连接
当收到对方的 FIN 报文时，仅表示对方不再发送数据但还能接收收据，我们也未必把全部数据都发给了对方，所以我们可以立即关闭，也可以发送一些数据给对方后，再发送 FIN 报文给对方表示同意关闭连接。因此关闭连接的 ACK 和 FIN 一般会分开发送.

### 20. 三次握手有什么缺陷

伪造 IP 大量的向 server 发送 TCP 连接请求报文包，从而将 server 的半连接队列 (即 server 收到连接请求 SYN 之后将 client 加入半连接队列中) 占满，从而使得 server 拒绝其他正常的连接请求，即拒绝服务攻击.

### 21. TCP 以什么方式提供数据可靠性

* 将数据切分为最合适发送的数据块
* 计时，收到后确认与超时重传。当 TCP 发出一个段后，它会启动一个定时器，等待目的端确认收到这个报文段。如果不能及时收到一个确认
* 将重发这个报文
* 首部与数据校验和确保数据在传输过程中的任何变化
* 重新排序机制确保收到的数据以正确的顺序交给应用层
* 丢弃重复数据
* 流量控制，防止较快主机致使较慢主机的缓冲区溢出

### 22. DNS 域名解析过程

1. 应用首先检查缓存中有没有这个域名对应的解析过的 IP 地址。如果缓存中有，则返回缓存中的 IP 地址。否则进行下一步
2. 查找本机的 host 文件，Linux 为 /etc/hosts,Windows 为 C:\Windows\System32\drivers\etc\hosts. 若查到，则返回。否则进行下一步
3. 发送请求到 DNS 服务器 Linux 为 /etc/resolv.conf.DNS 服务器在收到客户机的请求后，首先检查 DNS 服务器的缓存，若查到请求的地址或名字，即向客户机发出应答信息。否则在数据库中查找，若查到请求的地址或名字，即向客户机发出应答信息
4. 若没有找到，则将请求发给根域 DNS 服务器，并依序从根域查找顶级域，由顶级域查找二级域… 直至找到要解析的地址或名字，即向客户机所在网络的 DNS 服务器发出应答信息，DNS 服务器收到应答后现在缓存中存储，然后，将解析结果发给客户机

## Monitoring

### 1. prometheus组件

![avatar](https://raw.githubusercontent.com/bernylinville/DevOps-Interview/main/image/prometheus.png)

Prometheus由多个组件组成，但是其中许多组件是可选的；

* Prometheus Server 用于抓取指标、存储时间序列数据
* exporter 暴露指标让任务抓取
* Pushgateway push的方式将指标数据推送到网关
* alertmanager 处理报警的报警组件
* adhoc 用于数据查询

### 1. Prometheus operator组件

![avatar](https://raw.githubusercontent.com/bernylinville/DevOps-Interview/main/image/prometheus_operator.png)

Operator是核心部分，作为一个控制器而存在，Operator会创建Prometheus、ServiceMonitor、AlertManager及Prometheus Rule这四个CRD资源对象，然后一直监控并维持这4个CRD资源对象的状态

* Prometheus 资源对象是作为Prometheus Service存在的
* ServiceMonitor 资源对象是专门提供metrics数据接口的exporter的抽象，Prometheus就是通过ServiceMonitor提供的metrics数据接口去 pull 数据的
* AlerManager 资源对象是对应alertmanager组件
* PrometheusRule 资源对象是被Prometheus实例使用的告警规则文件

> CRD简介，全称CustomResourceDefinition,简单的来说CRD是对Kubernetes API的扩展，Kubernetes中的每个资源都是一个API对象集合

这样，在集群中监控数据，就变成Kubernetes直接去监控资源对象，Service和ServiceMonitor都是Kubernetes的资源对象，一个ServiceMonitor可以通过labelSelector匹配一类Service，Prometheus也可以通过labelSelector匹配多个ServiceMonitor，并且Prometheus和AlertManager都是自动感知监控告警配置的变化，不需要人为进行reload操作

### 2. 监控指标

* 容器环境监控，主要指服务所处运行环境的一些监控数据。
* 应用服务监控，主要指服务本身的基础数据指标，提现服务自身的运行状况。
  * 系统信息，包括运行时间、CPU使用率、系统负载等；
  * 内存使用情况，包括堆内存使用情况和非堆内存使用情况；
  * 线程使用情况，包括线程数、守护线程数、线程峰值等；
  * 类加载信息；
  * GC信息，包括GC次数、GC消耗时间等；
  * HTTP请求情况，描述HTTP请求的性能指标，是非常重要的监控指标，在统计HTTP服务的QPS、响应时间和成功率时必不可少。
* 第三方接口监控，主要指调用其他外部服务接口的情况。

### 3. 你如何监视服务器质量和网络质量？用个哪些工具及优缺点？

cacti绘图漂亮，查看以往数据非常方便，但是报警功能弱我觉得最大的缺点还是rrdtool上，cacti + rrdtool暂时没有把旧数据写到数据库的插件，导致做数据处理会很麻烦取数据方便有snmp，能写各种脚本想要什么数据就有什么数据.

nagios报警强大，定义好报警脚本想短信猫就短信猫，想post短信平台就post短信平台但是不适合查询以往数据，最大缺点不能像cacti那样一次行取多个数据进行记录、绘图，只能通过返回值确认是否故障。一般都和cacti同时使用

## Nginx/LVS

### Nginx 支持的负载均衡算法有哪些？分别有什么应用场景

* round robin 轮询
依次将请求分配到各个后端服务器中，是 Nginx 默认的负载均衡方式。适用于后端服务器性能一致的情况.

```conf
upstream bakend {
    server 172.16.1.2:8080;
    server 172.16.1.3:8080;
}
```

* weight 加权轮询
加权轮询。根据权重比率分发请求到各个后端服务器中。使用 weight 指定权重。适用于后端服务器性能不一致的情况.

```conf
upstream bakend {
    server 172.16.1.2:8080 weight=10;
    server 172.16.1.3:8080 weight=20;
}
```

* ip_hash 客户端 IP 哈希
根据客户端 IP 地址的 hash 值将请求发送到后端服务器。可保证来自同一个客户端的请求被转发到固定的后端服务器中，可解决 session 问题.

```conf
upstream bakend {
    ip_hash;
    server 172.16.1.2:8080;
    server 172.16.1.3:8080;
}
```

* least_conn 最小连接数
根据后端服务器的连接数进行分发，同时考虑后端服务器的权重.

```conf
upstream bakend {
    least_conn;
    server 172.16.1.2:8080;
    server 172.16.1.3:8080;
}
```

* least_time 最小平均响应时间
根据后端服务器的平均响应时间及活动连接数进行分发，同时考虑后端服务器的权重.

```conf
upstream bakend {
    least_time header | last_byte [inflight];
    server 172.16.1.2:8080;
    server 172.16.1.3:8080;
}
// 如果指定 `header`,则使用 `$upstream_header_time`(请求头的响应时间)作为响应时间的判断依据
// 如果指定 `last_byte`,则使用 `$upstream_response_time`(请求响应时间)作为响应时间的判断依据.如果同时指定了 `inflight` 参数,则不完整的请求也会计算在内.
```

* hash key 自定义 hash 键轮询
根据自定义 key 的值进行哈希从而选择后端服务器.key 可以包含文本，变量及其组合.

```conf
upstream bakend {
    hash <key> [consistent];
    server 172.16.1.2:8080;
    server 172.16.1.3:8080;
}
// 如果指定了 `consistent` 参数,则将使用 ketama 一致性哈希算法.
// 该算法可以确保向组中添加服务器或从组中删除时,只有很少的键被重新映射到与原来不同的服务器.有助于缓存服务器获得更高的缓存命中率
```

* random 完全随机
nginx version >= 1.15.1

将请求随机转发到后端服务器，同时考虑后端服务器权重.

```conf
upstream bakend {
    random [two [method]];
    server 172.16.1.2:8080;
    server 172.16.1.3:8080;
}
// 可选的 `two` 参数随机选择两个后端服务器,然后使用指定的 `method` 选择一个后端服务器.默认方法是 `least_conn`,将请求传递给活动连接数最少的服务器
```

### Nginx location 匹配的顺序是什么

Nginx location 按照如下优先级顺序进行匹配

* 精确匹配: location = /path
* 以某个常规字符串开头: location ^~ /prefix_path, 且 prefix_path 路径长者优先匹配
* 以正则表达式进行匹配: location ~ regex_path 或 location ~* regex_path(~* 表示忽略大小写). 不管正则表达式如何进行锚定匹配，首先按照定义的顺序进行匹配。见如下示例

```conf
# 如下情况 uri=/image/.jpg 返回 /image
location ~ /image {
    return 200 '/image';
}
location ~ \.jpg {
    return 200 '.jpg';
}

# 如下情况 uri=/image/.jpg 返回 .jpg
location ~ \.jpg {
    return 200 '.jpg';
}
location ~ /image {
    return 200 '/image';
}
```

* 通用匹配: location /path, 且 path 路径长者优先匹配

### Nginx 调优

#### 合理设置 Nginx worker 进程数，并绑定到不同 CPU

一般情况下，应将 worker 进程数设置为 CPU 核数，或设置为 auto 根据系统 CPU 自动配置.

可以通过 worker_cpu_affinity 将 Nginx worker 进程绑定到不同的 CPU 上，避免多个进程竞争同一个 CPU 资源，充分利用 CPU 多核资源.

```conf
worker_processes  4;
worker_cpu_affinity 0001 0010 0100 1000;
```

#### 使用 epoll 事件驱动模型，合理设置单个进程的最大连接数及最大打开文件数量

> 进程最大连接数受系统最大打开文件数限制，因此需要使用 ulimit 设置打开的文件数。或在 /etc/security/limits.conf, /etc/security/limits.d/nginx.conf 进行配置。配置如下

```conf
ulimit -n $max_open_files # 设置系统最大打开文件数
cat /etc/security/limits.d/nginx.conf
*       soft    nproc   131072
*       hard    nproc   131072
*       soft    nofile  131072
*       hard    nofile  131072
```

```conf
events {
    use epoll;
    worker_connections 15000;
    worker_rlimit_nofile 65535;
}
```

#### 优化服务器域名的散列表大小

如果在 server_name 中配置了长域名，可能会出现如下错误.

```conf
nginx: [emerg] could not build the server_names_hash, you should increase server_names_hash_bucket_size: 64
nginx: configuration file /usr/local/nginx/conf/nginx.conf test failed
```

此时需要对 http 配置段 server_names_hash_bucket_size 进行调整，如下:

```conf
http {
    # ...
    server_names_hash_bucket_size  512;
}
```

#### 开启高效文件传输模式，开启或 gzip 压缩

* http 配置段 sendfile 参数用于开启文件高效传输模式
* http 配置段 gzip 参数可用于开启压缩功能

```conf
http {
    gzip  on;
    sendfile on;
}
```

#### 优化 Nginx 连接超时时间

* http 配置段 keepalive_timeout 参数用于设置客户端连接保持会话的超时时间，超过这个时间服务器会关闭该连接
* http 配置段 client_header_timeout 参数用于设置读取客户端请求头数据的超时时间。如果读取请求头超时，服务器将返回 “Request time out (408)” 错误.
* http 配置段 client_body_timeout 参数用于设置读取客户端请求主体数据的超时时间。如果读取请求体超时，服务器将返回 “Request time out (408)” 错误.
* http 配置段 send_timeout 参数用于指定响应客户端的超时时间，如果超时，Nginx 将会关闭连接.

```conf
http {
    # 在大并发时,需要合理调小如下参数
    keepalive_timeout  65;
    client_header_timeout 15;
    client_body_timeout 15;
    send_timeout 25;
}
```

#### 限制上传文件大小

* http 配置段 send_timeout 参数用于设置客户端最大请求体大小。如果超过出，客户端会收到 413 错误，即请求体过大

```conf
http {
    client_max_body_size 8m;    # 设置客户端最大请求体大小为8M
}
```

#### 合理配置 Nginx expires 缓存

* expires 参数用于设置用户访问内容的缓存时间。用户会在本地浏览器中缓存这些内容，直到超过缓存时间。多用于配置静态内容

```conf
location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|js|css)$ {
    expires     3650d;
}
```

### 1. nginx 优化

```shell
# 隐藏版本号
server_tokens off;
# 修改用户和组
# expires缓存(一般是图片)
# 日志切割
# 设置超时时间
keepalive_timeout   #设置超时时间
client_header_timeout  #指定请求头的超时时间
client_body_timeout    #设置请求体超时时间
* Gzip压缩
Nginx_http_gzip_module压缩模块提供了对文件压缩的功能，以节约网站的带宽，提高用户的访问体验
默认Nginx已经安装该模块
    gzip on    #开启gzip压缩输出
    gzip_buffers 4 64;   #表示申请了4个单位为64kb的内存作为压缩结果流缓存
    gzip_http_version 1.1  #用于设置http协议版本
    gzip_min_length     #设置允许页面的最小字节
    gzip_vary on;       #让前端的缓存服务器缓存就经过的gzip压缩页面
* 防盗链
Nginx防盗链的原理是加入location项，用正则表达式过滤图片类型文件，对于信任的网站可以正常使用，对于不信任的网址则返回相应的错误页面
     [root@localhost ~]# vim /usr/local/nginx/conf/nginx.conf
          location ~*.(jpg|gif|swf)$ {
            valid_referers none blocked *.benet.com benet.com;
            if ( $invalid_referer ) {
               rewrite ^/ http://www.benet.com/error.png;
            }
         }
    ~*.(jpg|gif|swf)$: 匹配不区分大小写，以.jpg 或.gif或 .swf结尾的文件。
    valid_referers：设置信任的网站，可以正常使用图片。
    none：浏览器中refer为空的情况，就是直接在浏览器访问图片。
    blocked：浏览器中refer不为空的情况，但是值被代理或防火墙删除了，这些值不以http://或 https://开头。
    后面的网址或域名：refer包含相关字符串的网址。
    if语句：如果链接的来源域名不在valid_referers所列出的列表中， $invalid_referer 为1，则执行后面的操作，即进行重写或返回403页面

```

### 2. nginx upsteam 轮询方式

* RR 轮询: 默认的反向代理模式，用以平衡各个服务器的负载，若某个服务器宕机，会自动从轮询中剃掉。同时，可以手动指定某台服务器脱离轮询，用于离线检查
* weight 权重: 针对服务器性能不通，用来控制服务器被访问的比例，以实现老客户访问时的快速调度
* ip hash：主要记录了客户端IP访问的目标主机，以实现老用户访问的快速调度

### 3. nginx 跨域

首先我们要知道什么是跨域，跨域指的是浏览器不能执行其他网站的脚本，它是由浏览器的同源策略造成的，是浏览器对JavaScript施加的安全限制

举例:A页面想获取B页面资源，如果a,b页面的协议、端口、域名、子域名不同，所进行的所有访问请求都是跨域的，而浏览器一般为了安全都限制跨域请求，也就是不允许跨域请求资源。注意：跨域其实是浏览器的限制！

```shell
#如何解决跨域请求？
Access-Control-Allow-Origin: http://api.bob.com
Access-Control-Allow-Credentials: true
Access-Control-Expose-Headers: FooBar
Content-Type: text/html; charset=utf-8

Access-Control-Allow-Origin
# 这里大概意思是允许跨域的域名，可以写一个域名，或者写* 代表所有
Access-Control-Allow-Credentials
# 表示是否允许发送Cookie，默认情况下，Cookie不包括在CORS请求之中。设置为true，表示服务器明确许可，Cookie可以包含在请求中，一起发给服务器，如果服务器不需要浏览器发送的Cookie，删除该字段即可
Access-Control-Expose-Headers
# 表示请求头的字段 动态获取

```

### 4. nginx 反向代理和正向代理区别

* 正向代理

正向代理 是一个位于客户端和原始服务器(origin server)之间的服务器，为了从原始服务器取得内容，客户端向代理发送一个请求并指定目标(原始服务器)，然后代理向原始服务器转交请求并将获得的内容返回给客户端。客户端必须要进行一些特别的设置才能使用正向代理。就像要访问google用vpn代理翻墙去访问

* 反向代理

对于客户端而言它就像是原始服务器，并且客户端不需要进行任何特别的设置。客户端向反向代理 的命名空间(name-space)中的内容发送普通请求，接着反向代理将判断向何处(原始服务器)转交请求，并将获得的内容返回给客户端，就像这些内容 原本就是它自己的一样。（用户不知道要访问真正的服务器

#### 以租房为例解释正向代理和反向代理

正向代理 客户端 <一> 代理 一>服务端
A(客户端)想租C(服务端)的房子,但是A(客户端)并不认识C(服务端)租不到。
B(代理)认识C(服务端)能租这个房子所以你找了B(代理)帮忙租到了这个房子。
这个过程中C(服务端)不认识A(客户端)只认识B(代理)
C(服务端)并不知道A(客户端)租了房子，只知道房子租给了B(代理)。

反向代理 客户端 一>代理 <一> 服务端
A(客户端)想租一个房子,B(代理)就把这个房子租给了他。
这时候实际上C(服务端)才是房东。
B(代理)是中介把这个房子租给了A(客户端)。
这个过程中A(客户端)并不知道这个房子到底谁才是房东
他都有可能认为这个房子就是B(代理)的

> 由上的例子我们可以知道正向代理和反向代理的区别在于代理的对象不一样,正向代理的代理对象是客户端,反向代理的代理对象是服务端。

### 5. Nginx 和LVS 区别

1. 高并发连接：官方测试能够支撑5万并发连接，在实际生产环境中跑到2——3万并发连接数。
2. 内存消耗少：在3万并发连接数下，开启的10个nginx进程才消耗150M内存（150*10=150M）。
3. 配置文件非常简单：风格跟程序一样通俗易懂。
4. 成本低廉：nginx为开源软件，可以免费使用。而购买F5 big-ip、netscaler等硬件负载均衡交换机则需要十多万至几十万人民币。
5. 支持rewrite重写规则：能够根据域名、url的不同，将http请求分到不同的后端服务器群组。
6. 内置的健康检查功能：如果nginx proxy后端的某台web服务器宕机了，不会影响前端访问。
7. 节省带宽：支持gzip压缩，可以添加浏览器本地缓存的header头。

### 6. Nginx、LVS、haproxy区别

#### Nginx 优点

* 工作在网络的7层之上，可以针对http应用做一些分流的策略，比如针对域名、目录结构，它的正则规则比HAProxy更为强大和灵活，这也是它目前广泛流行的主要原因之一，Nginx单凭这点可利用的场合就远多于LVS了。
* Nginx对网络稳定性的依赖非常小，理论上能ping通就就能进行负载功能，这个也是它的优势之一；相反LVS对网络稳定性依赖比较大。
* Nginx安装和配置比较简单，测试起来比较方便，它基本能把错误用日志打印出来。LVS的配置、测试就要花比较长的时间了，LVS对网络依赖比较大。
* 可以承担高负载压力且稳定，在硬件不差的情况下一般能支撑几万次的并发量，负载度比LVS相对小些。
* Nginx可以通过端口检测到服务器内部的故障，比如根据服务器处理网页返回的状态码、超时等等，并且会把返回错误的请求重新提交到另一个节点，不过其中缺点就是不支持url来检测。比如用户正在上传一个文件，而处理该上传的节点刚好在上传过程中出现故障，Nginx会把上传切到另一台服务器重新处理，而LVS就直接断掉了，如果是上传一个很大的文件或者很重要的文件的话，用户可能会因此而不满。
* Nginx不仅仅是一款优秀的负载均衡器/反向代理软件，它同时也是功能强大的Web应用服务器。LNMP也是近几年非常流行的web架构，在高流量的环境中稳定性也很好。
* Nginx现在作为Web反向加速缓存越来越成熟了，速度比传统的Squid服务器更快，可以考虑用其作为反向代理加速器。
* Nginx可作为中层反向代理使用，这一层面Nginx基本上无对手，唯一可以对比Nginx的就只有 lighttpd了，不过 lighttpd目前还没有做到Nginx完全的功能，配置也不那么清晰易读，社区资料也远远没Nginx活跃。
* Nginx也可作为静态网页和图片服务器，这方面的性能也无对手。还有Nginx社区非常活跃，第三方模块也很多。

#### Nginx 缺点

Nginx仅能支持http、https和Email协议，这样就在适用范围上面小些，这个是它的缺点。
对后端服务器的健康检查，只支持通过端口来检测，不支持通过url来检测。不支持Session的直接保持，但能通过ip_hash来解决。

#### LVS 优点

* 抗负载能力强、是工作在网络4层之上仅作分发之用，没有流量的产生，这个特点也决定了它在负载均衡软件里的性能最强的，对内存和cpu资源消耗比较低。
* 配置性比较低，这是一个缺点也是一个优点，因为没有可太多配置的东西，所以并不需要太多接触，大大减少了人为出错的几率。
* 工作稳定，因为其本身抗负载能力很强，自身有完整的双机热备方案，如LVS+Keepalived。
* 无流量，LVS只分发请求，而流量并不从它本身出去，这点保证了均衡器IO的性能不会受到大流量的影响。
* 应用范围比较广，因为LVS工作在4层，所以它几乎可以对所有应用做负载均衡，包括http、数据库、在线聊天室等等。

#### LVS 缺点

* 软件本身不支持正则表达式处理，不能做动静分离；而现在许多网站在这方面都有较强的需求，这个是Nginx/HAProxy+Keepalived的优势所在。
* 如果是网站应用比较庞大的话，LVS/DR+Keepalived实施起来就比较复杂了，特别后面有 Windows Server的机器的话，如果实施及配置还有维护过程就比较复杂了，相对而言，Nginx/HAProxy+Keepalived就简单多了。

#### HAProxy优点

* HAProxy也是支持虚拟主机的。
* HAProxy的优点能够补充Nginx的一些缺点，比如支持Session的保持，Cookie的引导；同时支持通过获取指定的url来检测后端服务器的状态。
* HAProxy跟LVS类似，本身就只是一款负载均衡软件；单纯从效率上来讲HAProxy会比Nginx有更出色的负载均衡速度，在并发处理上也是优于Nginx的。
* HAProxy支持TCP协议的负载均衡转发，可以对MySQL读进行负载均衡，对后端的MySQL节点进行检测和负载均衡，大家可以用LVS+Keepalived对MySQL主从做负载均衡。
* HAProxy负载均衡策略非常多，HAProxy的负载均衡算法现在具体有如下8种：
  * roundrobin，表示简单的轮询，这个不多说，这个是负载均衡基本都具备的；
  * static-rr，表示根据权重，建议关注；
  * leastconn，表示最少连接者先处理，建议关注；
  * source，表示根据请求源IP，这个跟Nginx的IP_hash机制类似，我们用其作为解决session问题的一种方法，建议关注；
  * ri，表示根据请求的URI；
  * rl_param，表示根据请求的URl参数’balance url_param’ requires an URL parameter name；
  * hdr(name)，表示根据HTTP请求头来锁定每一次HTTP请求；
  * rdp-cookie(name)，表示根据据cookie(name)来锁定并哈希每一次TCP请求。

### 7. LVS 几种模式

1. DR模型 -- （Director Routing-直接路由）
2. NAT模型 -- (NetWork Address Translation-网络地址转换)
3. fullNAT -- （full NAT，双向数据包都进行SNAT与DNAT）
4. ENAT --（enhence NAT 或者叫三角模式/DNAT）
5. IP TUN模型 -- (IP Tunneling - IP隧道)

### 8. LVS负载均衡调度算法

* 轮询调度

轮询调度（Round Robin 简称’RR’）算法就是按依次循环的方式将请求调度到不同的服务器上，该算法最大的特点就是实现简单。轮询算法假设所有的服务器处理请求的能力都一样的，调度器会将所有的请求平均分配给每个真实服务器。

* 加权轮询调度

加权轮询（Weight Round Robin 简称’WRR’）算法主要是对轮询算法的一种优化与补充，LVS会考虑每台服务器的性能，并给每台服务器添加一个权值，如果服务器A的权值为1，服务器B的权值为2，则调度器调度到服务器B的请求会是服务器A的两倍。权值越高的服务器，处理的请求越多。

* 最小连接调度

最小连接调度（Least Connections 简称’LC’）算法是把新的连接请求分配到当前连接数最小的服务器。最小连接调度是一种动态的调度算法，它通过服务器当前活跃的连接数来估计服务器的情况。调度器需要记录各个服务器已建立连接的数目，当一个请求被调度到某台服务器，其连接数加1；当连接中断或者超时，其连接数减1。

（集群系统的真实服务器具有相近的系统性能，采用最小连接调度算法可以比较好地均衡负载。)

* 加权最小连接调度

加权最少连接（Weight Least Connections 简称’WLC’）算法是最小连接调度的超集，各个服务器相应的权值表示其处理性能。服务器的缺省权值为1，系统管理员可以动态地设置服务器的权值。加权最小连接调度在调度新连接时尽可能使服务器的已建立连接数和其权值成比例。调度器可以自动问询真实服务器的负载情况，并动态地调整其权值。

* 基于局部的最少连接

基于局部的最少连接调度（Locality-Based Least Connections 简称’LBLC’）算法是针对请求报文的目标IP地址的 负载均衡调度，目前主要用于Cache集群系统，因为在Cache集群客户请求报文的目标IP地址是变化的。这里假设任何后端服务器都可以处理任一请求，算法的设计目标是在服务器的负载基本平衡情况下，将相同目标IP地址的请求调度到同一台服务器，来提高各台服务器的访问局部性和Cache命中率，从而提升整个集群系统的处理能力。LBLC调度算法先根据请求的目标IP地址找出该目标IP地址最近使用的服务器，若该服务器是可用的且没有超载，将请求发送到该服务器；若服务器不存在，或者该服务器超载且有服务器处于一半的工作负载，则使用’最少连接’的原则选出一个可用的服务器，将请求发送到服务器。

* 带复制的基于局部性的最少连接

带复制的基于局部性的最少连接（Locality-Based Least Connections with Replication 简称’LBLCR’）算法也是针对目标IP地址的负载均衡，目前主要用于Cache集群系统，它与LBLC算法不同之处是它要维护从一个目标IP地址到一组服务器的映射，而LBLC算法维护从一个目标IP地址到一台服务器的映射。按’最小连接’原则从该服务器组中选出一一台服务器，若服务器没有超载，将请求发送到该服务器；若服务器超载，则按’最小连接’原则从整个集群中选出一台服务器，将该服务器加入到这个服务器组中，将请求发送到该服务器。同时，当该服务器组有一段时间没有被修改，将最忙的服务器从服务器组中删除，以降低复制的程度。

* 目标地址散列调度

目标地址散列调度（Destination Hashing 简称’DH’）算法先根据请求的目标IP地址，作为散列键（Hash Key）从静态分配的散列表找出对应的服务器，若该服务器是可用的且并未超载，将请求发送到该服务器，否则返回空。

* 源地址散列调度U

源地址散列调度（Source Hashing 简称’SH’）算法先根据请求的源IP地址，作为散列键（Hash Key）从静态分配的散列表找出对应的服务器，若该服务器是可用的且并未超载，将请求发送到该服务器，否则返回空。它采用的散列函数与目标地址散列调度算法的相同，它的算法流程与目标地址散列调度算法的基本相似。

* 最短的期望的延迟

最短的期望的延迟调度（Shortest Expected Delay 简称’SED’）算法基于WLC算法。举个例子吧，ABC三台服务器的权重分别为1、2、3 。那么如果使用WLC算法的话一个新请求进入时它可能会分给ABC中的任意一个。使用SED算法后会进行一个运算

A：（1+1）/1=2 B：（1+2）/2=3/2 C：（1+3）/3=4/3 就把请求交给得出运算结果最小的服务器。

* 最少队列调度

最少队列调度（Never Queue 简称’NQ’）算法，无需队列。如果有realserver的连接数等于0就直接分配过去，不需要在进行SED运算。

### 9. Nginx 调度算法

* 轮询 rr
按时间顺序逐一分配到不同的后端服务器

* 加权轮询 wrr
在nginx server后面配置权重，权重越高分配的概率越大

* ip_hash
每个请求按访问IP的hash分配，这样来自同一IP固定访问一个后端服务器

* least_hash
最少连接数，哪个机器连接数少就发给哪台机器

* url_hash
按访问的url的hash结果分配请求，是每个url定向到同一后端服务器上

* hash关键值
自定义hash的key

> 注: 调度算法在设置upstream中配置，例如在此大括号里写如ip_hash表示使用ip_hash的方式
> 1.轮询只是简单实现请求的顺序转发，并没有考虑不同服务器的性能差异；
> 2.加权轮询设置了初始时服务器的全站，但是没有考虑运行过程中的服务器状态
> 3.IP Hash保证同一客户端请求转发到同一后台服务器实现了session保存，然而当某一后台服务器发生故障时，某些客户端将访问失败；
> 4.最少连接数只是考虑了后端服务器的连接数情况，并没有完全考虑服务器的整体性能

### 10. 当LVS 超出它能承受最大的连接数了，你应做如何处理了？

### 11. 一千万 并发，你有那些方案？【提示这些单用LVS 承受不起的，】

先用智能dsn负载到不同的lvs上如果有钱可以在lvs前端上f5智能dns——F5——LVS三层负载分流，最后最大的压力其实还是在数据库上

### 12. Apache工作机制和Nginx工作机制对比分析

## DataBase

### MySQL

#### 1. mysql复制不一致，卡主

1. 人为原因导致从库与主库数据不一致（从库写入）
2. 主从复制过程中，主库异常宕机
3. 设置了ignore/do/rewrite等replication等规则
4. binlog非row格式
5. 异步复制本身不保证，半同步存在提交读的问题，增强半同步起来比较完美。 但对于异常重启（Replication Crash Safe），从库写数据（GTID）的防范，还需要策略来保证。
6. 从库中断很久，binlog应用不连续，监控并及时修复主从
7. 从库启用了诸如存储过程，从库禁用存储过程等
8. 数据库大小版本/分支版本导致数据不一致？，主从版本统一
9. 备份的时候没有指定参数 例如mysqldump —master-data=2 等
10. 主从sql_mode 不一致
11. 一主二从环境，二从的server id一致。
12. MySQL自增列 主从不一致
13. 主从信息保存在文件里面，文件本身的刷新是非事务的，导致从库重启后开始执行点大于实际执行点

预防措施：

1.master:innodb_flush_log_at_trx_commit=1&sync_binlog=1
2.slave:master_info_repository="TABLE"&relay_log_info_repository="TABLE"&relay_log_recovery=1
3.设置从库库为只读模式
4.可以使用5.7增强半同步避免数据丢失等
5.binlog row格式
6.必须引定期的数据校验机制

#### 2. Mysql 主从原理

1. 在Slave 服务器上执行sart slave命令开启主从复制开关，开始进行主从复制。
2. 此时，Slave服务器的IO线程会通过在master上已经授权的复制用户权限请求连接master服务器，并请求从执行binlog日志文件的指定位置（日志文件名和位置就是在配置主从复制服务时执行change master命令指定的）之后开始发送binlog日志内容
3. Master服务器接收到来自Slave服务器的IO线程的请求后，其上负责复制的IO线程会根据Slave服务器的IO线程请求的信息分批读取指定binlog日志文件指定位置之后的binlog日志信息，然后返回给Slave端的IO线程。返回的信息中除了binlog日志内容外，还有在Master服务器端记录的IO线程。返回的信息中除了binlog中的下一个指定更新位置。
4. 当Slave服务器的IO线程获取到Master服务器上IO线程发送的日志内容、日志文件及位置点后，会将binlog日志内容依次写到Slave端自身的Relay Log（即中继日志）文件（Mysql-relay-bin.xxx）的最末端，并将新的binlog文件名和位置记录到master-info文件中，以便下一次读取master端新binlog日志时能告诉Master服务器从新binlog日志的指定文件及位置开始读取新的binlog日志内容
5. Slave服务器端的SQL线程会实时检测本地Relay Log 中IO线程新增的日志内容，然后及时把Relay LOG 文件中的内容解析成sql语句，并在自身Slave服务器上按解析SQL语句的位置顺序执行应用这样sql语句，并在relay-log.info中记录当前应用中继日志的文件名和位置点

#### 3. MySQL 配置文件

配置文件读取顺序如下: /etc/my.cnf -> /etc/mysql/my.cnf -> /usr/local/mysql/etc/my.cnf -> ~/.my.cnf. 后面的配置会覆盖前面的配置。如果忘记，可通过 mysql --help | grep my.cnf 进行查看

#### 4. 日志文件

* 错误日志：记录 MySQL 启动，运行，关闭过程中发生的错误。可通过 ```show variables like '%log_error%'```; 查看错误日志文件位置.
* 慢查询日志：记录查询时间超过 ```long_query_time``` 参数值 (默认为 10) 的所有 SQL 语句
  * ```slow_query_log```: 设置是否开启慢查询日志
  * ```slow_query_log_file```: 慢查询日志文件
* 二进制日志 (binlog): 记录对 MySQL 数据库执行更改的所有操作，不包括 ```SELECT``` 和 ```SHOW``` 之类的操作。可用于复制备份恢复数据
  * ```log_bin```: 是否记录 binlog
  * ```log_bin_index```: binlog 索引
  * ```binlog_format```: 记录二进制日志的格式. ```STATEMENT``` 记录逻辑 SQL 语句.```ROW``` 格式记录表行更改情况 (在执行 UPDATE 时，数据与原来一致，则不不会执行).```MIXED``` 默认使用 ```STATEMENT``` 格式，一些情况使用 ```ROW``` 格式.
  * ```max_binlog_size```: binlog 文件最大大小，若超过该值，则产生新的 binlog 文件，并使索引加 1.
  * ```binlog_cache_size```: 未提交的 binlog 会被记录到缓存中，该选项配置会话缓存大小，默认为 32 KB.
  * ```sync_binlog```: 表示缓冲数据写入磁盘的方式。默认为 0.1 表示同步写磁盘来写二进制日志.

#### 5. 为什么 MySQL 数据库索引选择使用 B+ 树

* B+ 树中父节点元素都出现在子节点，所有叶子节点包含了全量元素信息，每个叶子节点都带有指向下一个节点的指针，形成了一个有序链表。相比于 B 树，更易于范围查找
* B 树中的每个节点带有卫星数据 (所谓卫星数据，指的是索引元素指向的数据记录，比如数据库中的一行). 而 B+ 树中只有叶子节点带有卫星数据 (中间节点仅仅是索引), 因此相同的磁盘页可以容纳更多的节点元素。这就意味着，相同数据量的情况下，B+ 树比 B 树更加” 矮胖”, 查询 IO 次数更少.

#### 6. 聚簇索引与非聚簇索引

* 聚簇索引
聚簇索引按照每张表的主键构造一棵 B+ 树，叶子节点存放整张表的行记录数据。每个数据页都通过一个双向链表来进行链接.

聚簇索引对于主键的排序查找和范围查找非常快。叶子节点就是用户索要查询的数据.

* 非聚簇索引
非聚簇索引中叶子节点不包含行记录的全部数据，而是带有指向数据的指针.

当通过非聚簇索引查找数据时，InnoDB 存储引擎会遍历非聚簇索引并通过叶级别的指针获得指向主键索引的主键，然后再通过主键找到一个完整的行记录.

#### 7. 如何定位锁问题

查看命令 ```show engine innodb status``` 的输出，并通过查询 ```information_schema``` 库中三个有关锁的表进行查看锁的详情

* ```innodb_trx```: 当前运行的所有事务
* ```innodb_locks```: 当前出现的锁
* ```innodb_lock_waits```: 锁等待的对应关系

#### 8. MySQL 事务有哪些特性

* 原子性 (atomicity): 事务是一个不可分割的操作，要么全部正确执行，要么全部不执行
* 一致性 (consistency): 事务把数据从一种一致性状态转化为另一种一致性状态，事务开始前后，数据库完整性没有被破坏
* 隔离性 (isolation): 要求每个读写事务之间是分开的，在提交事务之前对其它事务是不可见的
* 持久性 (durability): 事务一旦提交，结果是永久性的

#### 9. 事务的隔离级别

* 读未提交：能够读取到未提交的数据，脏读
* 读已提交：只能读取到已经提交的数据，解决脏读问题，产生不可重读问题。两次同样的查询，可能得到不一样的结果.
* 可重读 (默认): 解决不可重读问题，但仍然存在幻读，某个事务在读取范围内记录时，另一个事务又在该范围内插入新纪录，之前事务再次读取该范围的记录时，会产生幻行.
* 串行化：强制事务串行执行，避免幻读问题

#### 10. MySQL 复制过程及原理

过程:

* 主库把数据更改记录在二进制日志中
* 备库将主库上的日志复制到自己的中继日志中
* 备库读取中继日志中的事件，并将其重放到备库上

原理:

* 主库记录二进制日志，在每次提交事务完成数据更新之前，主库将数据更新的事件记录到二进制日志中.
* 备库将主库的二进制日志复制到本地中继日志中。备库会启动一个 I/O 线程，并使用该线程与主库建立连接，读取二进制日志中的事件，复制到备库的本地中继日志中。如果该线程追上了主库，则进入睡眠状态，直到有信号通知.
* 备库的 SQL 线程从中继日志中读取事件并在备库中执行，直到 SQL 线程追上 I/O 线程，从而实现备库的更新

#### 11. 如何判断 MySQL 是否同步，该如何使其同步

使用 ```show slave status\G``` 查看同步信息

```shell
Slave_IO_State:
Slave_IO_Running:
Slave_SQL_Running:
Slave_SQL_Running_State:
Last_IO_Errno:
Last_IO_Error:
Last_SQL_Errno:
Last_SQL_Error:
Read_Master_Log_Pos: 256364229
Exec_Master_Log_Pos: 256364229
Seconds_Behind_Master: 0
SQL_Delay: 0
```

```shell
# 主库配置
log_bin = mysql-bin # 开启二进制日志并设置二进制文件前缀名
server_id = 10 # 唯一服务器ID,可自行设置
sync_binlog = 1 # MySQL 每次在提交事务之前会将二进制日志同步到磁盘上,保证服务器在崩溃时不丢失事
innodb_flush_logs_at_trx_commit  #  每次提交事务时会记录日志,开启会对性能产生影响,但是提升准确性

# 从库配置
relay_log = /path/to/relay_log/relay-bin
log_slave_updates = 1 # 允许从节点在二进制日志中记录更新事务
log_bin = mysql-bin
skip_slave_start # 阻止从库在崩溃后自动启动复制
read_only = 1

# 从库执行
CHANGE MASTER TO MASTER_HOST='server1',
    MASTER_USER='repl',
    MASTER_PASSWORD='password',
    MASTER_LOG_FILE='mysql-bin.000001',
    MASTER_LOG_POS=0;
```

#### 12. MySQL 如何减少主从复制延迟

* 慢 SQL 语句过多，在开发或架构上做优化，减少慢 SQL 语句
* 主从复制单线程，主库写入过快。主库写，安全性较高，建议开启 ```sync_binlog=1```,```innodb_flush_log_at_trx_commit=1``` 之类的设置，而从库可以不开启
* master 负载过大。架构的前端要加 buffer 及缓存层，如 redis
* slave 负载过大，使用多台 slave 来分摊读请求。再从这些 slave 中取一台专用的服务器
* 网络延迟
* 从库硬件性能较差

#### 13. MySQL 重置密码

```shell
mysqld --skip-grant-tables --skip-networking
mysql> use mysql;
mysql> update user set password=password("new_password") where user="root";
mysql> flush privileges;
mysqld --正常启动测试
```

#### 14. SQL 优化

* 通过 show status like 'Com_%' 查看各种语句执行的次数，其中 Slow_queries 表示慢查询次数
* 通过 show processlist 命令查看当前 MySQL 正在进行的线程状态，是否锁表等
* 通过 explain 分析低效 SQL 的执行计划
  * select_type 表示选择的类型
  * type 表示 MySQL 的访问方式，all (全表扫描),index (索引),range (索引范围),
  * possible_key 表示查询时可能用到的索引
  * key 表示实际用到的索引
  * key_len 表示用到索引字段的长度
  * rows: 扫描行的数量

#### 15. 常用 SQL 优化

##### 1. 大量插入数据

* 使用多个值的 insert 语句，避免连接关闭等消耗
* 使用 insert delayed 获取更高速度
* 使用 load data infile 代替 insert
* 将索引文件和数据文件放在不通磁盘上，增大数据写入速率

##### 2. 创建合适的索引，并按照索引顺序进行查询

* 考虑在 where 及 order by 列上创建索引
* 避免在 where 字句中进行空值判断，或使用！=,<>,in,or,not in,like ‘% xxx’, 函数 / 计算，否则将导致放弃使用索引而进行全表扫描

#### 16. drop,delete,truncate 的区别

* drop 删除表有关的一切 (数据，结构，约束，键), 为 DDL 操作
* delete 删除表中所有数据，但每次从表中删除一行，较慢，可增加 where 语句
* truncate 一次性清空表数据，保留表结构，约束，键

### Redis

#### 1. Redis 基本数据类型

* 字符串
* 列表
* 散列
* 集合
* 有序集合

#### 2. redis 主从原理

当客户端向从服务器发送SLAVEOF命令，要求从服务器复制主服务器时，从服务器首先要执行同步操作，即将从服务器的数据库状态更新至主服务器当前所处的数据库状态。

同步过程步骤如下：

从服务器向主服务器发送SYNC命令。
收到SYNC命令的主服务器执行BGSAVE命令，在后台生成一个RDB文件，并使用一个缓冲区记录从现在开始执行的所有写命令。
当主服务器的BGSAVE命令执行完毕，主服务器将生成的RDB文件发送给从服务器，从服务器接收并加载这个RDB文件，将自己的数据库状态更新至主服务器执行BGSAVE命令是的数据库状态。
主服务器将记录在缓冲区里面的所有写命令发送给从服务器，从服务器执行这些写命令，将自己的数据库状态更新至主服务器当前所处的状态

#### 3. Redis持久化存储机制

Redis 数据保存在内存中，当进程重新启动时，释放内存并重新分配，数据会丢失。持久化就是将 Redis 内存中的数据保存在磁盘上，在进程重新启动时，可以从磁盘加载数据，保证数据持久性

#### 4. Redis 为什么这么快

* Redis 中的数据保存在内存中，绝大多数请求都是纯粹的内存操作，非常快速
* 使用多路 I/O 复用模型，非阻塞 IO
* 采用单线程，避免不必要的上下文切换和竞争，也不存在多进程或多线程中各种锁的问题

#### 5. 哨兵机制实现原理

##### 三个定时任务

1. 每隔 10 秒，Sentinel 节点向主 / 从节点发送 info 命令获取最新拓补结构
2. 每隔 2 秒，Sentinel 节点向 Redis 数据节点 __sentinel__:hello 频道发送该 Sentinel 节点对于主节点的判断及当前 Sentinel 节点信息。每个 Sentinel 也会订阅该频道，了解其它 Sentinel 以及对主节点的判断 (主要用于发现新的 Sentinel 节点，并交换主节点状态，作为客观下线及领导者选举的依据)
3. 每隔 1 秒，Sentinel 向主从节点，Sentinel 节点发送 ping 命令做心跳检测 (当节点不可达时做主观下线及领导者选举的依据)

##### 故障恢复过程

* 主观下线

由定时任务 3 完成，当节点不可达时，认为该节点下线

* 客观下线

当 Sentinel 主观下线节点是主节点时，该节点通过 sentinel is-master-down-by-addr 向其他节点询问对该主节点的判断，当主观下线超过 ```<quorum>``` 时，作出客观下线

* 领导者选举

  * 主观下线后，该节点通过 sentinel is-master-down-by-addr 命令，要求将自己设置为领导者.
  * 收到该命令的 Sentinel 节点，如果没有同意过其他 Sentinel 节点的命令，则同意，否则拒绝
  * 当 Sentinel 节点的票数大于等于 max(quorum, num(sentinels)/2 + 1) 它将成为领导者

* 故障转移 领导者选举选出的节点负责故障转移

  * 在从节点中选出一个节点执行 slaveof no one 让其成为新主节点
  * 向剩余节点发送命令，让他们成为新主节点的从节点
  * 将原来主节点更新为从节点，并保持关注，恢复后进程复制

> 设置 slave 节点资源池，实时掌握从节点状态，可保证从节点高可用

#### 6. 集群原理

Redis 集群采用虚拟槽分区的，所有的键根据哈希函数映射到 0-16383 槽内.

缺点:

* 批量操作受限制
* 复制结构只支持一层

##### 集群搭建

* 准备节点

```shell
cluster-enabled yes # 开启集群模式
cluster-config-file "" # 指定集群配置文件
```

* 节点握手，```cluster meet <redisIP> <redisPort>```
* 分配槽，配置主从.```cluster addlots <lot>```, ```cluster replicate <redisRunId>```

##### 集群节点通信

Gossip, 流言协议

ping,pong,meet,fail

#### 7. Redis 安全策略

* 简单的密码验证，通过 requirepass 配置
* 对危险命令重命名，通过 rename-command 设置，危险命令包括
  * keys: 如果键值较多，存在阻塞 Redis 的可能性
  * flushall/flushdb: 数据全部被清除
  * save: 如果键值较多，存在阻塞 Redis 的可能性
  * debug: 例如 debug reload 会重启 Redis
  * config: config 包含 redis 配置相关命令，应该交给管理员使用
  * shutdown: 停止 Redis
* bind: 选择指定网卡做绑定，而不要绑定 0.0.0.0
* 修改默认端口
* 尽量不要使用 root 用户运行
* 定期备份数据

## 中间件

### 1. tomcat 优化

1. 内存优化 修改内存等 JVM相关配置
2. 并发优化 Connector是连接器，负责接收客户的请求，以及向客户端回送响应的消息。所以 Connector的优化是重要部分。默认情况下 Tomcat只支持200线程访问，超过这个数量的连接将被等待甚至超时放弃，所以我们需要提高这方面的处理能力
3. 线程池 Executor代表了一个线程池，可以在Tomcat组件之间共享。使用线程池的好处在于减少了创建销毁线程的相关消耗，而且可以提高线程的使用效率。
使用compression来提高网页加载速度

compression 打开压缩功能
compressionMinSize 启用压缩的输出内容大小，这里面默认为2KB
compressableMimeType 压缩类型
connectionTimeout 定义建立客户连接超时的时间. 如果为 -1, 表示不限制建立客户连接的时间

### 2. kafka和rabbitmq区别

1.语言不通
RabbitMQ是由在高并发的erlanng语言开发，用在实时的对可靠性要求比较高的信息传递上

kafka是采用Scala语言开发，它主要用于处理灵活的流式数据，大数据量的数据处理上

2.结构不通
RabbitMQ采用AMQP (Advanced Message Queuing Protocol，高级消息队列协议)是一个进程间传递异步信息的网络协议

![avatar](https://raw.githubusercontent.com/bernylinville/DevOps-Interview/main/image/kafka1.png)

kafka采用mq结构: broker有part分区的概念

![avatar](https://raw.githubusercontent.com/bernylinville/DevOps-Interview/main/image/kafka_broker.png)

3.在集群负载均衡方面
rabbitMQ的负载均衡需要单独的loadbalancer进行支持

kafka采用zookeeper对集群的broker、consumer进行管理

4.使用场景
rabbitMQ支持对消息的可靠性的传递，支持事务，不支持批量的操作；基于存储的可靠性的要求存储可以采用内存或硬盘

kafka具有高的吞吐量，内部采用消息的批量处理，zero-copy机制，数据的存储和获取是本地磁盘顺序批量操作，具有O(1)复杂度(与分区上存储大小无关)，消息处理的效率很高(大数据的情况下)

### 3. kafka如何优化

kafka是一个高吞吐量分布式消息系统，并且提供了持久化，其高性能的有两个重要特点

利用了磁盘连续读写性能远远高于随机读写的特点
并发，将一个topic拆分多个partition
1.将不同磁盘的多个目录配置到broker的log.dirs

```shell
例如
log.dirs=/disk1/kafka-logs,/disk2/kafka-logs,/disk3/kafka-logs
kafka会在新建partition的时候，将新partition分布在partition最少的目录上，因此，一般不能将同一个磁盘的多个目录设置到log.dirs
```

2.JVM参数配置
推荐使用最新的G1来代替CMS作为垃圾回收期
推荐使用最低版本为JDK 1.7u51

```shell
-Xms30g -Xmx30g -XX:PermSize=48m -XX:MaxPermSize=48m -XX:+UseG1GC -XX:MaxGCPauseMillis=20 -XX:InitiatingHeapOccupancyPercent=35
```

3.配置文件优化
网络和io操作线程配置优化

```shell
# broker处理消息的最大线程数
num.network.threads=xxx
# broker处理磁盘IO的线程数
num.io.threads=xxx
用于接收并处理网络请求的线程数，默认为3。其内部实现是采用Selector模型。启动一个线程作为Acceptor来负责建立连接，再配合启动num.network.threads个线程来轮流负责从Sockets里读取请求，一般无需改动，除非上下游并发请求量过大。一般num.network.threads主要处理网络io，读写缓冲区数据，基本没有io等待，配置线程数量为cpu核数加1.
num.io.threads主要进行磁盘io操作，高峰期可能有些io等待，因此配置需要大些。配置线程数量为cpu核数2倍，最大不超过3倍.
```

log数据文件刷盘策略

> 为了大幅度提高producer写入吞吐量，需要定期批量写文件

```shell
# 每当producer写入10000条消息时，刷数据到磁盘 log.flush.interval.messages=10000
# 每间隔1秒钟时间，刷数据到磁盘
log.flush.interval.ms=1000
```

日志保留策略配置

当kafka server被写入海量信息后，会生成很多数据文件，且占用大量磁盘空间，如果不及时清理，可能磁盘空间不够用，kafka默认是保留7天

```shell
# 保留三天，也可以更短
log.retention.hours=72
# 段文件配置1GB，有利于快速回收磁盘空间，重启kafka加载也会加快(如果文件过小，则文件数量比较多，
# kafka启动时是单线程扫描目录(log.dir)下所有数据文件)
log.segment.bytes=1073741824
```

配置JMX服务

> kafka server中默认是不启动jmx端口，需要用户自己配置

```shell
[lizhitao@root kafka_2.10-0.8.1]$ vim bin/kafka-run-class.sh
#最前面添加一行
JMX_PORT=8060
```

kafka更多性能优化可以参考[kafka性能调优](https://blog.csdn.net/bluetjs/article/details/80485359)

## 运维平台

### 1. gitlab cicd

GitLab提供持续集成服务。如果添加一个.gitlab-ci.yml文件到项目根目录，并配置GitLab项目使用某个Runner，然后每一次提交或者是推送都会触发CI pipeline.

### 2. jenkins 权限用户怎么设置

1. 使用第三方插件Role Strategy Plugin
2. 创建管理角色（Manage Roles）
3. 创建用户
4. Assign Roles可以为用户分配所属角色

### 3. Gitlab调优

## 其他

### 1. ldap du dc 什么意思

LDAP 目录类似于文件系统目录

```shell
CN=test,OU=developer,DC=domainname,DC=com
在上面的代码中 cn=test 可能代表一个用户名，ou=developer 代表一个 active directory 中的组织单位。这句话的含义可能就是说明 test
```

### 2. 网站访问慢排查步骤

1. 程序代码执行方面
2. 大量数据库操作
3. 域名DNS解析问题
4. 服务器环境
5. 网络的带宽
6. 用许多javascript特效
7. 访问的东西大
8. 系统资源不足
9. 防火墙的过多使用
10. 网络中某个端口形成了瓶颈导致网速变慢

### 3. 机房建设

主要内容
机房整体建设一般包括以下几个方面：建筑装饰、电气系统、空调新风系统、弱电系统、环境设备监控系统、消防系统以及自动报警系统、屏蔽系统、综合布线系统、机柜系统、服务器系统等。

建设理念

建设理念随着计算机及网络技术迅猛发展、信息化时代的到来，各行各业信息化建设势在必行。美国可用性研究中心将信息时代的企业运营可用性界定为四个层面的工作，从人员管理的可用性，到工作流程的可用性，到IT信息技术的可用性，而最基础的一层是网络环境的可用性，即NCPI（网络关键物理基础设施-也就是我们常指的计算机机房工程）。NCPI是机房中与IT系统紧密相关的、关键的一部分，是由基础建设、电力供应、空气调节、制冷系统、弱电系统、消防系统、监控系统、系统管理服务等部分组成。更具体包括：装饰装修系统、供配电系统、照明系统、防雷接地系统、门禁系统、图像监控系统、综合布线系统、新风系统、精密空调系统、机柜系统、消防报警系统以及集中监控系统等。

性能需求

机房工程设计必须满足用户当前的各项业务应用需求（尤其是作为行业专业应用），同时又面向未来快速增长的发展需求，因此应是高质量的、灵活的、开放的。设计时考虑避免下列外界因素：电磁场、易燃物、易燃性气体、磁场、爆炸物品、电力杂波、潮气、灰尘等影响。实用性和先进性 采用先进成熟的技术和设备，尽可能采用先进的技术、设备和材料，以适应高速的数据与需要，使整个系统在一段时期内保证技术的先进性，并具有良好的发展潜力，以适应未来业务的发展和技术升级的需要。

安全可靠性
为保证各项业务应用，网络必须具有高可靠性，决不能出现单点故障。要对机房布局、结构设计、设备选型、日常维护等各个方面进行高可靠性的设计和建设。在关键设备采用硬件备份、冗余等可靠性技术的基础上，采用相关的软件技术提供较强的管理机制控制手段和事故监控与安全保密等技术措施提高电脑机房的安全可靠性。

灵活性与可扩展性
计算机机房必须具有良好的灵活性与可扩展性，能够根据机房业务不断深入发展的需要，扩大设备容量和提高用户数量和质量的功能。应具备支持多种网络传输，多种物理接口的能力，提供技术升级设备更新的灵活性。

可管理性
由于机房具有一定复杂性，随着业务的不断发展，管理的任务必定会日益繁重。所以在机房的设计中，必须建立一套全面、完善的机房管理和监控系统。所选用的设备应具有智能化、可管理的功能，同时条用先进和管理监控系统设备及软件，实现先进的集中管理监控，实时监控、监测整个机房的运行状况，实时灯光、语音报警，实时事件记录，这样可以迅速确定故障，简化机房管理人员的维护工作，从而为计算机机房的安全、可靠运行提供最有力的保障。

### 4. 16个球，只有一个更轻，一个天平，怎么三次找出最轻的球

### 5. 烧一根不均匀的绳子,从头烧到尾是要1个小时.现在有若干条材质相同的绳子 问如何用烧绳的方法来计时一个小时15分钟

### 6. 一个用户不能访问，解决思路

### 7. 怎么证明一个用户被劫持了
