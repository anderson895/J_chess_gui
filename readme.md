# ♟ Chess Engine Arena

A Python/Tkinter chess GUI that lets two UCI engines play against each other — or lets you play against an engine — with opening book recognition, live evaluation bar, and move quality analysis.

---

## Requirements

- Python 3.8+
- No third-party packages needed (uses only the standard library)

---

## Quick Start

```
python chess_arena.py
```

A loading screen will appear first. It waits until both the **Opening Book** and the **Analyzer Engine** are fully loaded before showing the main UI.

---

## Folder Structure

```
chess_arena.py          ← main script
opening/
    openings_sheet.csv  ← opening book (place your CSV here)
analyzer/
    stockfish.exe       ← Stockfish engine for move analysis (place here)
```

---

## Setup: Opening Book (CSV)

If no opening CSV is found on startup, the opening bar will show **⚠ Not found** and opening names will not be displayed during games.

### How to set it up manually

1. Create a folder named **`opening`** in the same directory as `chess_arena.py`

```
mkdir opening
```

2. Place your CSV file inside it. The file must be named one of:
   - `openings_sheet.csv` ← preferred
   - `openings.csv`

```
opening/
    openings_sheet.csv
```

3. The CSV must have these columns (header row required):

| Column  | Description                          | Example       |
|---------|--------------------------------------|---------------|
| `ECO`   | ECO code                             | `B20`         |
| `name`  | Opening name                         | `Sicilian Defense` |
| `moves` | Move list in SAN or UCI format       | `e4 c5 Nf3`   |

### Where to get an openings CSV

A good free source is the [lichess opening database](https://github.com/lichess-org/chess-openings):

```
https://github.com/lichess-org/chess-openings
```

Download `a.tsv`, `b.tsv`, `c.tsv`, `d.tsv`, `e.tsv`, combine them, save as `.csv` with `ECO`, `name`, `moves` columns.

### Alternative: load manually at runtime

Inside the app, click **📂 Load Openings CSV** in the left panel to browse and load any CSV file at any time — no restart needed.

---

## Setup: Analyzer Engine (Stockfish)

The analyzer is used for **move quality classification** (Brilliant / Best / Excellent / Good / Mistake / Blunder) and the **live evaluation bar**. Without it, moves are still played normally but quality badges and eval bar will not update from analysis.

### How to set it up manually

1. Download **Stockfish** from the official site:

```
https://stockfishchess.org/download/
```

2. Create a folder named **`analyzer`** in the same directory as `chess_arena.py`

```
mkdir analyzer
```

3. Place the Stockfish executable inside it:

```
analyzer/
    stockfish.exe          ← Windows
    stockfish              ← Linux / macOS (must be executable: chmod +x stockfish)
```

### Accepted filenames (checked in this order)

```
stockfish_18_x86-64.exe
stockfish.exe
stockfish_x86-64.exe
stockfish
```

Any of these names inside the `analyzer/` folder will be detected automatically on startup.

### Alternative: load manually at runtime

Inside the app, click **📂 Load Analyzer** in the left panel to browse and point to any Stockfish-compatible UCI engine executable.

---

## Setup: Playing Engines

The analyzer (above) is only for **analysis**. To actually play games you need one or two **separate** UCI engine executables.

- **Engine vs Engine mode:** load two engine `.exe` files using the **…** buttons in the left panel
- **Play vs Engine mode:** load one engine to play against

Any UCI-compatible engine works (Stockfish, Komodo, Leela, Fairy-Stockfish, etc.).

---

## What Happens on Startup

```
┌─────────────────────────────────────────┐
│  ♟  Chess Engine Arena                  │
│                                         │
│  📖 Openings CSV    [████░░░] Loading…  │
│  🔍 Analyzer Engine [████░░░] Loading…  │
│                                         │
│  Initialising…                          │
└─────────────────────────────────────────┘
```

The main UI will not appear until **both** finish loading. If either is not found, it is marked ⚠ and loading continues — the app will still work, just without that feature.

---

## Full Directory Example

```
chess_arena.py
opening/
    openings_sheet.csv
analyzer/
    stockfish.exe
engines/               ← optional, for your playing engines
    engine_a.exe
    engine_b.exe
```

---

## Troubleshooting

| Problem | Fix |
|---|---|
| Opening bar shows ⚠ Not found | Check the `opening/` folder exists and the CSV has `ECO`, `name`, `moves` columns |
| Analyzer shows ⚠ Not found | Check the `analyzer/` folder exists and the `.exe` name matches one of the accepted names |
| Analyzer shows ⚠ Failed | The engine was found but could not start — check it is not corrupted and (on Linux/macOS) has execute permission: `chmod +x analyzer/stockfish` |
| No move quality badges appear | Analyzer is not loaded — set it up as described above |
| Eval bar stays at 0.0 | Same as above — requires the analyzer engine |