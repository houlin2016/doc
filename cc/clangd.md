# Clangd

## compile_commands.json

### CMake

```bash
cmake -DCMAKE_EXPORT_COMPILE_COMMANDS=ON ..

```

### Make

```bash
sudo dnf install bear
export no_proxy=localhost,127.0.0.1
export GRPC_DNS_RESOLVER=native
unset https_proxy; unset http_proxy
bear -- make -j$(nproc)
```


## Refer
- [wrapper: failed with: gRPC call failed: failed to connect to all addresses](https://stackoverflow.com/questions/78880003/bear-in-wsl2-but-failed-wrapper-failed-with-grpc-call-failed-failed-to-conne)
