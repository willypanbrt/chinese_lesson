# AGENTS.md

## Cursor Cloud specific instructions

This repository is **data-only**. It currently contains a single asset:

- `pleco_cards.txt` — a [Pleco](https://www.pleco.com/) flashcard import file with Chinese
  vocabulary and example sentences.

There is **no application, source code, package manifest, build system, lint config, test
suite, or runnable service**. As a result:

- There is nothing to install, build, lint, test, or run as a long-lived service.
- The update script is intentionally a no-op (`true`); do not add dependency installs.

### File format

`pleco_cards.txt` follows Pleco's plain-text flashcard import format (UTF-8, Unix LF):

- Lines starting with `//` are category/folder headers (e.g. `// Folder/Subfolder/Section`).
- Every other non-blank line is a card with exactly three TAB-separated fields:
  `headword<TAB>pinyin<TAB>definition`.

### Validating the data (the closest thing to "running" this repo)

To confirm the file imports cleanly into Pleco, validate that each card line has exactly
three tab-separated fields and that the file is valid UTF-8:

```bash
python3 - <<'PY'
import sys
errors=[]
with open('pleco_cards.txt', encoding='utf-8') as f:
    for i, line in enumerate(f, 1):
        line=line.rstrip('\n')
        if line=='' or line.startswith('//'):
            continue
        if len(line.split('\t'))!=3:
            errors.append(i)
print("Format errors on lines:", errors or "none")
sys.exit(1 if errors else 0)
PY
```
