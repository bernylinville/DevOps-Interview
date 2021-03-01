# DevOps-Interview

## Linux

> [TLDR](https://github.com/tldr-pages/tldr)

> [Linux Performance](http://www.brendangregg.com/linuxperf.html)

![avatar](http://www.brendangregg.com/Perf/linux_observability_tools.png)

![avatar](http://www.brendangregg.com/BPF/bpf_performance_tools_book.png)

1. sed

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

```
# sed 在第一行添加信息
sed -i '1i\{{text}}' {{filename}}
# -i edit files in place

# 在最后一行行前添加
sed -i '$i\{{text}}' {{filename}}

# 在最后一行行后添加
sed -i '$a\{{text}}' {{filename}}

```

2. awk

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

3. top

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

4. **ps**
5. netstat
6. tail
7. cat
8. less
9. df
10. du
11. lsof
12. lspid
13. dmesg
14. iotop
15. pidstat
16. iostat
17. mpstat
18. free
19. vmstat

## Python

## Ansible

## Docker

## Kubernetes

1. **Kubernetes Components**

![avatar](https://d33wubrfki0l68.cloudfront.net/2475489eaf20163ec0f54ddc1d92aa8d4c87c96b/e7c81/images/docs/components-of-kubernetes.svg)

> [Kubernetes Components](https://kubernetes.io/docs/concepts/overview/components/)

**Control Plane Components**

* kube-apiserver

Kubernetes API，集群的统一入口，各组件协调者，以 RESTful API提供接口服务，所有对象资源的增删改查和监听操作都交给 APIServe 处理后再提交给 Etcd 存储。

* etcd

分布式键值存储系统。用于保存集群状态数据，比如 Pod、Service 等对象信息。

* kube-scheduler

根据调度算法为新创建的 Pod 选择一个 Node 节点，可以任意部署,可以部署在同一个节点上,也可以部署在不同的节点上。

* kube-controller-manager/cloud-controller-manager

处理集群中常规后台任务，一个资源对应一个控制器，而 ControllerManager 就是负责管理这些控制器的。

**Node Components**

* kubelet

kubelet 是 Master 在 Node 节点上的 Agent，管理本机运行容器的生命周期，比如创建容器、Pod 挂载数据卷、下载 secret、获取容器和节点状态等工作。kubelet 将每个 Pod 转换成一组容器。

* kube-proxy

在 Node 节点上实现 Pod 网络代理，维护网络规则和四层负载均衡工作。

* Container Runtime

2. **Pod 的生命周期**

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

## CI/CD

## Network

## Monitoring
