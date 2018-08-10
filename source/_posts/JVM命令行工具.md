---
title: JVM命令行工具
date: 2018-08-08 17:13:32
categories:
- Java虚拟机
tags:
- Java
- JVM
---

  ![](http://pbsg2r9io.bkt.clouddn.com/18-8-8/54097003.jpg)
  
  本文主要目的是从学习**深入理解Java虚拟机**中，针对性的做下读书笔记。整理主要从以下几个方面来梳理 虚拟机性能监控与故障处理工具。
  
  <!-- more --> 

### JDK命令行工具

#### jps

  可以列出正在运行的虚拟机进程，并显示虚拟机执行主类

#### jstat

  虚拟机统计信息监视工具,它可以显示虚拟机进程中的类装载、内存、垃圾收集、JIT编译等运行数据。
  
  jstat 命令格式: `jstat -<option> [-t] [-h<lines>] <vmid> [<interval> [<count>]]`
  
  `jstat -<命令> [-t] [-h<lines>] <进程号> [间隔时间/毫秒数] [查询次数]`
  
  示例：`jstat -gc 20257 250 10`  查看进程20257的 java堆的使用情况，每隔250ms执行一次，共查看10次。
  
  **options常用参数详解**
  
  + -gc 查看Java堆的使用情况。
  
  ```
    # C : Capacity 总容量  U : used 已使用容量
    [root@WINC201800004C75DI ~]# jstat -gc -h 10 20257 1000 10
     Survivor0/1   Survivor0/1    Eden     Eden      Old         Old     Method           压缩类空间       YoungGC[次数/耗时] FullGC[次数/耗时]  总耗时
     S0C    S1C    S0U    S1U      EC       EU        OC         OU       MC     MU       CCSC   CCSU     YGC     YGCT      FGC     FGCT     GCT   
    2560.0 17408.0 2464.2  0.0   505344.0 327751.9  166912.0   28920.2   63912.0 61147.5 8616.0 8056.2     20    0.188      3       0.266    0.454
    2560.0 17408.0 2464.2  0.0   505344.0 327751.9  166912.0   28920.2   63912.0 61147.5 8616.0 8056.2     20    0.188      3       0.266    0.454
  ```
  
  + -gcutil 与gc类同，主要关注空间使用百分比
  
  ```
  [root@WINC201800004C75DI ~]# jstat -gcutil 20257
    S0     S1     E      O      M     CCS    YGC   YGCT    FGC  FGCT     GCT   
   96.26   0.00  69.86  17.33  95.67  93.50  20    0.188   3    0.266    0.454
  ```
  
  + -gcnew 查看新生代GC状况
  
    ```
    [root@WINC201800004C75DI ~]# jstat -gcnew 20257
     S0C    S1C    S0U    S1U   TT  MTT  DSS      EC       EU     YGC     YGCT  
    2560.0 17408.0 2464.2 0.0   15  15   17408.0  505344.0 353052.0     20    0.188
    # TT: Tenuring threshold(提升阈值)
    # MTT：最大的tenuring threshold
    # DSS：survivor区域大小 (KB)
    ```

#### jmap Java内存映像工具
  
  `jmap [option] vmid `
  
  + -dump 格式：`jmap -dump:[live,] format=b, file=<filename>`
  
  生成Java堆转储快照。(生成JVM内存信息)
  
  ```
  [root@WINC201800004C75DI ~]# jmap -dump:live,format=b,file=aaa.bin 20257
  Dumping heap to /root/aaa.bin ...
  Heap dump file created
  [root@WINC201800004C75DI ~]# ls aaa.bin 
  aaa.bin
  ```
  
  + -heap 格式 `jmap -heap 20257`
  
  查看整个JVM内存状态
  
  ```
  [root@WINC201800004C75DI ~]# jmap -heap 20257
  Attaching to process ID 20257, please wait...
  Debugger attached successfully.
  Server compiler detected.
  JVM version is 25.162-b12
  
  using thread-local object allocation.
  Parallel GC with 4 thread(s)
  // 堆内存初始化配置
  Heap Configuration:
     MinHeapFreeRatio         = 0                       //对应jvm启动参数-XX:MinHeapFreeRatio  设置JVM堆最小空闲比率(default 0)   
     MaxHeapFreeRatio         = 100                     //对应jvm启动参数-XX:MaxHeapFreeRatio  设置JVM堆最大空闲比率(default 100)
     MaxHeapSize              = 3137339392 (2992.0MB)   //对应jvm启动参数-XX:MaxHeapSize=      设置JVM堆的最大大小
     NewSize                  = 65536000 (62.5MB)       //对应jvm启动参数-XX:NewSize=          设置JVM堆的‘新生代’的默认大小
     MaxNewSize               = 1045430272 (997.0MB)    //对应jvm启动参数-XX:MaxNewSize=       设置JVM堆的‘新生代’的最大大小
     OldSize                  = 131596288 (125.5MB)     //对应jvm启动参数-XX:OldSize=          设置JVM堆的‘老生代’的大小
     NewRatio                 = 2                       //对应jvm启动参数-XX:NewRatio=:        设置‘新生代’和‘老生代’的大小比率
     SurvivorRatio            = 8                       //对应jvm启动参数-XX:SurvivorRatio=    设置年轻代中Eden区与Survivor区的大小比值
     MetaspaceSize            = 21807104 (20.796875MB)  
     CompressedClassSpaceSize = 1073741824 (1024.0MB)   
     MaxMetaspaceSize         = 17592186044415 MB
     G1HeapRegionSize         = 0 (0.0MB)
  // 堆内存分布
  Heap Usage:
  PS Young Generation                                   ## 新生代
  Eden Space:                                           |-- ### Eden区
     capacity = 445644800 (425.0MB)                     // Eden区总容量
     used     = 143272 (0.13663482666015625MB)          // Eden区使用容量
     free     = 445501528 (424.86336517333984MB)        // Eden区剩余容量
     0.032149370978860295% used                         // 使用占比
  From Space:                                           |-- ### //其中一个Survivor区的内存分布
     capacity = 524288 (0.5MB)                          
     used     = 0 (0.0MB)
     free     = 524288 (0.5MB)
     0.0% used
  To Space:                                             |-- ### //另一个Survivor区的内存分布
     capacity = 14680064 (14.0MB)
     used     = 0 (0.0MB)
     free     = 14680064 (14.0MB)
     0.0% used
  PS Old Generation                                     |-- ###  //当前的Old区内存分布
     capacity = 333447168 (318.0MB)
     used     = 25710936 (24.519859313964844MB)
     free     = 307736232 (293.48014068603516MB)
     7.7106475830078125% used
  
  23755 interned Strings occupying 2910336 bytes.

  ```
  
  + -histo `jmap -histo[:live]`
  
  查看JVM堆中对象详细占用情况
  
  ```
  [root@WINC201800004C75DI ~]# jmap -histo:live 20257 > aa.txt
   num     #instances         #bytes  class name
  ----------------------------------------------
     1:         69004       10170952  [C
     2:         49257        1576224  java.util.concurrent.ConcurrentHashMap$Node
     3:         63083        1513992  java.lang.String
     4:         12854        1422896  java.lang.Class
     5:         14722        1295536  java.lang.reflect.Method
     6:          2836         918424  [B
     7:         10628         640464  [Ljava.lang.Object;
     8:         13610         544400  java.util.LinkedHashMap$Entry
     9:         30580         489280  java.lang.Object
    10:          5169         473744  [I
    11:          5646         453920  [Ljava.util.HashMap$Node;
    12:           205         411808  [Ljava.util.concurrent.ConcurrentHashMap$Node;
    ... 省略 ...
  ```

#### jstack: Java堆栈跟踪工具 

  `jstack [option] pid`
  
  + `jstack -l 20257` 除堆栈外，打印锁的附加信息
  
  + `jstack -F 20257` 当正常输出请求不被响应时，强制输入线程堆栈
  
  + `jstack -m 20257` 如果调用本地方法，显示C/C++堆栈
  
  使用示例：
  
  + jps 查看当前进程
  
  + top -Hp 20257 查看当前进程最耗资源的线程
  
  ```
  [root@WINC201800004C75DI ~]# top -Hp 20257
  top - 19:50:35 up 21 days,  3:02,  1 user,  load average: 0.00, 0.00, 0.00
  Tasks:  34 total,   0 running,  34 sleeping,   0 stopped,   0 zombie
  Cpu(s):  0.1%us,  0.1%sy,  0.0%ni, 99.8%id,  0.0%wa,  0.0%hi,  0.0%si,  0.0%st
  Mem:  12249784k total, 11710396k used,   539388k free,   269812k buffers
  Swap:  8388604k total,    11016k used,  8377588k free,  3877564k cached
  
  PID USER      PR  NI  VIRT  RES  SHR S %CPU %MEM    TIME+  COMMAND                                                                                                                                                                    
  20257 root      20   0 6754m 1.0g  14m S  0.0  8.5   0:00.00 java                                                                                                                                                                        
  20258 root      20   0 6754m 1.0g  14m S  0.0  8.5   0:06.04 java                                                                                                                                                                        
  20259 root      20   0 6754m 1.0g  14m S  0.0  8.5   0:00.46 java                                                                                                                                                                        
  20260 root      20   0 6754m 1.0g  14m S  0.0  8.5   0:00.36 java                                                                                                                                                                        
  20261 root      20   0 6754m 1.0g  14m S  0.0  8.5   0:00.38 java                                                                                                                                                                        
  20262 root      20   0 6754m 1.0g  14m S  0.0  8.5   0:00.41 java                                                                                                                                                                        
  20263 root      20   0 6754m 1.0g  14m S  0.0  8.5   0:08.35 java                                                                                                                                                                        
  20264 root      20   0 6754m 1.0g  14m S  0.0  8.5   0:00.00 java                                                                                                                                                                        
  20265 root      20   0 6754m 1.0g  14m S  0.0  8.5   0:00.04 java                                                                                                                                                                        
  20266 root      20   0 6754m 1.0g  14m S  0.0  8.5   0:00.00 java                                                                                                                                                                        
  20267 root      20   0 6754m 1.0g  14m S  0.0  8.5   0:11.13 java                                                                                                                                                                        
  20268 root      20   0 6754m 1.0g  14m S  0.0  8.5   0:11.28 java                                                                                                                                                                        
  20269 root      20   0 6754m 1.0g  14m S  0.0  8.5   0:04.41 java                                                                                                                                                                        
  20270 root      20   0 6754m 1.0g  14m S  0.0  8.5   0:00.00 java                                                                                                                                                                        
  20271 root      20   0 6754m 1.0g  14m S  0.0  8.5   2:26.33 java  
  ```
  
  + 选择线程号20271 ， printf "%x\n" 20271 得到 十六进制
  
  ```
  [root@WINC201800004C75DI ~]# printf "%x\n" 20271
  4f2f
  ```
  
  + jstack 输出20271的堆栈
  
  ```
  [root@WINC201800004C75DI ~]# jstack -l 20257 | grep 4f2f
  "VM Periodic Task Thread" os_prio=0 tid=0x00007f38b418c800 nid=0x4f2f waiting on condition 
  ```
  
  得到这是个虚拟机的进程。

### 其他

  + JIT生成代码反汇编
  
  + JDK的可视化工具：JConsole、VisualVM  
  