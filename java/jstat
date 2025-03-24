# Jstat

## 1. jstat 简介
`jstat`（Java Virtual Machine Statistics Monitoring Tool）是 JDK 自带的工具，用于监控 Java 虚拟机的运行状态，特别是垃圾回收（GC）情况。

## 2. 基本使用
### 2.1 查看 JVM 进程 ID（PID）
在使用 `jstat` 之前，需要获取目标 JVM 进程的 PID。
```sh
jps -l
```
示例输出：
```
12345 org.example.MyApplication
```
其中 `12345` 为目标 JVM 进程 ID。

### 2.2 运行 jstat 命令
`jstat` 的基本语法如下：
```sh
jstat -<option> <pid> [interval] [count]
```
- `<option>`：要查看的统计信息类型，如 `gcutil`、`gccapacity` 等。
- `<pid>`：目标 JVM 进程 ID。
- `[interval]`（可选）：采样时间间隔（单位：毫秒）。
- `[count]`（可选）：采样次数。

## 3. jstat 监控 GC 相关参数

### 3.1 `jstat -gcutil`
`gcutil` 主要用于查看 GC 活跃情况，包括堆各区域的使用率。
```sh
jstat -gcutil <pid> 1000 10
```
示例输出：
```
  S0     S1     E      O      M     CCS   YGC     YGCT   FGC    FGCT     GCT  
  0.00  98.76  32.45  67.89  95.67  78.34  3456   12.34   23     45.67    58.01
```
**字段说明：**
- `S0` / `S1`：两个 Survivor 区的使用率（%）。
- `E`（Eden）：Eden 区使用率（%）。
- `O`（Old）：老年代使用率（%）。
- `M`（Metaspace）：元空间使用率（%）。
- `CCS`（Compressed Class Space）：压缩类空间使用率（%）。
- `YGC`（Young GC Count）：年轻代 GC 次数。
- `YGCT`（Young GC Time）：年轻代 GC 所用时间（秒）。
- `FGC`（Full GC Count）：Full GC 次数。
- `FGCT`（Full GC Time）：Full GC 所用时间（秒）。
- `GCT`（Total GC Time）：GC 总时间（秒）。

#### 3.1.1 通过 `gcutil` 分析 GC 状况
- **年轻代 GC 频繁，`YGC` 值较大**：可能是 Eden 空间过小或对象存活率高。
- **Full GC 频繁，`FGC` 值较大**：可能是老年代满了或元空间不足。
- **`GCT` 占 CPU 时间过高**：GC 可能影响应用性能，需要优化。

### 3.2 `jstat -gccapacity`
`gccapacity` 主要用于查看堆各个区域的容量信息。
```sh
jstat -gccapacity <pid> 1000 10
```
示例输出：
```
  NGCMN    NGCMX     NGC    S0C    S1C     EC     OGCMN    OGCMX     OGC     OC
  5120.0  10240.0  8192.0  1024.0 1024.0  6144.0  10240.0  20480.0  16384.0  16384.0
```
**字段说明：**
- `NGCMN` / `NGCMX`：新生代最小/最大容量（KB）。
- `NGC`：当前新生代容量（KB）。
- `S0C` / `S1C`：Survivor 0 / 1 区容量（KB）。
- `EC`：Eden 区容量（KB）。
- `OGCMN` / `OGCMX`：老年代最小/最大容量（KB）。
- `OGC`：当前老年代容量（KB）。
- `OC`：老年代当前使用容量（KB）。

#### 3.2.1 通过 `gccapacity` 分析 GC 状况
- **`NGC` 靠近 `NGCMX`，`OGC` 靠近 `OGCMX`**：可能需要增加堆大小。
- **`S0C` / `S1C` 过小**：可能导致对象直接进入老年代，增大 Full GC 频率。

## 4. 结合 `jstat` 进行 GC 调优

### 4.1 确保年轻代 GC 频率适中
```sh
jstat -gcutil <pid> 1000
```
**优化建议：**
- 如果 `YGC` 过高，可以增大 `-Xmn`（新生代大小）。
- 观察 `E`，如果 Eden 区长期接近 100%，可能需要增大 Eden 空间。

### 4.2 降低 Full GC 频率
```sh
jstat -gcutil <pid> 1000
```
**优化建议：**
- `FGC` 过高：可能是老年代满了，增大 `-Xmx`。
- `FGCT` 时间过长：检查 `Metaspace` 是否足够，可调整 `-XX:MetaspaceSize`。

### 4.3 调整 Survivor 比例
```sh
jstat -gccapacity <pid> 1000
```
**优化建议：**
- 如果 `S0C` / `S1C` 太小，导致对象直接晋升老年代，可以调整 `-XX:SurvivorRatio`。

## 5. 总结
- `jstat -gcutil` 观察 GC 频率和时间。
- `jstat -gccapacity` 查看堆内存分配情况。
- 结合 `YGC`、`FGC`、`GCT` 数据分析 GC 效率。
- 适当调整 `-Xms`、`-Xmx`、`-Xmn`、`-XX:MetaspaceSize` 进行优化。

利用 `jstat` 监控 GC 状况，有助于优化 Java 应用的性能。🚀

