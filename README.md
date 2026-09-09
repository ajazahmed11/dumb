# 🥱 DUMB — DUMB's Universal Migration & Backup

> **A zero-bloat, distro-agnostic workstation migration framework — so you can finally get some sleep.**

---

## 💡 The Story Behind DUMB

At 6:00 AM after an all-nighter recovering from a fresh Linux reinstall, searching for lost lecture slides, reconfiguring laptop fan curves, and dreading the thought of re-authenticating 20 different websites with 2FA, the realization hit:

> *"Why am I doing this by hand? I feel dumb."*

Linus Torvalds famously named **Git** after British slang for an annoying person. Following that proud tradition of hacker self-deprecation, **DUMB** is a recursive acronym (**D**UMB's **U**niversal **M**igration & **B**ackup) created so you never have to feel dumb when migrating or backing up your Linux workstation again.

---

## 🎯 The Philosophy: Fresh Setup > Blind Restoration

Traditional full-system backups (Timeshift, Snapper) fail when hopping distros (e.g., Nobara &rarr; Arch &rarr; Fedora &rarr; Debian) because blindly restoring `/etc` or root system files breaks display servers, package managers, and hardware drivers.

Meanwhile, dotfile managers (Chezmoi, GNU Stow) are great for a few text files (`.bashrc`), but fall apart on browser session databases, AI memory stores, and local binaries.

**DUMB solves this through three core principles:**

1. **Taxonomy Over Monoliths**: Your workstation is not an opaque blob. It is made of distinct layers (personal data, app configs, browser sessions, dotfiles, AI memories, and hardware recipes).
2. **Never Backup Disposable Garbage**: Electron, Chromium, and Python caches waste gigabytes, burn cloud bandwidth, and hit API rate limits. Prune the trash automatically.
3. **Preserve Logins, Skip the 2FA Hell**: Save persistent session tokens (`cookies.sqlite` + `storage/default/`) while stripping hundreds of megabytes of disk cache (`cache2/`). When you restore, your browser opens with ChatGPT, Claude, Google, and GitHub already logged in.
4. **Move-on-Write Safety**: Standard `sync` tools can accidentally delete remote files if a local folder is wiped. DUMB uses Move-on-Write (`--backup-dir`) to archive any modified or deleted file into timestamped `Old-Versions/YYYY-MM-DD/` directories.

---

## 🏗️ The 3 Decoupled Pillars

DUMB is designed to be completely modular, not locked to any single tool or cloud:

```text
┌──────────────────────────────────────────────────────────┐
│ 1. THE TAXONOMY (The Intelligence)                      │
│    Intelligent knowledge of Linux workstations:          │
│    • Strips cache2/ but preserves cookies.sqlite         │
│    • Prunes node_modules/, .venv/, and __pycache__/      │
│    • Isolates AI agent brains (Antigravity, Claude, etc.)│
│    • Separates hardware fan curves from user configs     │
└────────────────────────────┬─────────────────────────────┘
                             ▼
┌──────────────────────────────────────────────────────────┐
│ 2. THE ENGINE (Pluggable Transport)                      │
│    • rclone    (Multi-cloud: GDrive, OneDrive, Proton, S3)│
│    • rsync     (Ultra-fast local USB drives, SSH, NAS)   │
│    • restic    (Planned: Deduplicated snapshots)         │
└────────────────────────────┬─────────────────────────────┘
                             ▼
┌──────────────────────────────────────────────────────────┐
│ 3. THE TARGET (Where It Lives)                           │
│    • External USB SSD / HDD                              │
│    • Home NAS (NFS / Samba / TrueNAS)                    │
│    • Cloud (Google Drive, OneDrive, Nextcloud, S3, B2)   │
└──────────────────────────────────────────────────────────┘
```

---

## 📦 The 6 Layers of a Clean Workstation

```text
DUMB Vault
├── 01-Personal-Files/        # Documents, Projects, Pictures (clean source code)
├── 02-dot-config/            # App configurations (stripped of GPUCache & sockets)
├── 03-Browser-Vault/         # Active logins & session cookies (zero 2FA friction)
├── 04-Home-Dotfiles/         # Shell configs, gitconfig, and custom local scripts
├── 05-AI-Agent-Vault/        # AI memory databases, conversation logs, and brain contexts
├── 06-Hardware-Blueprint/    # Fan curves, CPU governor/boost units, and fonts
└── Old-Versions/             # Timestamped rollback archive (Move-on-Write)
```

| Layer | Source | What's Preserved | What's Stripped |
| :--- | :--- | :--- | :--- |
| **01: Personal** | `~/Documents`, `~/Projects`, `~/Pictures` | Code, notes, academic archives | `.venv/`, `node_modules/`, `target/`, `build/`, `__pycache__/` |
| **02: Config** | `~/.config` | Terminal, editor, desktop settings | `*Cache*`, `*GPUCache*`, `*blob_storage*`, `*.lock`, `*.sock` |
| **03: Browser** | Firefox / Chromium profiles | `cookies.sqlite`, `storage/`, active sessions | `cache2/`, `jumpListCache/`, crash dumps |
| **04: Dotfiles** | `~/.bashrc`, `~/.gitconfig`, `~/.local/bin` | Declarative `DOTFILES` array (`.bashrc`, `.zshrc`, `.tmux.conf`), custom scripts | Heavy precompiled binaries (`rclone`, `agy`) |
| **05: AI Agents** | `~/.gemini`, `~/.claude`, `~/.codex` | Brain contexts, memories, session logs | Ephemeral daemon sockets, runtime caches, API tokens |
| **06: Hardware** | Blueprint recipes & fonts | Fan curves (`asusd`), CPU boost service, fonts | Machine-locked system state |

---

## 🚀 Quick Start

### 1. Installation

```bash
git clone https://github.com/ajazahmed11/dumb.git ~/.local/share/dumb
ln -s ~/.local/share/dumb/bin/dumb ~/.local/bin/dumb
```

### 2. Basic Usage

```bash
# 1. Configure DUMB for your machine (interactive 4-step wizard)
dumb init

# 2. Calculate how much disposable cache bloat DUMB will strip
dumb inspect

# 3. Dry-run inspection (see what transfers without touching anything)
dumb check

# 4. Run the clean 6-layer backup (Cloud via rclone, or USB/Nextcloud via rsync)
dumb backup

# Target & Selective Examples:
dumb backup -r /run/media/$USER/PENDRIVE/DUMB-Vault  # USB Pendrive (offline, native rsync)
dumb backup -r $HOME/Nextcloud/DUMB-Vault            # Nextcloud / Dropbox sync folder
dumb backup -r gdrive:DUMB-Vault                     # Google Drive (cloud rclone)
dumb backup -l 1,4                                   # Fast selective backup (Personal + Dotfiles)
dumb backup -n                                       # Safe backup dry-run preview

# 5. Show exact WHAT, WHERE, and HOW breakdown
dumb explain

# 6. Create a single-file portable archive on a USB Pendrive
dumb bundle /run/media/$USER/PENDRIVE/my-workstation.tar.zst
dumb unbundle /run/media/$USER/PENDRIVE/my-workstation.tar.zst

# 7. Restore onto a fresh OS (interactive checklist or flag-driven)
dumb restore
dumb restore -n --layers 1,3,4   # Safe preview mode (-n / --dry-run)
dumb restore --layers 1,3,4      # Restore Personal, Browser Logins, and Dotfiles
```

---

## 🔒 Security Advisory: Layer 3 & Browser Sessions

* **The Design**: Layer 3 (`03-Browser-Vault`) preserves session cookies (`cookies.sqlite`) and local storage by design so that you don't have to re-authenticate 2FA for 20+ accounts after a fresh install.
* **Best Practice for Cloud Storage**: Because session tokens are sensitive, if you are backing up to a shared or public cloud remote, we strongly recommend pointing DUMB to an **`rclone crypt`** remote (client-side encrypted bucket) or keeping Layer 3 on an **offline USB flash drive / pendrive**.

---

## 🤝 Community Contributions: Help Make DUMB Smarter!

DUMB is open-source and community-driven. You don't have to use Google Drive or Firefox—we want DUMB to support every workflow!

### Ways to Contribute:
- [ ] **Browser Presets**: Add profile pruning rules for Brave, Chrome, Vivaldi, Arc, and Zen.
- [x] **Engine Adapters**: Auto-routing dual engine: native `rsync` (USB/local) + `rclone` (cloud).
- [ ] **Hardware Recipes**: Add fan and thermal profiles for ThinkPad (`thinkfan`), Framework, and Dell laptops.
- [ ] **Desktop Presets**: Add window manager configurations for Hyprland, Sway, GNOME, and i3.
- [ ] **Interactive TUI**: Build a clean terminal UI to toggle layers on and off.

Check out [CONTRIBUTING.md](CONTRIBUTING.md) to get started!

---

## 📄 License

MIT License © 2026 Ajaz Ahmed. Free to use, modify, and distribute. Don't feel dumb, enjoy your sleep!
