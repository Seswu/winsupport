# Reading Material: Windows 11 Core Orientation

## 1. Windows 11 Interface & Navigation

### Settings App vs. Control Panel
Windows 11 has two parallel configuration interfaces. The Settings app (`Win + I`) is the modern interface covering most common settings. Control Panel (`control` from Run dialog) is the legacy interface, still containing deeper configuration options not yet migrated. Microsoft is slowly moving everything to Settings, but as a supporter you need to know both exist and where settings live in each.

Key Settings categories:
- **System**: Display, sound, notifications, power, storage, about
- **Bluetooth & devices**: Printers, USB, mouse, keyboard
- **Network & internet**: Wi-Fi, Ethernet, proxy, advanced network settings
- **Personalization**: Themes, taskbar, start menu, colors
- **Apps**: Installed apps, default apps, startup apps
- **Accounts**: Your info, sign-in options, email & accounts, access work/school
- **Time & language**: Region, language, typing, date & time
- **Gaming**: Game bar, game mode, captures
- **Accessibility**: Display, audio, interaction, keyboard, mouse
- **Privacy & security**: Location, camera, microphone, Windows Security
- **Windows Update**: Check for updates, update history, advanced options

### Right-Click Context Menu
Windows 11 introduced a simplified context menu. Many advanced options are hidden behind "Show more options" or `Shift + right-click`. This is relevant for support when you need to "Run as administrator," access file properties, or use command-line tools from the context menu.

## 2. File Explorer & File System Structure

### Path Structure
```
C:\
├── Windows\           # OS files (like /usr, /etc combined)
├── Program Files\     # 64-bit applications
├── Program Files (x86)\  # 32-bit applications
├── Users\
│   └── <username>\
│       ├── Desktop\
│       ├── Documents\
│       ├── Downloads\
│       ├── AppData\           # Hidden by default
│       │   ├── Local\         # Per-user app data, caches (like ~/.local/share, ~/.cache)
│       │   ├── Roaming\       # Synced settings (no direct Linux equivalent)
│       │   └── LocalLow\      # Low-integrity app data (sandboxed apps)
│       └── OneDrive\          # If OneDrive Known Folder Move is enabled
└── ProgramData\       # Hidden, system-wide app data (like /etc for app configs)
```

### Special Folders (Environment Variables)
- `%USERPROFILE%` — C:\Users\<username>
- `%APPDATA%` — C:\Users\<username>\AppData\Roaming
- `%LOCALAPPDATA%` — C:\Users\<username>\AppData\Local
- `%PROGRAMDATA%` — C:\ProgramData
- `%WINDIR%` or `%SYSTEMROOT%` — C:\Windows
- `%TEMP%` — Current user's temp folder
- `%PROGRAMFILES%` — C:\Program Files

### Hidden Files and Extensions
By default, Windows hides:
- Hidden files/folders (those with the hidden attribute)
- Known file extensions (.txt, .docx, .pdf, etc.)
- Protected operating system files

Support tip: Always enable "Show file name extensions" and "Show hidden files" when troubleshooting. Users often don't realize `document.docx` is actually `document.docx.txt` or that a `.lnk` shortcut is not the actual file.

## 3. Task Manager

Access: `Ctrl + Shift + Esc` or right-click Start button > Task Manager

### Tabs:
- **Processes**: Running apps and background processes with CPU, memory, disk, network, GPU usage. Right-click > Go to details for PID. Right-click > Open file location to find the binary.
- **Performance**: Real-time graphs for CPU, memory, disk, network, GPU. Shows utilization, capacity, and hardware details.
- **App history**: Resource usage by app over time (useful for identifying long-running resource hogs).
- **Startup apps**: Programs that launch at login. Enable/disable to improve boot time. This is your equivalent of managing `~/.config/autostart/` or systemd user services.
- **Users**: Resource usage per logged-in user (useful on shared/RDS systems).
- **Details**: Full process list with PIDs, status, CPU, memory, description. Your equivalent of `ps aux`. Right-click > Set priority or Set affinity.
- **Services**: Lists Windows services with status. Click "Open Services" to get the full `services.msc` console.

### Key concepts:
- **Processes vs. Apps**: Apps are the user-facing applications. Processes include background services and system processes.
- **Background processes**: Apps running without a visible window (updaters, sync clients, services).
- **Windows processes**: Core OS processes. Do not kill these.
- **End task vs. End process tree**: End task kills the process; end process tree kills it and all child processes.

## 4. Event Viewer

Access: `eventvwr.msc` or search "Event Viewer"

### Key Log Locations:
- **Windows Logs**:
  - **Application**: App crashes, warnings, info from installed software
  - **Security**: Audit events, login attempts (if auditing enabled)
  - **System**: Driver events, service start/stop, hardware events, Windows Update
  - **Setup**: Installation and setup events
- **Applications and Services Logs**: Product-specific logs. Expand `Microsoft` to find logs for Windows Update, OneDrive, Office, etc.

### Event Levels:
- **Critical**: System unusable, data loss risk
- **Error**: Functionality broken, feature not working
- **Warning**: Something may become a problem
- **Information**: Normal operation (often too noisy for troubleshooting)
- **Verbose**: Detailed debug info (must be enabled explicitly)

### Filtering:
Right-click a log > Filter Current Log. Filter by level, event ID, source, or date range. This is essential for cutting through noise when troubleshooting.

## 5. Windows Services

Access: `services.msc` or Task Manager > Services tab > Open Services

Services are background processes managed by the Service Control Manager (SCM). They are your closest equivalent to systemd units.

### Service Properties:
- **Status**: Running, Stopped, Starting, Stopping
- **Startup type**:
  - **Automatic**: Starts at boot
  - **Automatic (Delayed Start)**: Starts after boot, reduces boot-time contention
  - **Manual**: Starts on demand (when an app requests it)
  - **Disabled**: Cannot start
- **Log On As**: Which account the service runs under (Local System, Network Service, specific user)
- **Dependencies**: Other services this service requires, or services that require this one

### Common Services to Know:
| Service | Name | Purpose |
|---|---|---|
| Windows Update | `wuauserv` | Downloads and installs updates |
| Print Spooler | `spooler` | Manages print jobs |
| DHCP Client | `dhcpc` | Obtains IP addresses |
| DNS Client | `Dnscache` | Caches DNS lookups |
| Windows Defender | `WinDefend` | Antivirus service |
| Cryptographic Services | `CryptSvc` | Certificate management |
| Plug and Play | `PlugPlay` | Hardware detection |
| Windows Installer | `msiserver` | MSI package installation |

### Managing from Command Line:
```cmd
net start <service_name>
net stop <service_name>
sc query <service_name>
sc config <service_name> start= auto
```

Or PowerShell:
```powershell
Get-Service -Name <service_name>
Start-Service -Name <service_name>
Stop-Service -Name <service_name>
Set-Service -Name <service_name> -StartupType Automatic
```

## 6. User Account Control (UAC)

UAC is the prompt that asks "Do you want to allow this app to make changes to your device?" It is Windows' mechanism for privilege separation.

### How it works:
- Users get two tokens at login: a standard user token and an elevated (admin) token
- By default, processes run with the standard token
- When elevation is needed, UAC prompts for consent
- Processes marked as "requireAdministrator" in their manifest always trigger UAC

### UAC Levels:
1. **Always notify** — Prompt for everything (most secure)
2. **Notify only when apps try to make changes** — Default
3. **Notify only when apps try to make changes (do not dim desktop)** — Less secure
4. **Never notify** — UAC effectively disabled (dangerous)

### Support relevance:
- "Run as administrator" bypasses the standard token check for a single launch
- If UAC is set too high, legitimate admin tasks become tedious
- If UAC is disabled, malware can run without restriction
- Some installers silently fail if they can't elevate
- Group Policy can enforce UAC settings

## 7. User Context & Privileges

### `whoami /all`
This command shows everything about your current security context. The output has several sections:

- **User name**: Current user SID and display name
- **Group names**: Security groups the user belongs to (determines access rights)
- **Privileges**: Specific permissions granted (backup files, debug programs, etc.)
- **Mandatory label**: Integrity level (Low, Medium, High, System)

### Integrity Levels:
- **Low**: Sandboxed processes (browser protected mode)
- **Medium**: Standard user processes (default)
- **High**: Elevated/administrator processes
- **System**: OS-level processes

This is somewhat similar to Linux's user/group/other model combined with capabilities, but with an additional mandatory integrity control layer.

## 8. Environment Variables

Windows has two scopes for environment variables:
- **User variables**: Apply only to the current user (stored in registry under HKCU)
- **System variables**: Apply to all users (stored in registry under HKLM)

### Viewing and Editing:
- GUI: Settings > System > About > Advanced system settings > Environment Variables
- Command line: `set` (shows current session), `setx` (persists)
- PowerShell: `$env:VARIABLE_NAME`, `[Environment]::GetEnvironmentVariable("NAME", "Machine")`

### Common Gotchas:
- `PATH` is merged from both User and System scopes
- Changes to environment variables don't affect already-running processes
- `setx` truncates values over 1024 characters
- Some applications read environment variables at startup only and need a restart to pick up changes
- Case-insensitive: `PATH`, `Path`, and `path` all refer to the same variable
