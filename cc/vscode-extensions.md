# Vscode Extensions

## Extensions List

### Export
```bash
code --list-extensions > vscode_extensions.txt
```

### Install
```bash
cat vscode_extensions.txt | xargs -n 1 code --install-extension
```

### Extensions List 
```text
42crunch.vscode-openapi
amiralizadeh9480.cpp-helper
bazelbuild.vscode-bazel
bierner.markdown-mermaid
charliermarsh.ruff
davidanson.vscode-markdownlint
dbaeumer.vscode-eslint
eamodio.gitlens
esbenp.prettier-vscode
foxundermoon.shell-format
golang.go
grafana.vscode-jsonnet
llvm-vs-code-extensions.vscode-clangd
llvm-vs-code-extensions.vscode-mlir
ms-python.debugpy
ms-python.python
ms-python.vscode-pylance
ms-toolsai.jupyter-keymap
ms-vscode-remote.remote-containers
ms-vscode-remote.remote-ssh
ms-vscode-remote.remote-ssh-edit
ms-vscode.remote-explorer
pb33f.vacuum
pylyzer.pylyzer
redhat.ansible
redhat.vscode-yaml
rust-lang.rust-analyzer
shd101wyy.markdown-preview-enhanced
signageos.signageos-vscode-sops
stkb.rewrap
stylelint.vscode-stylelint
takumii.markdowntable
thesofakillers.vscode-pbtxt
yzhang.markdown-all-in-one
zxh404.vscode-proto3
```