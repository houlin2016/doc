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

### 3.2 直接生成 `SVG` 可视化
```sh
/path/to/jeprof --show_bytes --svg /proc/$(pidof your_program)/exe /tmp/jeprof.12345.0.f.heap > heap.svg
```

## 4. 对比多个 heap 生成差异 `SVG`

### 4.1 生成 heap profile 差异图
```sh
/path/to/jeprof --show_bytes --svg --base=/path/to/base_heap.prof /path/to/binary /path/to/target_heap.prof > diff.svg
```

这样可以得到 `diff.svg`，在浏览器中查看两次 heap profile 之间的内存变化。

## 5. 结论
1. **安装 `jemalloc`**：使用 `dnf` 或编译 `--enable-prof` 版本。
2. **启用 Profiling**：`export MALLOC_CONF="prof:true"` 或 `mallctl("prof.dump")`。
3. **分析 Heap Profile**：`jeprof --text` 或 `jeprof --svg` 直接生成可视化。
4. **对比多个 Heap Profile**：使用 `jeprof --base` 直接生成 `SVG` 差异图。

🔥 **推荐直接使用 `jeprof --svg` 生成可视化结果，便捷高效！**

