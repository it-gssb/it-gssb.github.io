# AATGRegistration

Repo: [it-gssb/AATGRegistration](https://github.com/it-gssb/AATGRegistration).
A small Python script that creates one account per student with Lingco,
the test provider for the AATG National German Exam. Lingco has no bulk
upload, so the script calls Lingco's sign-up endpoint once per row.
Process: [AATG exam registration](../aatg/index.md).

## What's in it

| Path | What it is |
| :--- | :--------- |
| `src/AATGRegistration.py` | Entry point: reads the sheet and registers each row |
| `src/AATGRegistrationManager.py` | Lingco API calls (`/api/users/register`) |
| `src/SpreadSheetReader.py` | Reads the columns `StudentFirstName`, `StudentLastName`, `StudentGSSBEmail`, `LingcoPwd`, `StudentCode` |
| `data/AATG-Template-2022.xlsx` | Input template |

The input sheet comes from [sycamore](sycamore.md)
`ExtractForRegistration.py`, which already has the `StudentGSSBEmail` and
`LingcoPwd` columns. Filter it down to the students taking the exam.

## How to run

```powershell
py -m venv .venv
.venv\Scripts\pip install pandas openpyxl requests
cd src
..\.venv\Scripts\python AATGRegistration.py --studentSpreadSheetPath C:\path\to\students.xlsx --schoolURL www.gssb.org
```

The script prints a status code for each student. Treat anything other
than success or "already exists" as an error. Look into those rows and
register them again.

!!! warning "Last used in 2022–2023"

    Check with AATG which test provider they use this year. If it's still
    Lingco, test with one dummy row first, because their API may have
    changed. There's no dry-run mode.

## Owner

GSSB IT team.

## Last verified

2026-10-24, by GSSB IT team (from reading the code, not from a test run).
