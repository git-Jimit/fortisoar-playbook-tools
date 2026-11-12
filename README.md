# FortiSOAR Playbook Tools

Optional companion tools for the
[FortiSOAR Playbook Builder](https://github.com/git-Jimit/fortisoar-playbook-builder)
skill ([website](https://git-jimit.github.io/fortisoar-playbook-builder-site/)).

The skill works without them: it carries the same checks and methods as written
instructions. These programs apply them mechanically, which is faster and more
reliable on large playbooks.

> Not affiliated with or endorsed by Fortinet. FortiSOAR, FortiSIEM and
> FortiAnalyzer are trademarks of Fortinet, Inc.

## The tools

| File | What it does | Skill reference it implements |
|---|---|---|
| `validate_playbook.py` | Checks a playbook export for the faults that make it fail to import, fail at run time, or run and do nothing | `reference/validation-checklist.md` |
| `extract_connector_configs.py` | Lists every connector config, version and operation, picklist uuid and sub-playbook reference in an export from your instance | `reference/conventions.md`, "Connector configuration" |
| `pdf_report_engine.py` | Draws an A4 PDF inside a FortiSOAR Code Snippet step, with no PDF library, and attaches it to the record | `reference/reports.md` |

## Usage

Python 3 only. `jinja2` is optional: if it is installed, the validator also
syntax-checks every Jinja template.

```bash
git clone https://github.com/git-Jimit/fortisoar-playbook-tools.git
python3 fortisoar-playbook-tools/validate_playbook.py my_playbook.json
python3 fortisoar-playbook-tools/extract_connector_configs.py export_from_my_instance.json
```

The validator also enforces the skill's layout rules:
- the designer grid: `left` = 125 + 175 × column, `top` = 30 + 135 × row;
- no two steps in the same position;
- no route back to an earlier step;
- playbook descriptions of 255 characters or fewer.

The validator prints each finding as ERROR, WARN or INFO and exits non-zero if
there is any ERROR.
- **ERROR**: the playbook will fail or silently do nothing.
- **WARN**: it will run, but something will hurt at run time.
- **INFO**: an accepted exception, such as an unattached spare step.

The delivery bar for a generated playbook is 0 errors and 0 warnings. Exports
made by hand in the FortiSOAR designer commonly show warnings; read them as a
review list.

`pdf_report_engine.py` is not run on your machine. Paste it into a FortiSOAR
Code Snippet step after your data preparation, set the `REPORT_*` values at its
top, and draw with `new_page()`, `banner()`, `sec()`, `key_value()`,
`data_table()` and `body_text()`. Then call `finish_pdf()` and `attach()`. The
header comment in the file shows the full sequence.

## Privacy and safety

- The validator and the extractor read only the file you pass them and print to
  the terminal.
- They make no network calls, read no credentials or environment variables, and
  install nothing.
- The PDF engine runs inside your own FortiSOAR and uploads the report only to
  that instance.

## License

MIT, see [`LICENSE`](LICENSE). © 2026 Jimit Patel.
