# Ghostty 安装记录

安装日期：2026-04-05
系统：macOS (Darwin 25.2.0)
安装版本：Ghostty 1.3.1

## 前置检查

```bash
# 确认 Homebrew 已安装
which brew && brew --version
# 输出：/usr/local/bin/brew, Homebrew 5.1.3

# 检查是否已安装 Ghostty
ls /Applications/Ghostty.app
# 结果：未安装
```

## 安装命令

使用 Homebrew Cask 安装：

```bash
brew install --cask ghostty
```

安装过程中 Homebrew 自动完成了以下操作：
- 将 `Ghostty.app` 移动到 `/Applications/Ghostty.app`
- 链接 manpage：`/usr/local/share/man/man1/ghostty.1`、`/usr/local/share/man/man5/ghostty.5`
- 链接 Bash 补全：`/usr/local/etc/bash_completion.d/ghostty`
- 链接 Fish 补全：`/usr/local/share/fish/vendor_completions.d/ghostty.fish`
- 链接 Zsh 补全：`/usr/local/share/zsh/site-functions/_ghostty`

## 验证安装

```bash
/Applications/Ghostty.app/Contents/MacOS/ghostty --version
```

输出：
```
Ghostty 1.3.1
Version
  - version: 1.3.1
  - channel: stable
```

## 配置修改

本次安装未手动修改任何配置文件。Homebrew Cask 自动创建的符号链接（manpage、shell 补全）均位于系统标准路径。

Ghostty 用户配置文件默认位置（如需自定义）：
- `~/.config/ghostty/config`

## 后续操作建议

- 首次启动：`open -a Ghostty` 或在 Launchpad 中打开
- 创建配置文件以自定义主题、字体等：`mkdir -p ~/.config/ghostty && touch ~/.config/ghostty/config`
- 升级：`brew upgrade --cask ghostty`
- 卸载：`brew uninstall --cask ghostty`
