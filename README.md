# verify-code 🔐 → 🔗 → 📅 → 💳

**Auto-copy email verification codes AND magic links to your clipboard. Extract dates/times and add them to Google Calendar. Track subscriptions & get renewal/trial warnings.** Supports **Gmail, Outlook, and Hotmail** on **Linux, Windows, and macOS**.

No more hunting through your inbox for that 2FA code or magic link. Verify-code watches your inbox (via IMAP), extracts verification codes or URLs, and copies them to your clipboard — instantly. It also tracks your subscriptions — detecting plans, prices, renewals, and free trial expirations from 80+ services — and finds dates/times in your emails (flights, appointments, reservations) to add to your Google Calendar.

## One-Liner Install

```bash
# Linux / macOS
curl -fL https://raw.githubusercontent.com/aakashjabraham-hue/verify-code/main/verify-code -o ~/.local/bin/verify-code && chmod +x ~/.local/bin/verify-code && ~/.local/bin/verify-code install

# The setup wizard will launch automatically. Just follow the prompts!
```

```powershell
# Windows (run in PowerShell)
powershell -Command "iwr -Uri 'https://raw.githubusercontent.com/aakashjabraham-hue/verify-code/main/verify-code' -OutFile \"$env:APPDATA\verify-code\verify-code.py\"; python \"$env:APPDATA\verify-code\verify-code.py\" install"
```

## Quick Start

```bash
# Interactive setup wizard
verify-code setup

# One-shot: copy the latest code or magic link
verify-code

# Scan inbox for events, codes, and subscriptions
verify-code scan

# View tracked subscriptions (with warnings & spending totals)
verify-code subscriptions

# Run the 24/7 daemon (auto-copies codes/links, tracks subs & calendar)
verify-code daemon

# Keep it up to date
verify-code update

# View copied code/link history
verify-code history

# Launch the web dashboard (subscriptions, codes, events)
verify-code dashboard
```

## Features

### 🎯 Smart Code & Magic Link Extraction
- Cascading regex with confidence scoring — explicit OTP labels → grouped 3+3 → alphanumeric → bare digits
- **Magic Link Support:** If no code is found, safely extracts verification URLs from HTML emails and copies them to your clipboard.
- **Phishing Protection:** Blacklists "Unsubscribe" or social media links. Unknown senders require very strong context words (verify, confirm, sign in) for links to be copied.
- **80+ known senders** whitelist (GitHub, Google, AWS, Microsoft, Discord, banking, gaming, etc.)
- Alphanumeric 2FA support (Discord, Steam, etc.)

### 💳 Subscription Tracker
- **Auto-detects subscriptions** from your inbox — Netflix, Spotify, Apple, Adobe, GitHub, Discord, and 80+ services
- **Automatic Daemon Tracking:** The daemon now tracks subscriptions automatically in the background without needing extra flags!
- **Parses receipts & renewal notices** — extracts plan name, price, billing cycle, and next renewal date
- **Free trial warnings** — alerts you before a trial converts to paid
- **Renewal reminders** — warns you N days before a subscription renews
- **Monthly/yearly totals** — shows your total subscription spend at a glance
- `verify-code subscriptions` — view all tracked subscriptions in a table

### 📅 Date/Time → Calendar
- Extracts events from email bodies: flights, appointments, reservations, meetings, deliveries
- Multiple date format support: "June 15, 2026 at 2:30 PM", "2026-08-20", "8/25/2026"
- **Google Calendar integration** — 1-click OAuth, auto-adds events to your calendar
- The daemon automatically processes and adds calendar events if authenticated

### ✉️ Multi-Provider Support
- **Gmail** — IMAP via `imap.gmail.com`
- **Outlook** — IMAP via `outlook.office365.com`
- **Hotmail** — IMAP via `outlook.office365.com`
- App Password required (not your regular password)

### 🔒 Secure Mode
- Auto-erases verification codes/links from clipboard after a configurable timeout
- Prevents sensitive data from lingering in your clipboard
- Enable during setup or via `--secure --secure-timeout 15` on the daemon

### 📋 Code & Link History
- `verify-code history` shows the last 10 copied codes/links with timestamps and sender info
- Full history is logged to `~/.local/share/verify-code/log.txt` (Linux/macOS) or `%APPDATA%\verify-code\data\log.txt` (Windows)
- Web dashboard displays a beautiful ledger of all copied codes and extracted events

### ⚡ Robust Delivery
- Smart polling mode for maximum stability across all email providers
- Fixed Wayland/X11 clipboard handling — properly backgrounding `wl-copy`/`xclip` to prevent daemon lockups

### 🔒 Privacy & Safety
- BODY.PEEK — emails are **never** marked as read
- CRC-validated atomic state — crashes never corrupt tracking
- Dedup — same code/link never copied twice, same event never added twice

## All Commands

| Command | What it does |
|---------|-------------|
| `verify-code setup` | Interactive wizard: provider + Calendar + clipboard + secure mode |
| `verify-code` | One-shot: find & copy newest code or magic link |
| `verify-code scan` | Scan inbox for events, codes, and subscriptions (no copy) |
| `verify-code subscriptions` | View all tracked subscriptions with warnings and spending totals |
| `verify-code history` | Show the last 10 copied codes/links |
| `verify-code daemon` | Background: polls inbox, auto-copies codes/links, auto-tracks subs & calendar |
| `verify-code install` | Download latest + install to PATH + auto-start service |
| `verify-code update` | Check for updates; only downloads if newer version exists |
| `verify-code uninstall` | Remove everything (script, config, data, service) |
| `verify-code auth` | (Re-)authenticate Google Calendar |
| `verify-code version` | Show the installed version |
| `verify-code dashboard` | Launch a local web dashboard in your browser |
| `verify-code daemon --secure --secure-timeout 15` | Daemon with auto-erase after 15s |

## Options

```
--minutes MIN           Lookback window in minutes (default: 10)
--interval SEC          Daemon poll interval in seconds (default: 15)
--known-senders         Only extract from trusted senders (80+ known)
--idle                  Enable IMAP IDLE for instant notification (experimental)
--calendar              Enable Google Calendar integration (one-shot/scan mode)
--cal-threshold N       Minimum confidence % for calendar events (default: 50)
--subscriptions         Enable subscription tracking (one-shot/scan mode)
--warn-days N           Days before renewal/trial-end to warn (default: 3)
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
| macOS | pbcopy | osascript notification | — |

## Safety

- BODY.PEEK — messages are NEVER marked as read
- Credentials in `~/.config/verify-code/` (chmod 600)
- Atomic state writes with fsync + CRC — no corruption survival
- Dedup tracking prevents re-copying codes/links or re-adding events
- **Secure Mode** — auto-erases clipboard after configurable timeout
- Open source — you can read every line (it's one self-contained script!)

## License

MIT
