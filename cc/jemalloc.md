# jemalloc 使用手册

## 1. 安装 jemalloc

### 1.1 在 CentOS Stream 9 (EL9) 安装
```sh
sudo dnf install jemalloc
```
检查版本：
```sh
jemalloc-config --version
```

### 1.2 源码编译（带 `prof` 支持）
```sh
git clone https://github.com/jemalloc/jemalloc.git
cd jemalloc
git checkout 5.3.0  # 选择合适的版本
./autogen.sh
CFLAGS="-g -O2" ./configure --enable-prof --enable-stats
make -j$(nproc)
sudo make install
```
检查是否支持 `prof`：
```sh
jemalloc-config --config | grep PROF
```

## 2. 启用 jemalloc Profiling

### 2.1 运行时开启 Profiling
```sh
export MALLOC_CONF="prof:true,lg_prof_sample:17,prof_prefix:/tmp/jeprof"
./your_program
```
生成的 heap profile 文件位于 `/tmp/jeprof.*.heap`。

### 2.2 代码中手动触发 Profiling Dump
```cpp
#include <jemalloc/jemalloc.h>
mallctl("prof.dump", NULL, NULL, NULL, 0);
```

## 3. 使用 `jeprof` 进行分析

### 3.1 生成 Heap Profile 文本
```sh
jeprof --text --show_bytes /proc/$(pidof your_program)/exe /tmp/jeprof.12345.0.f.heap > heap.txt
```

### 3.2 生成 `dot` 图
```sh
jeprof --dot /proc/$(pidof your_program)/exe /tmp/jeprof.12345.0.f.heap > heap.dot
```
转换为 `SVG`：
```sh
dot -Tsvg heap.dot -o heap.svg
```

## 4. 生成火焰图分析内存使用

### 4.1 安装 FlameGraph
```sh
git clone https://github.com/brendangregg/FlameGraph.git
cd FlameGraph
```

### 4.2 生成火焰图
```sh
cat heap.txt | ./FlameGraph/flamegraph.pl --title="jemalloc Heap Profile" > heap_flame.svg
```

## 5. 对比多个 heap 生成差异火焰图

### 5.1 导出不同时间点的 heap profile
```sh
jeprof --text --show_bytes /proc/$(pidof your_program)/exe /tmp/jeprof.12345.0.f.heap > heap1.txt
jeprof --text --show_bytes /proc/$(pidof your_program)/exe /tmp/jeprof.12345.1.f.heap > heap2.txt
```

### 5.2 计算差异
```sh
diff heap1.txt heap2.txt > heap_diff.txt
./FlameGraph/difffolded.pl heap1.txt heap2.txt > heap_diff.folded
```

### 5.3 生成火焰图
```sh
cat heap_diff.folded | ./FlameGraph/flamegraph.pl --title="jemalloc Heap Diff" > heap_diff.svg
```

## 6. 结论
1. **安装 `jemalloc`**：使用 `dnf` 或编译 `--enable-prof` 版本。
2. **启用 Profiling**：`export MALLOC_CONF="prof:true"` 或 `mallctl("prof.dump")`。
3. **分析 Heap Profile**：`jeprof --text` 或 `jeprof --dot`。
4. **火焰图分析**：`FlameGraph` 生成 SVG 直观展示内存使用热点。
5. **对比多个 Heap Profile**：`diff` 计算差异，生成火焰图找出内存增长点。

🔥 **推荐使用火焰图**，更直观地分析 `jemalloc` 的内存使用和泄漏情况。

