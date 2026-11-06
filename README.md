# Logitech G915 Diablo II: Resurrected Remap (Fedora KDE Wayland)

Per-window key remapping for **Diablo II: Resurrected** on **Fedora KDE Plasma Wayland** with a **Logitech G915** keyboard using `xremap-kde`:

- Maps `Caps Lock` (`KEY_CAPSLOCK`) to `]` (`KEY_RIGHTBRACE`) for **Show Items** inside Diablo II, avoiding thumb strain on `Left Alt`, bypassing the in-game `Caps Lock` restriction, and preventing accidental `Alt+Tab` / `Alt+F4` while holding Show Items.
- Keeps `Caps Lock` working normally outside the game (e.g. `grp:caps_toggle` layout switching in KDE).
- Automatically disables the Left Super key (`KEY_LEFTMETA: []`) while Diablo II is focused (software Game Mode).
- Works both wirelessly via the Lightspeed receiver (`046d:c541`) and wired (`046d:c33e`).

## Setup

### 1. Install `xremap-kde` from Copr

```bash
sudo dnf copr enable -y blakegardner/xremap
sudo dnf install -y xremap-kde
```

### 2. Grant non-root input access via `uaccess`

The rule must be numbered before `/usr/lib/udev/rules.d/73-seat-late.rules` so `systemd-logind` applies the ACL immediately without logging out:

```bash
sudo cp 70-xremap-input.rules /etc/udev/rules.d/70-xremap-input.rules
sudo udevadm control --reload-rules && sudo udevadm trigger --subsystem-match=input
```

### 3. Install configuration and user service

```bash
mkdir -p ~/.config/xremap ~/.config/systemd/user
cp config.yml ~/.config/xremap/config.yml
cp xremap.service ~/.config/systemd/user/xremap.service
systemctl --user daemon-reload
systemctl --user enable --now xremap.service
```

### 4. Bind in Diablo II: Resurrected

Open **Options -> Controls** in Diablo II: Resurrected, select **Show Items**, and press **Caps Lock** (it will bind as `]`).
