# verify-code 🔐 → 📅

**Auto-copy email verification codes to your clipboard. Extract dates/times and add them to Google Calendar.** Supports **Gmail, Outlook, and Hotmail** on **Linux, Windows, and macOS**.

No more hunting through your inbox for that 2FA code. Verify-code watches your inbox (via IMAP), extracts verification codes, and copies them to your clipboard — instantly. It also finds dates, times, and events in your emails (flights, appointments, reservations, meetings) and adds them to your Google Calendar.

## One-Liner Install

```bash
# Linux
curl -fL https://raw.githubusercontent.com/aakashjabraham-hue/verify-code/main/verify-code -o ~/.local/bin/verify-code && chmod +x ~/.local/bin/verify-code && ~/.local/bin/verify-code install

# The setup wizard will launch automatically. Just follow the prompts!
```

## Quick Start

```bash
# Interactive setup wizard (fun! 🎉)
verify-code setup

# One-shot: copy the latest code
verify-code

# Scan inbox for events with dates/times
verify-code scan

# Run the daemon 24/7 (with calendar)
verify-code daemon --calendar

# Keep it up to date
verify-code update
```

## Features

### 🎯 Smart Code Extraction
- Cascading regex with confidence scoring — explicit OTP labels → grouped 3+3 → alphanumeric → bare digits
- **80+ known senders** whitelist (GitHub, Google, AWS, Microsoft, Discord, banking, gaming, etc.)
- Marketing email false-positive guard
- Alphanumeric 2FA support (Discord, Steam, etc.)

### 📅 Date/Time → Calendar
- Extracts events from email bodies: flights, appointments, reservations, meetings, deliveries
- Multiple date format support: "June 15, 2026 at 2:30 PM", "2026-08-20", "8/25/2026"
- **Google Calendar integration** — 1-click OAuth, auto-adds events to your calendar
- Configurable confidence threshold

### ✉️ Multi-Provider Support
- **Gmail** — IMAP via `imap.gmail.com`
- **Outlook** — IMAP via `outlook.office365.com`
- **Hotmail** — IMAP via `outlook.office365.com`
- App Password required (not your regular password)

### 🔒 Secure Mode
- Auto-erases verification codes from clipboard after a configurable timeout
- Prevents codes from lingering in your clipboard
- Enable during setup or via `--secure --secure-timeout 15` on the daemon

### ⚡ Instant Delivery
- IMAP IDLE push notifications — no polling
- Falls back to smart polling automatically

### 🔒 Privacy & Safety
- BODY.PEEK — emails are **never** marked as read
- CRC-validated atomic state — crashes never corrupt tracking
- Dedup — same code never copied twice, same event never added twice

## All Commands

| Command | What it does |
|---------|-------------|
| `verify-code setup` | Interactive wizard: provider + Calendar + clipboard + secure mode |
| `verify-code` | One-shot: find & copy newest code |
| `verify-code scan` | Scan inbox for events with dates/times (no copy) |
| `verify-code daemon` | Background: watches inbox, auto-copies, auto-adds events |
| `verify-code install` | Download latest + install to PATH + auto-start service |
| `verify-code update` | Check for updates; only downloads if newer version exists |
| `verify-code uninstall` | Remove everything (script, config, data, service) |
| `verify-code auth` | (Re-)authenticate Google Calendar |
| `verify-code version` | Show the installed version |
| `verify-code daemon --secure --secure-timeout 15` | Daemon with auto-erase after 15s |

## Options

```
--minutes MIN           Lookback window in minutes (default: 10)
--interval SEC          Daemon poll/IDLE timeout interval (default: 15)
--known-senders         Only extract from trusted senders (80+ known)
--no-idle               Force polling even if server supports IMAP IDLE
--calendar              Enable Google Calendar integration (daemon mode)
--cal-threshold N       Minimum confidence % for calendar events (default: 50)
--dry-run               Show what would be done, no clipboard or calendar action
--secure                Enable secure mode (auto-erase clipboard after timeout)
--secure-timeout N      Seconds before auto-erasing clipboard (default: 30)
--version               Show version
```

## Requirements

- Python 3.8+
- Gmail, Outlook, or Hotmail account with [App Password](https://support.google.com/accounts/answer/185833)
- **Linux:** `wl-clipboard` (Wayland) or `xclip` (X11)
- **Windows:** PowerShell (Set-Clipboard built-in)
- **macOS:** `pbcopy` (built-in)
- (Optional) `google-auth-oauthlib` + `google-api-python-client` + `python-dateutil` for Calendar

## Cross-Platform Support

| Platform | Clipboard | Notifications | Auto-start |
|----------|-----------|---------------|------------|
| Linux | wl-copy / xclip | notify-send | systemd |
| Windows | PowerShell Set-Clipboard | PowerShell Toast | Task Scheduler (manual) |
| macOS | pbcopy | — | — |

## Safety

- BODY.PEEK — messages are NEVER marked as read
- Credentials in `~/.config/verify-code/` (chmod 600)
- Atomic state writes with fsync + CRC — no corruption survival
- Dedup tracking prevents re-copying codes or re-adding events
- **Secure Mode** — auto-erases clipboard after configurable timeout
- Open source — you can read every line (it's one self-contained script!)

## License

MIT