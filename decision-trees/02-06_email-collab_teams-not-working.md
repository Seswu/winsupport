# Teams Won't Start / Audio Not Working

Microsoft Teams won't launch, or audio/video isn't working during meetings.

```
User reports: "Teams won't start" or "I can't hear people in meetings"

├─ TEAMS WON'T START
│   ├─ Clear the Teams cache:
│   │   1. Quit Teams (right-click system tray → Quit)
│   │   2. Delete: `%appdata%\Microsoft\Teams\*` (all contents)
│   │   3. Restart Teams (it will rebuild cache)
│   │   └─ Works? → Corrupt cache was the issue
│   │
│   ├─ Check if Teams is stuck in a loop:
│   │   → Task Manager → End all Teams processes
│   │   → Restart Teams
│   │
│   ├─ Check Web version:
│   │   → teams.microsoft.com → Does it work there?
│   │   ├─ YES → Desktop client issue
│   │   │   → Reinstall Teams (Settings → Apps → Microsoft Teams → Uninstall)
│   │   │   → Download fresh from https://teams.microsoft.com/downloads
│   │   └─ NO  → Service issue → Check M365 health dashboard
│   │
│   └─ Check if Teams is using the new (v2) or classic client
│       → New Teams ("Teams 2.0") has better performance
│       → Toggle: Click "Try the new Teams" button (top-left)
│
├─ AUDIO NOT WORKING
│   ├─ Check the correct audio device is selected:
│   │   Teams meeting → ... (more) → Device settings
│   │   → Check Speaker and Microphone dropdowns
│   │   → Try a different device (speakers vs headset)
│   │
│   ├─ Test audio in Windows:
│   │   Settings → System → Sound → Test microphone / Test speakers
│   │   ├─ Works in Windows → Issue is Teams-specific
│   │   │   → Reset Teams audio settings: %appdata%\Microsoft\Teams\desktop-config.json
│   │   │   → Delete or rename this file → Restart Teams
│   │   └─ Fails in Windows → Driver or hardware issue
│   │       → [see 05-05](05-05_hardware_webcam-microphone.md)
│   │
│   ├─ Check privacy settings:
│   │   Settings → Privacy & security → Microphone
│   │   → Ensure "Allow apps to access your microphone" is ON
│   │   → Ensure "Microsoft Teams" is in the allowed list
│   │
│   ├─ Check if another app is using the audio device:
│   │   → Close other apps using audio (Zoom, browser, etc.)
│   │   → Right-click speaker icon → Sound → More sound settings
│   │   → Troubleshoot → Find which app has exclusive control
│   │
│   └─ Check for audio enhancements:
│   │   Settings → System → Sound → More sound settings
│   │   → Right-click default device → Properties → Advanced
│   │   → Uncheck "Enable audio enhancements"
│   └─
│
├─ VIDEO / WEBCAM NOT WORKING
│   ├─ Check privacy settings (same as audio)
│   ├─ Check camera privacy: Settings → Privacy → Camera → Allow apps
│   └─ Is the camera being used by another app?
│       → Close other apps → restart Teams
│
└─ STILL BROKEN?
    └─ Escalate with collected details:
        Does it happen in web browser too?
        Does it affect only this user or others?
        When did it start? (after update? after hardware change?)
```

**RESULT** → Teams running and audio/video functional.
