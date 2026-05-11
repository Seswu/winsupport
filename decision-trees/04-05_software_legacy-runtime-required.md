# Java / .NET / Legacy Runtime Required

A legacy application requires Java, .NET Framework, Visual C++ Redistributable, or another runtime to install or run.

```
User reports: "The app says I need Java / .NET / VC++ to run"

├─ WHAT RUNTIME IS NEEDED?
│   ├─ **Java**
│   │   ├─ Which version? (Java 8, Java 11, Java 17, etc.)
│   │   │   → Check the app's requirements documentation
│   │   │   → Open `java -version` in CMD to see what's installed
│   │   ├─ Download from: https://adoptium.net (OpenJDK / Eclipse Temurin)
│   │   │   → Or the vendor's site (Oracle Java may require license)
│   │   ├─ Install → Verify: `java -version`
│   │   └─ Note: Multiple Java versions can coexist
│   │       → Configure JAVA_HOME environment variable if needed
│   │
│   ├─ **.NET Framework**
│   │   ├─ Which version? (3.5, 4.7.2, 4.8, .NET Desktop Runtime 6/7/8)
│   │   ├─ Check what's installed:
│   │   │   → `reg query "HKLM\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full" /v Release`
│   │   │   → Or: Settings → Apps → look for "Microsoft .NET Framework"
│   │   ├─ **.NET Framework 3.5** (includes 2.0 and 3.0):
│   │   │   → Enable via: Turn Windows features on or off → .NET Framework 3.5
│   │   │   → May need installation media if WSUS/Windows Update can't supply
│   │   ├─ **.NET Framework 4.8**:
│   │   │   → Download the offline installer from Microsoft
│   │   └─ **.NET Desktop Runtime** (modern .NET 6/7/8):
│   │       → Download from: dotnet.microsoft.com/en-us/download/dotnet
│   │
│   ├─ **Visual C++ Redistributable**
│   │   ├─ Which year? (2005, 2008, 2010, 2012, 2013, 2015-2022)
│   │   ├─ Check what's installed:
│   │   │   → Settings → Apps → Search "Microsoft Visual C++"
│   │   │   → Note which versions are present
│   │   ├─ Download the "All-in-one" or individual installers from Microsoft
│   │   │   → "Visual C++ Redistributable latest supported downloads"
│   │   └─ The 2015-2022 package covers all modern versions
│   │
│   └─ **DirectX / Silverlight / Flash** (older legacy apps)
│       ├─ DirectX: Run the DirectX End-User Runtime Web Installer
│       ├─ Silverlight: May still be needed for old intranet apps
│       │   → Download from Microsoft (archived)
│       └─ Flash: Discontinued — app must be migrated to HTML5
│
├─ INSTALLATION ISSUES
│   ├─ **"Turn on Windows feature" fails (0x800F081F)**
│   │   → Windows Update can't supply the source files
│   │   → Use: DISM with /Source from Windows installation media
│   │   → `DISM /Online /Enable-Feature /FeatureName:NetFx3 /All /Source:D:\sources\sxs /LimitAccess`
│   │
│   ├─ **Installer says "already installed" but app can't find it**
│   │   → Repair the runtime installation
│   │   → Settings → Apps → Select the runtime → Modify → Repair
│   │
│   └─ **Architecture mismatch**
│       → 64-bit app needs 64-bit runtime; 32-bit app needs 32-bit (x86) runtime
│       → Install both architectures to be safe
│
└─ AFTER INSTALLATION — app still fails?
    └─ Check if the app needs environment variables:
        → JAVA_HOME (for Java)
        → Path (for .NET or VC++ command-line tools)
        → Check app documentation for specific setup steps
```

**RESULT** → Required runtime installed and application runs correctly.
