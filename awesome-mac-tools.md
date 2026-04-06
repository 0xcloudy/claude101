# Awesome Mac Tools

macOS 终端工作流安装与配置记录。

工具栈：**Ghostty** (终端) + **Starship** (提示符) + **fzf** (模糊搜索) + **zoxide** (智能跳转) + **Yazi** (文件管理器) + **fd** (fzf 搜索后端)

统一视觉主题：**Catppuccin Mocha**

---

## Ghostty

> 现代化 GPU 加速终端模拟器

- 安装日期：2026-04-05
- 版本：1.3.1

### 安装命令

```bash
brew install --cask ghostty
```

### Homebrew 自动完成的操作

- 将 `Ghostty.app` 移动到 `/Applications/Ghostty.app`
- 链接 manpage：`/usr/local/share/man/man1/ghostty.1`、`/usr/local/share/man/man5/ghostty.5`
- 链接 Bash 补全：`/usr/local/etc/bash_completion.d/ghostty`
- 链接 Fish 补全：`/usr/local/share/fish/vendor_completions.d/ghostty.fish`
- 链接 Zsh 补全：`/usr/local/share/zsh/site-functions/_ghostty`

### 配置修改

配置文件 `~/.config/ghostty/config`：

```ini
# ── 主题 ─────────────────────────────────────────────────────
theme = catppuccin-mocha

# ── 字体（需提前安装 JetBrains Mono）────────────────────────
font-family = JetBrainsMono Nerd Font Mono
font-size = 14
font-feature = calt
font-feature = liga
font-thicken = true

# ── 透明度 ────────────────────────────────────────────────────
background-opacity = 0.95

# ── 窗口内边距 ───────────────────────────────────────────────
window-padding-x = 14
window-padding-y = 12
window-padding-balance = true
window-inherit-working-directory = true

# ── 光标 ─────────────────────────────────────────────────────
cursor-style = bar
cursor-style-blink = true

# ── Shell 集成 ───────────────────────────────────────────────
shell-integration = zsh

# ── 滚动历史 ─────────────────────────────────────────────────
scrollback-limit = 10000

# ── macOS 专属 ───────────────────────────────────────────────
macos-titlebar-style = hidden

# ── 快捷键 ────────────────────────────────────────────────────
keybind = cmd+t=new_tab
keybind = cmd+w=close_surface
keybind = cmd+d=new_split:right
keybind = cmd+shift+d=new_split:down
```

### 快捷键

| 快捷键 | 功能 |
|--------|------|
| `Cmd+T` | 新建标签页 |
| `Cmd+W` | 关闭当前面板 |
| `Cmd+D` | 水平分屏 |
| `Cmd+Shift+D` | 垂直分屏 |
| `Cmd+N` | 新建窗口 |
| `Cmd+K` | 清屏 |
| `Cmd++` / `Cmd+-` | 字体放大 / 缩小 |

### 常用操作

```bash
open -a Ghostty                  # 启动
ghostty +list-themes             # 查看所有可用主题
brew upgrade --cask ghostty      # 升级
brew uninstall --cask ghostty    # 卸载
```

---

## fzf

> 通用命令行模糊搜索工具

- 安装日期：2026-04-06
- 版本：0.71.0

### 安装命令

```bash
brew install fzf fd    # fd 作为更快的文件搜索后端
```

### 配置修改

在 `~/.zshrc` 中添加：

```bash
# ── fzf 初始化 ────────────────────────────────────────────────
eval "$(fzf --zsh)"

# ── fzf 外观（Catppuccin 配色）────────────────────────────────
export FZF_DEFAULT_OPTS="
  --color=bg+:#313244,bg:#1e1e2e,spinner:#f5e0dc,hl:#f38ba8
  --color=fg:#cdd6f4,header:#f38ba8,info:#cba6f7,pointer:#f5e0dc
  --color=marker:#b4befe,fg+:#cdd6f4,prompt:#cba6f7,hl+:#f38ba8
  --layout=reverse --border=rounded --height=40%
  --prompt='  ' --pointer='>' --marker='v'
"

# ── fzf 文件搜索后端（需安装 fd）─────────────────────────────
export FZF_DEFAULT_COMMAND='fd --type f --hidden --follow --exclude .git'
export FZF_CTRL_T_COMMAND="$FZF_DEFAULT_COMMAND"
```

### 快捷键

| 快捷键 | 功能 |
|--------|------|
| `Ctrl+R` | 模糊搜索历史命令 |
| `Ctrl+T` | 模糊搜索文件，将路径插入命令行 |
| `Alt+C` | 模糊搜索子目录并直接跳转 |
| `**` + Tab | 触发补全，如 `vim **` + Tab 选择文件 |

### 常用命令

```bash
vim $(fzf)                           # 模糊搜索文件并用 vim 打开
kill -9 $(ps aux | fzf)              # 模糊搜索进程并 kill
git checkout $(git branch | fzf)     # 模糊搜索分支并切换
brew upgrade fzf                     # 升级
brew uninstall fzf                   # 卸载
```

---

## zoxide

> 更智能的 cd 命令，基于使用频率和最近访问自动跳转目录

- 安装日期：2026-04-06
- 版本：0.9.9

### 安装命令

```bash
brew install zoxide
```

### 配置修改

在 `~/.zshrc` 中添加：

```bash
eval "$(zoxide init zsh)"
```

### 常用命令

| 命令 | 功能 |
|------|------|
| `z foo` | 跳到最常访问的含 foo 的目录 |
| `z foo bar` | 多词匹配，如 `z work api` |
| `zi` | 交互式选择目录（fzf 弹窗） |
| `z -` | 跳回上一个目录 |
| `zoxide query -l` | 查看所有已记录目录 |
| `zoxide edit` | 手动编辑目录数据库 |

```bash
brew upgrade zoxide    # 升级
brew uninstall zoxide  # 卸载
```

---

## Starship

> 轻量、快速、高度可定制的跨 shell 提示符

- 安装日期：2026-04-06
- 版本：1.24.2

### 安装命令

```bash
brew install starship
```

### 配置修改

**1. `~/.zshrc` 修改：**

将 oh-my-zsh 主题置空，改用 starship：

```bash
# 原来
ZSH_THEME="robbyrussell"
# 改为
ZSH_THEME=""
```

在 `~/.zshrc` 末尾添加（必须放最后）：

```bash
eval "$(starship init zsh)"
```

**2. 配置文件 `~/.config/starship.toml`（Powerline 风格 + Catppuccin 配色）：**

```toml
# Powerline style prompt (colored background segments)

format = """
[](fg:#a6da95)\
[ $os ](bg:#a6da95 fg:#1e1e2e bold)\
[](fg:#a6da95 bg:#7dc4e4)\
[ $directory](bg:#7dc4e4 fg:#1e1e2e bold)\
[](fg:#7dc4e4 bg:#c6a0f6)\
[$git_branch$git_status](bg:#c6a0f6 fg:#1e1e2e bold)\
[](fg:#c6a0f6)\
$nodejs$python$rust$golang\
$cmd_duration\
$line_break\
$character"""

[os]
disabled = false
format = "$symbol"

[os.symbols]
Macos = ""

[directory]
format = "[$path ]($style)"
style = "bg:#7dc4e4 fg:#1e1e2e bold"
truncation_length = 3
truncate_to_repo = false

[git_branch]
format = "[ $symbol$branch ]($style)"
symbol = " "
style = "bg:#c6a0f6 fg:#1e1e2e bold"

[git_status]
format = "[$all_status$ahead_behind]($style)"
style = "bg:#c6a0f6 fg:#1e1e2e bold"
conflicted = "!"
ahead = "↑${count}"
behind = "↓${count}"
modified = "!"
untracked = "?"
staged = "+"
renamed = "»"
deleted = "✘"

[character]
success_symbol = "[❯](bold #a6e3a1)"
error_symbol = "[❯](bold #f38ba8)"

[nodejs]
format = "[ $symbol($version) ](fg:#a6e3a1)"
symbol = " "

[python]
format = "[ $symbol($version) ](fg:#f9e2af)"
symbol = " "

[rust]
format = "[ $symbol($version) ](fg:#f38ba8)"
symbol = " "

[golang]
format = "[ $symbol($version) ](fg:#89dceb)"
symbol = " "

[cmd_duration]
min_time = 2000
style = "#f9e2af"
format = "[ $duration ]($style)"
```

### 提示符效果

```
  ~/projects/claude101   main !?
❯
```

绿色块（ macOS 图标）→ 蓝色块（目录路径）→ 紫色块（git 分支 + 状态），Powerline 箭头衔接。

### 常用操作

```bash
starship explain              # 解释当前提示符每段含义
starship timings              # 查看各模块渲染耗时
brew upgrade starship         # 升级
brew uninstall starship       # 卸载
```

---

## Yazi

> 终端文件管理器，三栏布局，支持图片/文件预览

- 安装日期：2026-04-06
- 版本：26.1.22

### 安装命令

```bash
brew install yazi
```

### 配置修改

**1. `~/.config/yazi/yazi.toml`：**

```toml
[manager]
show_hidden = true
sort_by = "natural"
sort_sensitive = false
sort_reverse = false

[preview]
image_filter = "lanczos3"
image_quality = 75
tab_size = 2
max_width = 600
max_height = 900

[opener]
edit = [
  { run = 'nvim "$@"', block = true, desc = "NeoVim" },
]
```

**2. `~/.config/yazi/keymap.toml`：**

```toml
[manager]
keymap = [
  # 导航
  { on = "h", run = "leave",        desc = "进入父目录" },
  { on = "l", run = "enter",        desc = "进入选中目录" },
  { on = "j", run = "arrow 1",      desc = "向下移动" },
  { on = "k", run = "arrow -1",     desc = "向上移动" },
  { on = "g", run = "arrow -999",   desc = "跳到顶部" },
  { on = "G", run = "arrow 999",    desc = "跳到底部" },

  # 操作
  { on = "y", run = "yank",         desc = "复制" },
  { on = "d", run = "yank --cut",   desc = "剪切" },
  { on = "p", run = "paste",        desc = "粘贴" },
  { on = "D", run = "remove",       desc = "删除" },
  { on = "r", run = "rename",       desc = "重命名" },
  { on = "a", run = "create",       desc = "新建文件/目录" },

  # 搜索（集成 fzf）
  { on = "f", run = "search --via=fd",   desc = "搜索文件" },
  { on = "F", run = "search --via=rg",   desc = "搜索内容" },
  { on = "z", run = "plugin zoxide",     desc = "zoxide 跳转" },

  # 其他
  { on = ".", run = "toggle_hidden",     desc = "切换隐藏文件" },
  { on = "q", run = "quit",             desc = "退出" },
]
```

**3. 在 `~/.zshrc` 中添加 shell wrapper：**

```bash
# ── Yazi：退出后 cd 到最后所在目录 ────────────────────────────
function y() {
  local tmp="$(mktemp -t "yazi-cwd.XXXXXX")"
  yazi "$@" --cwd-file="$tmp"
  if cwd="$(cat -- "$tmp")" && [ -n "$cwd" ] && [ "$cwd" != "$PWD" ]; then
    cd -- "$cwd"
  fi
  rm -f -- "$tmp"
}
```

### 快捷键

| 快捷键 | 功能 |
|--------|------|
| `h / l` | 进入父目录 / 子目录 |
| `j / k` | 上下移动光标 |
| `Space` | 多选文件 |
| `y / d / p` | 复制 / 剪切 / 粘贴 |
| `D` | 删除文件 |
| `r` | 重命名 |
| `a` | 新建文件或目录（末尾加 `/` 为目录） |
| `f` | fd 搜索文件 |
| `F` | rg 搜索内容 |
| `z` | zoxide 跳转目录 |
| `.` | 显示/隐藏 隐藏文件 |
| `q` | 退出 |

### 常用操作

```bash
y                     # 启动 Yazi（退出后自动 cd 到最后浏览的目录）
yazi                  # 直接启动（退出后不切目录）
brew upgrade yazi     # 升级
brew uninstall yazi   # 卸载
```

> Yazi 图片预览需要终端支持 Kitty 图形协议，Ghostty 原生支持，开箱即用。

---

## ~/.zshrc 完整改动摘要

本次对 `~/.zshrc` 的修改：

1. `ZSH_THEME="robbyrussell"` → `ZSH_THEME=""`（禁用 oh-my-zsh 主题，改用 starship）
2. 移除重复的 `export PATH` 行
3. 在文件末尾添加工具初始化（**顺序不可颠倒**）：

```bash
# ── fzf 初始化 ────────────────────────────────────────────────
eval "$(fzf --zsh)"
export FZF_DEFAULT_OPTS="..."          # Catppuccin 配色
export FZF_DEFAULT_COMMAND='fd ...'    # fd 搜索后端
export FZF_CTRL_T_COMMAND="$FZF_DEFAULT_COMMAND"

# ── zoxide 初始化 ─────────────────────────────────────────────
eval "$(zoxide init zsh)"

# ── Yazi shell wrapper ───────────────────────────────────────
function y() { ... }

# ── Starship 初始化（必须放最后）──────────────────────────────
eval "$(starship init zsh)"
```

---

## 安装验证清单

- [ ] 打开 Ghostty，确认 Catppuccin Mocha 主题和 JetBrains Mono 字体正确显示
- [ ] 确认 Starship 提示符带 git 信息和语言标识
- [ ] `cd` 到任意项目目录，再用 `z 项目名` 跳回，验证 zoxide 记忆
- [ ] 按 `Ctrl+R`，确认出现 fzf 历史搜索弹窗（Catppuccin 配色）
- [ ] 按 `Ctrl+T`，确认出现文件模糊搜索弹窗
- [ ] 输入 `y` 启动 Yazi，确认三栏布局正常，图片可预览
- [ ] 在 Yazi 内按 `z`，确认 zoxide 跳转弹窗正常

---

## 参考资料

- Ghostty：https://ghostty.org
- Starship：https://starship.rs
- zoxide：https://github.com/ajeetdsouza/zoxide
- fzf：https://github.com/junegunn/fzf
- Yazi：https://yazi-rs.github.io
- fd：https://github.com/sharkdp/fd
- JetBrains Mono 字体：https://www.jetbrains.com/lp/mono/
- Catppuccin 主题：https://github.com/catppuccin
