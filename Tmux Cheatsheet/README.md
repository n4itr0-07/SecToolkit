# 📦 What is Tmux?

**Tmux (Terminal Multiplexer)** allows you to manage multiple terminal sessions from a single window. It’s like a tiling window manager *inside your terminal*. Ideal for sysadmins, developers, hackers, CTF players, and remote workers.

---

## 💡 Benefits of Using Tmux

| Feature | Description |
|--------|-------------|
| **Session Persistence** | Detach from a session and come back later — it keeps running in the background. |
| **Multi-pane** | Split terminal into multiple windows and panes. |
| **Remote Safe** | Perfect for SSH — even if your connection drops, tmux keeps everything running. |
| **Customizable** | Easily tweak key bindings, status bar, colors, and behavior. |
| **Scriptable** | Automate layouts and workflows using `.tmux.conf` or scripting tools like `tmuxinator`. |
| **Mouse Support** | Can interact with panes/windows using the mouse. |
| **Clipboard Integration** | Works well with system clipboard via plugins or external tools. |
| **Lightweight** | No GUI overhead, fast, minimal memory usage. |

---

# 🧩 How Tmux Works: Mental Model

```

Session → Windows → Panes

• Session = entire workspace
• Window = tab inside session
• Pane = split areas inside a window

````
```yaml
+------------------------------------------+
| Session 1                                |
| +---------------------+----------------+ |
| | Window 1 (Attack)   | Window 2 (Tools)| |
| |                     |                | |
| | +-------+---------+ | +--------------+ |
| | | Pane A | Pane B | | |   Pane D     | |
| | |       |         | | |              | |
| | +-------+---------+ | +--------------+ |
| | | Pane C          | |                | |
| | |                 | |                | |
| | +-----------------+ |                | |
| +---------------------+----------------+ |
+------------------------------------------+
```
---

### **Basic Installation (Very brief):** You could add a tiny section on how to install Tmux on common OS. It's usually a one-liner.

- `sudo apt install tmux` (Debian/Ubuntu)
- `sudo pacman -S tmux` (Arch)
- `brew install tmux` (macOS)
- `choco install tmux` (Windows via Chocolatey)

---

# ⚙️ Tmux Configuration (`~/.tmux.conf`)

Location: `~/.tmux.conf`

This file runs when tmux starts. You can use it to customize key bindings, behavior, appearance, and plugins.

---

## 🔧 Common Config Options

| Setting | Description |
|--------|-------------|
| `set -g prefix C-a` | Changes prefix from `Ctrl+b` to `Ctrl+a`. |
| `unbind C-b` | Unbind default prefix. |
| `bind | split-window -h` | Horizontal split with `|` key. |
| `bind - split-window -v` | Vertical split with `-` key. |
| `set -g mouse on` | Enable mouse mode. |
| `setw -g mode-keys vi` | Use `vi` keys in copy mode. |
| `set -g history-limit 10000` | Set scrollback buffer size. |
| `set -g status-bg colour235` | Set background color of status bar. |
| `set -g status-fg colour136` | Set foreground color of status bar. |
| `set-option -g allow-rename off` | Prevents auto-renaming windows. |

---

## 🧪 Sample: Minimal Clean `.tmux.conf`

```tmux
# Change prefix to Ctrl+a
unbind C-b
set -g prefix C-a
bind C-a send-prefix

# Enable mouse
set -g mouse on

# Better scrollback
set -g history-limit 10000

# Status bar customization
set -g status-bg black
set -g status-fg white

# Use vi keys in copy mode
setw -g mode-keys vi

# Easy pane splitting
bind | split-window -h
bind - split-window -v

# Reload config
bind r source-file ~/.tmux.conf \; display-message "Config reloaded!"
````

---

## 📋 Clipboard Integration (Linux / WSL)

To copy text to system clipboard from Tmux:

```bash
# Add to .tmux.conf (Linux with xclip)
bind-key -T copy-mode-vi y send -X copy-pipe-and-cancel "xclip -selection clipboard -i -r"

# For WSL:
bind-key -T copy-mode-vi y send -X copy-pipe-and-cancel "clip.exe"
```

---

## 🔌 Plugin Manager (TPM)

**Tmux Plugin Manager** helps you manage plugins like Vim’s Vundle.

### ✅ Install TPM

```bash
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```

### 🛠 Add to `.tmux.conf`

```tmux
# List of plugins
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'tmux-plugins/tmux-sensible'
set -g @plugin 'tmux-plugins/tmux-resurrect'

# Initialize TPM
run '~/.tmux/plugins/tpm/tpm'
```

Use `prefix + I` to install plugins.

---

## 🚀 Pro Tips

* Use `tmux-resurrect` to auto-save/restore sessions
* Combine with `oh-my-zsh` + `powerlevel10k` for rich UX
* Use Tmux in *floating terminals* inside Tiling WMs for maximum productivity

---

> 💭 **Summary**: Tmux is your best companion for multitasking inside terminal. Whether you’re a developer, hacker, or sysadmin — it boosts productivity, provides robustness over SSH, and is fully customizable.





# Tmux Complete Cheatsheet

| Command | Description |
|--------|-------------|
| `tmux` | Start a new tmux session. |
| `tmux new -s <name>` | Start a new session with a name. |
| `tmux a` or `tmux attach` | Attach to the most recent session. |
| `tmux a -t <name>` | Attach to a specific session. |
| `tmux ls` or `tmux list-sessions` | List all sessions. |
| `tmux kill-session -t <name>` | Kill a session by name. |
| `tmux kill-server` | Kill all tmux sessions. |

---

## 🧠 Prefix Key

| Command | Description |
|--------|-------------|
| `Ctrl+b` | Default tmux prefix key (used before issuing commands). |
| `Ctrl+b ?` | Show all key bindings. |
| `Ctrl+b d` | Detach from current session. |

---

## 🔁 Session Management

| Command | Description |
|--------|-------------|
| `tmux rename-session -t <old> <new>` | Rename a session. |
| `tmux switch -t <session>` | Switch between sessions. |
| `Ctrl+b s` | List and switch sessions. |

---

## 🪟 Window Management

| Command | Description |
|--------|-------------|
| `Ctrl+b c` | Create a new window. |
| `Ctrl+b ,` | Rename current window. |
| `Ctrl+b n` | Move to next window. |
| `Ctrl+b p` | Move to previous window. |
| `Ctrl+b w` | List windows. |
| `Ctrl+b &` | Kill current window. |
| `Ctrl+b <number>` | Switch to specific window. |

---

## 🪟 Pane Management

| Command | Description |
|--------|-------------|
| `Ctrl+b "` | Split pane horizontally. |
| `Ctrl+b %` | Split pane vertically. |
| `Ctrl+b o` | Go to next pane. |
| `Ctrl+b q` | Show pane numbers. |
| `Ctrl+b ;` | Go to the last active pane. |
| `Ctrl+b x` | Close current pane. |
| `Ctrl+b z` | Zoom in/out current pane. |
| `Ctrl+b !` | Break pane into new window. |
| `Ctrl+b Ctrl+<Arrow>` | Resize pane. |

---

## 🔁 Pane Navigation

| Command | Description |
|--------|-------------|
| `Ctrl+b ↑` | Move to the pane above. |
| `Ctrl+b ↓` | Move to the pane below. |
| `Ctrl+b ←` | Move to the left pane. |
| `Ctrl+b →` | Move to the right pane. |

---

## 📝 Copy Mode (Scrolling + Copy)

| Command | Description |
|--------|-------------|
| `Ctrl+b [` | Enter copy mode. |
| `q` | Exit copy mode. |
| `Space` | Start selection in copy mode. |
| `Enter` | Copy selected text. |
| `Ctrl+b ]` | Paste copied text. |
| `Ctrl+b PgUp` | Scroll up inside a pane. |

---

## 🧰 Miscellaneous

| Command | Description |
|--------|-------------|
| `Ctrl+b t` | Show current time. |
| `Ctrl+b r` | Reload config file. |
| `Ctrl+b :` | Enter command prompt. |
| `Ctrl+b ?` | Show all key bindings. |

---

## 🛠️ Useful Tmux Commands

| Command | Description |
|--------|-------------|
| `tmux source-file ~/.tmux.conf` | Reload tmux config. |
| `tmux list-keys` | Show all key bindings. |
| `tmux list-commands` | Show all tmux commands. |

---

## ⚙️ Sample ~/.tmux.conf Snippet

```bash
# Remap prefix from Ctrl+b to Ctrl+a
unbind C-b
set -g prefix C-a
bind C-a send-prefix

# Enable mouse
set -g mouse on

# Set default terminal mode
set -g default-terminal "screen-256color"

# Split shortcuts
bind | split-window -h
bind - split-window -v
```

---

