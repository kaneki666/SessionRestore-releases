# Session Restore

Reopens your apps after a power cut. A small native app for people who lose power often, available for **Windows** and **Ubuntu / Linux**.

Latest release: [v1.0.1](https://github.com/kaneki666/SessionRestore-releases/releases/latest)

## Install

### Windows (10 / 11)

1. Download `SessionRestore.exe` from the [latest release](https://github.com/kaneki666/SessionRestore-releases/releases/latest).
2. Put it somewhere permanent, for example `C:\Tools\SessionRestore\`.
3. Double-click it. The first time, it shows a **"Before you start"** screen. Nothing is read until you click **Allow and start**.
4. Say **Yes** to starting with Windows, so it is always running.

Or with winget (once the package is approved):

```
winget install MohammadSadman.SessionRestore
```

Windows may show a SmartScreen warning because the app is not code-signed yet. Click **More info**, then **Run anyway**. You can check the download against the SHA256 listed in the release notes:

```
Get-FileHash .\SessionRestore.exe -Algorithm SHA256
```

### Ubuntu / Linux (24.04 or newer, 64-bit)

1. Download `sessionrestore_1.0.1_amd64.deb` from the [latest release](https://github.com/kaneki666/SessionRestore-releases/releases/latest).
2. Install it:

```
sudo apt install ./sessionrestore_1.0.1_amd64.deb
```

3. Open **Session Restore** from the app menu, or run `sessionrestore` in a terminal. The first time, it shows what it can see and asks you to allow it.

Commands:

```
sessionrestore             open the dashboard (starts the service if needed)
sessionrestore status      what it's doing
sessionrestore snapshot    save what's open now
sessionrestore restore     reopen everything from the last snapshot
sessionrestore stop        stop the service
```

The Linux version runs in the background and has a dashboard that opens in your browser. The dashboard is served only to you, on this PC (127.0.0.1, protected by a secret key).

To uninstall: turn off **Settings > Start when I log in** and quit from the dashboard, then `sudo apt remove sessionrestore`. Optional: `rm -rf ~/.local/share/sessionrestore ~/.config/autostart/sessionrestore.desktop`.

## What it does

**Reopens your session after a power cut**
- Windows: VS Code, Docker Desktop and its containers, terminals (PowerShell, cmd) in their folders with Claude Code resumed, File Explorer folders at the same position, and any app you add.
- Linux: VS Code, terminals in the same folder (GNOME Terminal, Konsole, kitty, Alacritty and more) with Claude Code resumed, Docker containers, and any app you add. Browsers come back with your last tabs.

**Power cut protection.** With a UPS connected by USB or a laptop battery, it saves your session when the power goes and hibernates the PC before the battery runs out. You get a countdown and a Cancel button. It keeps a history of every power cut.

**Git safety.** Shows which projects have uncommitted or unpushed work. Every 10 minutes it backs up uncommitted work to a hidden ref (`refs/session-restore/backup`) without touching your branches, staging area or stash, and reminds you in the evening.

**Ports.** Shows which program uses which port, and can end it.

**Security.** Checks whether other accounts on the PC can read your SSH keys, cloud and Git logins, and browser data, and fixes it with one click. On Windows it also clears copied API keys, tokens and passwords from the clipboard after 30 seconds.

## Permissions and privacy

- It shows what it reads, saves and can change **before it reads anything**, with a switch for each part. You can review it again any time from Settings.
- It reads which apps and windows are open, your terminals' working folders, your git projects, and (on Windows) the clipboard to catch secrets.
- It only changes things you allow: a startup entry, hibernating the PC, and folder permissions when you click **Make private**.
- It **never connects to the internet**.
- Its own files are private to you. Windows: `%APPDATA%\SessionRestore\`, encrypted with Windows DPAPI. Linux: `~/.local/share/sessionrestore`, readable only by you.
- Passwords and tokens found in command lines are never saved.

## Differences on Linux

- Window positions and file manager folders are not restored, because Wayland (Ubuntu's default) does not let apps see or move other apps' windows.
- Clipboard protection and auto-lock are not available for the same reason. Use Ubuntu's own Settings > Privacy > Screen Lock.
- Ubuntu turns hibernate off by default. You can choose "shut down safely instead" in the Power tab, or set up hibernation (needs a swap file at least as large as your RAM).

## What it can't do

It can't protect you from malware running as your own account, or from someone who knows your password. For a stolen PC, turn on BitLocker / device encryption.

## License

See [LICENSE](LICENSE).
