# Building Chess Engine Arena as a Standalone .exe

This guide explains how to package `main.py` into a single `.exe` that includes the **Stockfish analyzer**, **opening CSV**, and **app icon** — so end users need nothing installed.

## Chess Engine Resources
https://e.pcloud.link/publink/show?lang=en&code=kZHEppZbCDCs9wagDhvjGGM2bo36LEIvynX

---

## Prerequisites

You need **Windows** (to build a Windows `.exe`) and **Python 3.8+**.

### Install PyInstaller
```cmd
pip install pyinstaller
```

---

## Step 1 — Recommended Project Layout

Set up your folder exactly like this before building:
```
chess_arena/
|
|-- main.py
|-- assets/
|   |-- logo.ico
|   `-- logo.png
|
|-- opening/
|   `-- openings_sheet.csv
|
`-- analyzer/
    `-- stockfish.exe  (download from https://stockfishchess.org/)
```

---

## Step 2 — Use `resource_path()` in your script

Before building, make sure `main.py` resolves bundled file paths correctly at runtime:
```python
import sys, os

def resource_path(relative_path):
    """Get absolute path to resource, works for dev and PyInstaller."""
    base_path = getattr(sys, '_MEIPASS', os.path.dirname(os.path.abspath(__file__)))
    return os.path.join(base_path, relative_path)
```

Use `resource_path("analyzer/stockfish.exe")` and `resource_path("opening/openings_sheet.csv")` wherever those files are referenced.

---

## Step 3 — Run the Build Command
```cmd
pyinstaller --onefile --windowed --icon=assets/logo.ico --name="ChessEngineArena" --add-data="opening;opening" --add-data="analyzer;analyzer" --add-data="assets;assets" main.py
```

### Flag Reference

| Flag | Purpose |
|------|---------|
| `--onefile` | Bundle everything into a single `.exe` |
| `--windowed` | Hide the console window |
| `--icon=assets/logo.ico` | Set the app icon |
| `--name="ChessEngineArena"` | Name of the output `.exe` |
| `--add-data="opening;opening"` | Bundle the `opening/` folder |
| `--add-data="analyzer;analyzer"` | Bundle the `analyzer/` folder |
| `--add-data="assets;assets"` | Bundle the `assets/` folder |

> The separator in `--add-data` is `;` on Windows and `:` on Linux/macOS.

---

## Step 4 — Find Your Output
```
chess_arena/
`-- dist/
    `-- ChessEngineArena.exe   <- distribute this file
```

The `build/` folder and `.spec` file are temporary and can be deleted.

---

## Distribution

Users only need the single `.exe` file. No Python, no folders, no setup required. Stockfish and the opening book are extracted automatically to a temp folder when the app launches.

---

## Troubleshooting

**Build fails: logo.ico not found**
Make sure `logo.ico` is inside the `assets/` folder and the path `assets/logo.ico` is used in the build command.

**App opens then immediately closes**
Remove `--windowed` to see the error in the console:
```cmd
pyinstaller --onefile --icon=assets/logo.ico --name="ChessEngineArena" --add-data="opening;opening" --add-data="analyzer;analyzer" --add-data="assets;assets" main.py
```

**Analyzer not found at runtime**
You skipped Step 2. The script must use `resource_path()` or Stockfish will not be found in the temp extraction folder.

**Windows Defender / antivirus flags the .exe**
This is common with PyInstaller executables and chess engines combined. Add an exception in Windows Defender for the `dist/` folder during testing. For distribution, consider code signing.

**The .exe is very large (80–150 MB)**
Normal — it contains Python, tkinter, and Stockfish. To compress with UPX:
```cmd
pyinstaller --onefile --windowed --upx-dir="C:\path\to\upx" --icon=assets/logo.ico ...
```
Download UPX from https://upx.github.io

**Want repeatable builds?**
Edit the auto-generated `ChessEngineArena.spec` file and rebuild:
```cmd
pyinstaller ChessEngineArena.spec
```

---

## Full Build Command (copy-paste)

Windows CMD:
```cmd
pyinstaller --onefile --windowed --icon=assets/logo.ico --name="ChessEngineArena" --add-data="opening;opening" --add-data="analyzer;analyzer" --add-data="assets;assets" main.py
```