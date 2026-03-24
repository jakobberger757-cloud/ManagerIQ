# Aztec Group — US Prospecting Intelligence Tool

## Files

| File | Purpose |
|------|---------|
| `aztec_prospecting_v9.html` | The tool — open in Chrome or Safari |
| `aztec_template.html` | Template for rebuilds (copy of v9, rename it) |
| `build_aztec.py` | Rebuild the tool with new data |
| `research_server.py` | (Optional) Delaware research backend |

---

## Using the Tool

Just open `aztec_prospecting_v9.html` in Chrome or Safari. No server needed.

**Your pipeline data** (notes, status, next steps) saves automatically to browser localStorage.
Use **💾 Save State** to export it as a JSON file before clearing your browser or switching machines.
Use **📂 Load State** to restore it.

---

## Refreshing Data (monthly or quarterly)

When you have new Convergence or Preqin exports:

### Step 1 — Install dependencies (one time only)
```bash
pip3 install pandas openpyxl
```

### Step 2 — Rename your exports
Rename your fresh exports to exactly these filenames and place them in the same folder as `build_aztec.py`:

| Your export | Rename to |
|-------------|-----------|
| Convergence manager-level CSV | `convergence_managers.csv` |
| Convergence fund-level CSV | `convergence_funds.csv` |
| Preqin fund manager XLSX | `preqin_managers.xlsx` |
| Preqin fund-level XLSX | `preqin_funds.xlsx` |
| Preqin fund forecast XLSX | `preqin_forecast.xlsx` |

### Step 3 — Set up the template
Copy `aztec_prospecting_v9.html` and rename the copy to `aztec_template.html`.
Place it in the same folder as `build_aztec.py`.

### Step 4 — Run the build
```bash
python3 build_aztec.py
```

This produces a new `aztec_prospecting.html` in the same folder. Open it in your browser.

**Build time:** approximately 3–5 minutes.

---

## Updating Client and Target Lists

Open `build_aztec.py` in any text editor and find the CONFIG section near the top.

**Add a new client win:**
```python
AZTEC_CLIENTS = [
    "Your New Client Name",  # add here
    "Clearlake Capital Group",
    ...
]
```

**Add a CRM target:**
```python
CRM_TARGETS = [
    "New Prospect Name",  # add here
    ...
]
```

**Add a CRM note:**
```python
CRM_NOTES = {
    "New Prospect Name": "Met CFO at conference June 2026",
    ...
}
```

Save the file, run `python3 build_aztec.py` again.

---

## Starting Fresh in a New Claude Session

If you want Claude to modify the tool (add features, change filters), share these files:
1. `build_aztec.py` — the full data pipeline
2. `aztec_prospecting_v9.html` — the current UI

Tell Claude: *"Here is our current Aztec prospecting tool build script and HTML. Please modify X."*

Claude can then make changes to the build script and/or HTML, and you run `build_aztec.py` locally to produce the updated tool.

---

## Research Backend (Optional)

To use the Delaware entity research feature:

```bash
# Install
pip3 install flask flask-cors anthropic

# Set your API key
export ANTHROPIC_API_KEY=sk-ant-YOUR-KEY-HERE

# Run (keep this terminal open while using the tool)
python3 research_server.py
```

The research feature is available from any expanded manager card in the tool.
Cost: approximately $0.05–0.15 per firm researched.

---

## Data Sources

| Source | What it provides |
|--------|-----------------|
| Convergence (manager) | Contacts (CFO/COO/CEO + emails), SEC-filed AUM, primary admin, auditor |
| Convergence (fund) | Admin tenure — tracks which funds were filed under which admin and when |
| Preqin (manager) | Total firm AUM, HQ city/state, investment geography, year founded, staff |
| Preqin (fund) | Fund numbers, raising status, vintages, law firms, placement agents |
| Preqin (forecast) | Next launch date, cycle status, dry powder, deployment cadence |

**AUM source:** Preqin total firm AUM is shown by default (most accurate for large firms).
Convergence SEC-filed RAUM is shown as a footnote inside expanded cards.

---

## Troubleshooting

**Tool won't open:** Make sure you're using Chrome or Safari. Firefox has stricter local file security.

**Build script fails:** Check that all 5 input files are in the right folder with the exact filenames listed above.

**Missing managers:** Check that your Preqin manager export includes both US and Canada (North America filter).

**Pipeline data lost:** Restore from your last 💾 Save State JSON file using 📂 Load State.
