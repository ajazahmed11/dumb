# Contributing to DUMB

Thank you for wanting to make DUMB smarter! 

DUMB was born from the realization that Linux backups and distro migrations shouldn't require manual labor, 2FA re-authentications, or gigabytes of wasted cloud storage. We welcome contributions from anyone, regardless of experience.

---

## 🎯 Where We Need Help

### 1. Browser Presets (`presets/browsers/`)
Firefox is currently supported with zero-bloat session preservation (`cookies.sqlite` kept, `cache2/` stripped).
We would love PRs adding exclusion rules for:
* **Chromium / Google Chrome**
* **Brave Browser**
* **Vivaldi**
* **Arc / Zen Browser**

### 2. Transport Engine Adapters (`engines/`)
Currently, DUMB uses `rclone` for cloud storage. We want to support:
* `rsync` — for ultra-fast local external SSD and NAS migrations.
* `restic` / `borg` — for encrypted local snapshots.

### 3. Hardware Blueprints (`presets/hardware/`)
Laptop and desktop thermal/fan recipes:
* ThinkPad (`thinkfan` / `tlp`)
* Framework Laptop power profiles
* Dell XPS thermal governors

---

## 🛠️ Development & Testing

1. Fork the repo and clone your branch:
   ```bash
   git clone https://github.com/<your-username>/dumb.git
   cd dumb
   ```
2. Test changes locally using `--dry-run`:
   ```bash
   ./bin/dumb check
   ```
3. Commit with clean, descriptive messages:
   ```bash
   git commit -m "feat(browser): add brave session preservation preset"
   ```
4. Open a Pull Request!

---

## 💡 Code of Conduct

Be kind, patient, and respectful. We've all felt dumb at 6 AM fighting system bugs—this project exists to lift each other up.
