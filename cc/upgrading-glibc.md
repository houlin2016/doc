# 升级共享库 (`glibc` 等) 方法总结

在某些情况下，我们需要升级 `glibc` 或其他共享库 (`libstdc++.so` 等)，但不能直接覆盖系统库，可能的原因包括：
- 服务器权限限制，不能直接 `yum` 或 `apt` 升级。
- 避免影响系统上已有的其他程序。
- 需要在不同版本的 `glibc` 之间切换测试。

## 方案 1：使用 patchelf 修改 ELF 依赖路径
`patchelf` 允许我们修改可执行文件或动态库的 `rpath` 或 `interpreter`，让程序使用新的 `glibc` 版本。

### **步骤 1：编译新版本 `glibc`**
```bash
wget http://ftp.gnu.org/gnu/libc/glibc-2.35.tar.gz
mkdir glibc-build && cd glibc-build
tar -xvzf ../glibc-2.35.tar.gz
cd glibc-2.35
mkdir build && cd build
../configure --prefix=/opt/glibc-2.35 --disable-werror
make -j$(nproc)
sudo make install
```

### **步骤 2：使用 `patchelf` 修改二进制文件**
```bash
patchelf --set-interpreter /opt/glibc-2.35/lib/ld-2.35.so my_binary
patchelf --set-rpath /opt/glibc-2.35/lib my_binary
```
这样，`my_binary` 运行时会使用 `/opt/glibc-2.35/lib/ld-2.35.so` 作为动态链接器，并搜索 `/opt/glibc-2.35/lib` 目录中的共享库。

## 方案 2：使用 `LD_LIBRARY_PATH`
如果不想修改二进制文件，可以通过 `LD_LIBRARY_PATH` 让程序加载新 `glibc`。

```bash
export LD_LIBRARY_PATH=/opt/glibc-2.35/lib:$LD_LIBRARY_PATH
./my_binary
```

但 `LD_LIBRARY_PATH` 可能影响其他程序，建议仅在需要时使用。

## 方案 3：使用 `chroot` 或 `namespace` 运行新 `glibc`
可以使用 `chroot` 或 `bubblewrap` 让应用程序在隔离环境中运行：
```bash
sudo chroot /opt/glibc-2.35 /bin/bash
```
或者使用 `bubblewrap`：
```bash
bwrap --ro-bind /opt/glibc-2.35 /glibc --chdir /glibc /glibc/bin/bash
```

## **验证升级结果**
可以使用 `ldd` 检查 `glibc` 版本：
```bash
ldd --version
ldd my_binary
```

或者直接运行：
```bash
/opt/glibc-2.35/lib/ld-2.35.so --version
```

## **结论**
- `patchelf` 是最灵活的方法，适用于需要持久修改的情况。
- `LD_LIBRARY_PATH` 适用于临时切换 `glibc` 版本。
- `chroot` 和 `bubblewrap` 适用于隔离运行特定应用。

根据你的需求选择合适的方式升级 `glibc` 或其他共享库！

## **参考**
- 原文链接: [Multi Glibc & patchelf](https://fancyerii.github.io/2024/03/07/multi-glibc-patchelf/)

