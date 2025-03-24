# Git Shell

## Starship

```bash
curl -sS https://starship.rs/install.sh | sh
```

Add in .bashrc or .zshrc

```bash
eval "$(starship init bash)"  # bash
eval "$(starship init zsh)"   # zsh
```

## Powerline

- Powerline 是一个高度可定制的状态栏插件，广泛用于 shell（如 Bash、Zsh）以及其他工具（如 Vim）。
- 它提供了丰富的图标和字体，可以显示当前路径、git 状态、时间等信息。
- [Powerline GitHub] (https://github.com/powerline/powerline)

```bash
sudo apt-get install powerline
```

## Zsh + Oh My Zsh

- Zsh 是一个功能强大的 shell，提供更好的自动补全和自定义功能。Oh My Zsh 是 Zsh 的一个社区驱动框架，提供了多种插件和主题，可以增强终端体验。
- 通过使用 Powerlevel10k 等主题，可以使终端更加美观，显示 git 状态、命令执行时间、系统信息等。

```bash
# 安装 Oh My Zsh（Zsh 需要预安装）：
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# 安装 Powerlevel10k 主题（推荐）：
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git $ZSH_CUSTOM/themes/powerlevel10k
```

## Fish Shell

- Fish 是另一个功能强大的 Shell，内置了许多功能，如语法高亮、智能补全等。其默认提示符已经相当美观，并且可以很容易地扩展和自定义。
- Fish 提供了一个内置的主题引擎，能够提供即时反馈和丰富的提示功能。

```bash
sudo apt-get install fish
```

## Bash-git-prompt

- Bash-git-prompt 是一个用于 Bash 的插件，可以显示当前 git 仓库的状态（如分支、修改、未跟踪文件等）。
- 它非常轻量，并且能无缝集成到现有的 Bash 配置中。

```bash
git clone https://github.com/magicmonty/bash-git-prompt.git ~/.bash-git-prompt
```

## Prezto

- Prezto 是 Zsh 的另一个配置框架，它的功能非常强大，并且比 Oh My Zsh 更加精简、快速。
- 它内置了很多功能，比如自动补全、git 状态、主题和插件等。

```bash
git clone --recursive https://github.com/sorin-ionescu/prezto.git $ZSH_CONFIG_DIR
```

## Pure

- Pure 是一个专为 Zsh 设计的提示符，提供了简单、快速和现代化的提示符，它会显示如 git 状态、执行时间等信息。
- Pure 的重点是速度和美观，默认配置非常精简，提供极佳的性能。

```bash
npm install --global pure-prompt
```

## Themer

- Themer 是一个基于 Node.js 的命令行工具，可以快速安装和切换终端主题，包括颜色配置、字体等。它支持多种终端（如 bash、zsh、fish）和编辑器（如 VSCode、iTerm2）。

```bash
npm install -g themer
```

