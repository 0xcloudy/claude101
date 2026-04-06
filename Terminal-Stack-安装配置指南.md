# Terminal Stack 终端工作流安装配置指南

> 适用系统：macOS / Linux（Ubuntu / Arch）
> 工具组合：**Ghostty · Starship · zoxide · fzf · Yazi**

---

## 0. 工具概览

| 工具 | 定位 |
|------|------|
| Ghostty | 终端模拟器 · GPU渲染 · 支持分屏/主题/连字 |
| Starship | 跨 Shell 提示符 · 显示 git/语言/目录信息 |
| zoxide | 智能 cd 替代 · 记忆访问频率 · 模糊跳转 |
| fzf | 模糊搜索器 · 历史命令/文件/目录交互选择 |
| Yazi | 终端文件管理器 · 三栏布局 · 图片/文件预览 |

> 推荐 Shell：zsh（默认配置）。fish 用户将文中 zsh 相关命令替换为 fish 即可，逻辑相同。

---

## 1. 安装

### 1.1 macOS

先安装 Homebrew（如已安装跳过）：

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

安装全部工具：

```bash
# 终端模拟器
brew install --cask ghostty

# 命令行工具
brew install starship zoxide fzf yazi

# 可选：更快的文件搜索后端（fzf 用）
brew install fd
```

验证安装：

```bash
ghostty --version
starship --version
zoxide --version
fzf --version
yazi --version
```

### 1.2 Linux · Ubuntu / Debian

```bash
# fzf
sudo apt install fzf fd-find

# starship
curl -sS https://starship.rs/install.sh | sh

# zoxide
curl -sSfL https://raw.githubusercontent.com/ajeetdsouza/zoxide/main/install.sh | sh

# yazi
cargo install yazi-fm yazi-cli

# Ghostty（从源码或官方包）
# 详见 https://ghostty.org/docs/install/binary
```

### 1.3 Linux · Arch

```bash
sudo pacman -S ghostty starship zoxide fzf fd yazi
```

---

## 2. Ghostty 配置

配置文件路径：`~/.config/ghostty/config`（修改后自动热重载，无需重启）

创建配置目录：

```bash
mkdir -p ~/.config/ghostty
```

写入以下配置到 `~/.config/ghostty/config`：

```ini
# ── 主题 ─────────────────────────────────────────────────────
theme = catppuccin-mocha

# ── 字体（需提前安装 JetBrains Mono）────────────────────────
font-family = JetBrains Mono
font-size = 14
font-feature = calt
font-feature = liga
font-thicken = true

# ── 透明度 ────────────────────────────────────────────────────
background-opacity = 0.95

# ── 窗口内边距（留白是优雅的关键）────────────────────────────
window-padding-x = 14
window-padding-y = 12
window-padding-balance = true
window-inherit-working-directory = true

# ── 光标 ─────────────────────────────────────────────────────
cursor-style = bar
cursor-style-blink = true

# ── Shell 集成（按你的 Shell 修改）────────────────────────────
shell-integration = zsh

# ── 滚动历史 ─────────────────────────────────────────────────
scrollback-limit = 10000

# ── macOS 专属（Linux 可删除这两行）────────────────────────────
macos-titlebar-style = hidden

# ── 快捷键 ────────────────────────────────────────────────────
keybind = cmd+t=new_tab
keybind = cmd+w=close_surface
keybind = cmd+d=new_split:right
keybind = cmd+shift+d=new_split:down
```

查看所有可用主题：

```bash
ghostty +list-themes
```

> 安装 JetBrains Mono 字体：https://www.jetbrains.com/lp/mono/（免费下载，安装后重启 Ghostty）

---

## 3. Starship 提示符配置

创建配置文件目录：

```bash
mkdir -p ~/.config
```

写入以下配置到 `~/.config/starship.toml`：

```toml
# 提示符格式：目录 → git → 语言 → 换行 → 输入符
format = """
$directory$git_branch$git_status$nodejs$python$rust$golang
$character"""

[directory]
style = "bold #89b4fa"
truncation_length = 3
truncate_to_repo = true

[git_branch]
symbol = " "
style = "bold #a6e3a1"

[git_status]
style = "bold #f38ba8"
conflicted = "! "
ahead = "up${count} "
behind = "dn${count} "
modified = "* "
untracked = "? "
staged = "v "

[character]
success_symbol = "[>](bold #a6e3a1)"
error_symbol = "[>](bold #f38ba8)"

[nodejs]
symbol = " "
style = "#a6e3a1"

[python]
symbol = " "
style = "#f9e2af"

[rust]
symbol = " "
style = "#f38ba8"

[golang]
symbol = " "
style = "#89dceb"

[cmd_duration]
min_time = 2000
style = "#f9e2af"
format = "took [$duration]($style) "
```

---

## 4. Yazi 文件管理器配置

Yazi 采用三栏布局（父目录 · 当前目录 · 预览），完全键盘操作，支持图片、PDF、视频缩略图实时预览。

创建配置目录：

```bash
mkdir -p ~/.config/yazi
```

写入以下配置到 `~/.config/yazi/yazi.toml`：

```toml
[manager]
# 显示隐藏文件
show_hidden = true
# 排序方式（按文件名字母序）
sort_by = "natural"
sort_sensitive = false
sort_reverse = false

[preview]
# 图片预览（Ghostty 支持 kitty 协议）
image_filter = "lanczos3"
image_quality = 75
tab_size = 2
max_width = 600
max_height = 900

[opener]
# 文件打开规则（按需修改编辑器）
edit = [
  { run = 'nvim "$@"', block = true, desc = "NeoVim" },
]
```

写入快捷键配置到 `~/.config/yazi/keymap.toml`：

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

在 `~/.zshrc` 中添加 shell wrapper（退出 Yazi 后自动跳到当前目录）：

```zsh
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

之后用 `y` 命令启动 Yazi，退出时会自动 `cd` 到你最后浏览的目录。

### Yazi 常用快捷键

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
| `z` | zoxide 跳转目录 |
| `.` | 显示/隐藏 隐藏文件 |
| `q` | 退出 |

> Yazi 图片预览需要终端支持 Kitty 图形协议，Ghostty 原生支持，开箱即用。

---

## 5. Shell 配置（~/.zshrc）

将以下内容追加到 `~/.zshrc` 末尾，**顺序不可颠倒**：

```zsh
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

# ── zoxide 初始化 ─────────────────────────────────────────────
eval "$(zoxide init zsh)"

# ── Yazi：退出后 cd 到最后所在目录 ────────────────────────────
function y() {
  local tmp="$(mktemp -t "yazi-cwd.XXXXXX")"
  yazi "$@" --cwd-file="$tmp"
  if cwd="$(cat -- "$tmp")" && [ -n "$cwd" ] && [ "$cwd" != "$PWD" ]; then
    cd -- "$cwd"
  fi
  rm -f -- "$tmp"
}

# ── Starship 初始化（必须放最后）──────────────────────────────
eval "$(starship init zsh)"
```

使配置立即生效：

```bash
source ~/.zshrc
```

---

## 6. 常用快捷键速查

### 6.1 fzf 快捷键

| 快捷键 | 功能 |
|--------|------|
| `Ctrl + R` | 模糊搜索历史命令（最常用，替代 ↑↑↑） |
| `Ctrl + T` | 模糊搜索文件，将路径插入命令行 |
| `Alt + C` | 模糊搜索子目录并直接跳转 |
| `** + Tab` | 触发补全，如 `vim **` + Tab 选择文件 |

### 6.2 zoxide 命令

| 命令 | 功能 |
|------|------|
| `z foo` | 跳到最常访问的含 foo 的目录 |
| `z foo bar` | 多词匹配，如 `z work api` |
| `zi` | 交互式选择目录（fzf 弹窗） |
| `z -` | 跳回上一个目录 |
| `zoxide query -l` | 查看所有已记录目录 |
| `zoxide edit` | 手动编辑目录数据库 |

### 6.3 Yazi 快捷键

| 快捷键 | 功能 |
|--------|------|
| `h / l` | 进入父目录 / 子目录 |
| `j / k` | 上下移动光标 |
| `Space` | 多选文件 |
| `y / d / p` | 复制 / 剪切 / 粘贴 |
| `D` | 删除文件 |
| `r` | 重命名 |
| `a` | 新建文件或目录 |
| `f` | fd 搜索文件 |
| `z` | zoxide 跳转目录 |
| `.` | 显示/隐藏 隐藏文件 |
| `q` | 退出 |

### 6.4 Ghostty 快捷键（macOS）

| 快捷键 | 功能 |
|--------|------|
| `Cmd + T` | 新建标签页 |
| `Cmd + W` | 关闭当前面板 |
| `Cmd + D` | 水平分屏 |
| `Cmd + Shift + D` | 垂直分屏 |
| `Cmd + N` | 新建窗口 |
| `Cmd + K` | 清屏 |
| `Cmd + +` / `Cmd + -` | 字体放大 / 缩小 |

---

## 7. 安装验证清单

- [ ] 打开 Ghostty，确认主题和字体正确显示
- [ ] 输入 `starship --version`，确认提示符带 git 信息和语言标识
- [ ] `cd` 到任意项目目录，再用 `z 项目名` 跳回，验证 zoxide 记忆
- [ ] 按 `Ctrl+R`，确认出现 fzf 历史搜索弹窗
- [ ] 按 `Ctrl+T`，确认出现文件模糊搜索弹窗
- [ ] 在 git 仓库内确认 Starship 显示分支名和修改状态
- [ ] 输入 `y` 启动 Yazi，确认三栏布局正常，图片可预览
- [ ] 在 Yazi 内按 `z`，确认 zoxide 跳转弹窗正常

---

## 8. 参考资料

- Ghostty 官网：https://ghostty.org
- Starship 官网：https://starship.rs
- zoxide GitHub：https://github.com/ajeetdsouza/zoxide
- fzf GitHub：https://github.com/junegunn/fzf
- Yazi 官网：https://yazi-rs.github.io
- Yazi GitHub：https://github.com/sxyazi/yazi
- JetBrains Mono 字体：https://www.jetbrains.com/lp/mono/
- Catppuccin 主题：https://github.com/catppuccin
