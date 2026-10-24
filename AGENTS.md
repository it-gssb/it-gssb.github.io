# AGENTS.md

MkDocs Material docs site → https://it-gssb.github.io/ (GitHub Pages, deployed by Actions).

## Hard rules
- Repo and site are PUBLIC, history included. Never add secrets, passwords, keys, internal IPs/serials, or personal data about students, parents, or staff. Reference where a secret is stored, never its value.
- Maintainers edit via the GitHub web UI + PR. Never require a local build to update a page.
- AGENTS.md must be correct at all times, and must use terse language

## Layout
- `docs/`: Markdown pages. `mkdocs.yml`: config + explicit `nav`.
- New page → add it to `nav` in the same change.
- Sections: `common-issues/` (fixes), `aatg/` (AATG exam registration), `it-scripts/` (one page per it-gssb org repo, for engineers), `templates/` (copyable templates + examples for non-engineers).
- Common issues follow `templates/common-issue-template.md`: Symptoms, Quick fixes, Step-by-step fix, Still not working?, Owner, Last verified.
- Repo pages (`it-scripts/`): repo link + summary, then as relevant: Purpose, What's in it, Prerequisites, How to run, Useful context; always Owner, Last verified. Windows commands; `<placeholders>` for IDs/tokens/passwords. New org repo → add page + row in `it-scripts/index.md`.
- How-tos follow `templates/how-to-template.md`: Purpose, Prerequisites, Steps, Troubleshooting, Owner, Last verified.
- `docs/templates/` is excluded by MkDocs by default; `exclude_docs: !/templates/` re-includes it. Keep it.
- Audience uses Windows only. Write Windows steps; no Mac/Linux tabs.
- Only enabled markdown extensions work (see `mkdocs.yml`); e.g. no `pymdownx.keys`.

## Build
```sh
python3 -m venv .venv && .venv/bin/pip install -r requirements.txt
NO_MKDOCS_2_WARNING=1 .venv/bin/mkdocs build --strict
```
- Must pass `--strict` with zero warnings; CI uses the same command.
- `deploy.yml`: push to main / manual → build + deploy. `pr-check.yml`: PRs → build only.

## Dependencies
- mkdocs-material is maintenance-only (EOL 2027-05-05); successor is Zensical. Planned migration once Zensical is stable.
- Keep pins: `mkdocs-material==9.8.0`, `mkdocs<2` (2.0 is incompatible).
- No third-party plugins; use only built-in Material/pymdownx features (keeps Zensical migration simple).
- No Dependabot. Bump versions only deliberately.
