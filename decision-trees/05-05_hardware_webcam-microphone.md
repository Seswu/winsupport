# Webcam / Microphone Not Working

The built-in or external webcam/microphone is not detected or not working in applications.

```
User reports: "My camera/mic isn't working in [Teams/Zoom/etc.]"

├─ CHECK THE OBVIOUS
│   ├─ Is there a physical shutter/camera cover? → Open it
│   ├─ Is there a mute switch on the mic? → Unmute
│   └─ Is the device plugged in? → Re-seat USB connection
│
├─ WINDOWS PRIVACY SETTINGS
│   ├─ **Camera**:
│   │   → Settings → Privacy & security → Camera
│   │   ├─ "Camera access" → ON
│   │   ├─ "Let apps access your camera" → ON
│   │   └─ Check the specific app (Teams, Zoom) → ON
│   │
│   └─ **Microphone**:
│   │   → Settings → Privacy & security → Microphone
│   │   ├─ "Microphone access" → ON
│   │   ├─ "Let apps access your microphone" → ON
│   │   └─ Check the specific app → ON
│   │
│   └─ Note: Windows updates sometimes reset these permissions
│
├─ DEVICE MANAGER
│   ├─ Open: `devmgmt.msc`
│   │   ├─ Camera → Under "Cameras" or "Imaging devices"
│   │   │   ├─ Device missing → Driver not loaded
│   │   │   ├─ Yellow exclamation → Driver issue
│   │   │   └─ Device present but grey → Disabled → Right-click → Enable
│   │   └─ Microphone → Under "Audio inputs and outputs"
│   │       → Same checks as camera
│   │
│   ├─ Update driver:
│   │   → Right-click device → Update driver → Search automatically
│   │   → If no update found → Check manufacturer's site
│   │
│   └─ Disable and re-enable:
│   │   → Right-click device → Disable → Wait → Enable
│   └─
│
├─ APPLICATION-SPECIFIC
│   ├─ Check which device the app is using:
│   │   → Teams: Settings → Devices → Camera / Microphone dropdowns
│   │   → Zoom: Settings → Video / Audio → Select the correct device
│   │
│   ├─ Check if the app has permission in Windows:
│   │   → Settings → Privacy → Camera → Which apps? → Check the app is listed and ON
│   │
│   └─ Restart the application → Test
│
├─ COMMON FIXES
│   ├─ Run the audio troubleshooter:
│   │   → Settings → System → Troubleshoot → Other troubleshooters → Audio
│   │
│   ├─ Restart the Windows Audio service:
│   │   → `services.msc` → Windows Audio → Restart
│   │
│   ├─ Check sound output device:
│   │   → Right-click speaker icon → Sound settings
│   │   → Check the correct output/input device is default
│   │
│   └─ Check if another app has exclusive control of the mic/camera:
│   │   → Close all other apps that use audio/video
│   │   → Right-click speaker icon → Sound → More sound settings
│   │   → Recording tab → Properties → Advanced → Uncheck exclusive mode
│   └─
│
├─ DEVICE-SPECIFIC FIXES
│   ├─ **Built-in webcam on laptop**
│   │   → Check if a function key (F-key) disables the camera
│   │   → Look for keyboard icon with camera → Press Fn + that key
│   │
│   ├─ **External USB webcam**
│   │   → [see 05-04](05-04_hardware_usb-not-recognized.md)
│   │
│   ├─ **Headset microphone**
│   │   → Check the TRRS connector (single plug with 3 rings)
│   │   → Some laptops have separate mic and headphone jacks
│   │   → Make sure it's fully inserted
│   │
│   └─ **Bluetooth headset**
│   │   → Is it paired and connected?
│   │   → Settings → Bluetooth → Check device status
│   │   → Remove and re-pair if needed
│   └─
│
└─ STILL NOT WORKING?
    └─ Test in another app:
        ├─ Works in other app → Issue is the original application
        └─ Fails in all apps → Hardware or driver issue → Escalate
```

**RESULT** → Webcam/microphone working in all applications.
