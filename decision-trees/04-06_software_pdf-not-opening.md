# PDF Won't Open or Prints Garbled

User cannot open a PDF file, or the PDF prints as symbols/garbage text.

```
User reports: "My PDF won't open" or "PDF prints as weird symbols"

├─ PDF WON'T OPEN
│   ├─ Double-click the PDF — what happens?
│   │   ├─ Nothing → File association may be broken
│   │   │   → Right-click → Open with → Choose another app
│   │   │   → Select: Microsoft Edge, Adobe Acrobat, or a PDF reader
│   │   │   → Check: "Always use this app to open .pdf files"
│   │   ├─ Opens in browser (Edge/Chrome) but not in reader?
│   │   │   → PDF reader may not be the default
│   │   │   → Edge reads PDFs natively — this is fine
│   │   ├─ Opens but shows blank pages?
│   │   │   → PDF may be a scanned image page → Missing OCR
│   │   │   → Try opening in a different PDF reader
│   │   ├─ Opens but says "File is damaged" / "Cannot open"
│   │   │   → File is corrupt → Ask sender to re-generate the PDF
│   │   │   → Try: Right-click PDF → Properties → Unblock (if downloaded)
│   │   └─ Opens but says "This file is password-protected"
│   │       → User needs the password from the sender
│   │
│   └─ Check Edge PDF viewer:
│       → edge://settings/content/pdfDocuments → "Always open PDF files externally"
│       → Toggle OFF (open in Edge) or ON (open in default PDF app)
│
├─ PDF PRINTS AS GARBLED / SYMBOLS
│   ├─ Verify it's not just the preview:
│   │   → Try printing only the current page
│   │   → Try printing "As image" in the Print dialog
│   │
│   ├─ Common print solutions:
│   │   ├─ Print as Image:
│   │   │   → Print dialog → Advanced → "Print as Image" → ON
│   │   ├─ Use a different PDF reader to print:
│   │   │   → Open in Edge → Ctrl+P → try printing from Edge
│   │   │   → Open in Adobe Reader → try printing from there
│   │   ├─ Check if the PDF uses embedded fonts:
│   │   │   → Garbled text = font not embedded or font substitution issue
│   │   │   → Print as Image bypasses font rendering
│   │   └─ Update printer driver:
│   │       → [see 05-01](05-01_hardware_printer-not-printing.md)
│   │
│   └─ Check printer settings:
│       → Print dialog → Properties → Advanced
│       → Try: "Print text as graphics" or "Raster" mode
│
├─ SPECIFIC: "PDF is scanned image, text isn't selectable"
│   └─ This is normal for scanned docs (image-based, not text-based)
│      → OCR can make text selectable, but that's a document feature, not a fix
│
└─ STILL NOT WORKING?
    └─ Can other PDFs open fine?
        ├─ YES → This specific PDF is the issue → Ask sender to re-send
        └─ NO  → PDF reader is broken → Reinstall or repair PDF reader
            → Settings → Apps → Adobe Acrobat / PDF reader → Modify → Repair
```

**RESULT** → PDF opening and printing correctly.
