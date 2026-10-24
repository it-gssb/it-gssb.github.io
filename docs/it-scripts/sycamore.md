# sycamore

Repo: [it-gssb/sycamore](https://github.com/it-gssb/sycamore).
Python scripts that pull data out of Sycamore through its REST API and
reshape it for other systems, plus PowerShell scripts that sync student
accounts into Microsoft Entra ID. This is the most active repo and the
starting point for most yearly IT tasks.

## Purpose

- **Microsoft SDS**: generate the CSV files that School Data Sync imports
  to create Teams classes for students and teachers.
- **Student accounts**: create, enable, or disable `student.gssb.org`
  accounts in Entra so they match current enrollment, and reset passwords at
  the start of the year.
- **Registration and family lists**: one spreadsheet with a row per student
  per class, including parent contacts and custom fields. It feeds mail
  merges ([pdffiller](pdffiller.md)) and AATG registration.
- **Data quality**: report Sycamore records that break naming rules before
  they turn into bad email addresses.

## What's in it

| Path | What it does | Output |
| :--- | :----------- | :----- |
| `src/lib/` | Shared code: REST client (`SycamoreRest`), on-disk cache (`SycamoreCache`), and naming, email, and phone formatting rules (`Generators`) | n/a |
| `src/sds/ExtractFromSycamoreToSDS21.py` | SDS v2.1 import files (the current format) | `orgs`, `users`, `roles`, `classes`, `enrollments`, `academicSessions`, `courses`, `relationships` `.csv` |
| `src/sds/ExtractFromSycamoreToSDSClever.py` | Older SDS import in the Clever CSV format | `schools`, `students`, `sections`, `teachers`, `enrollments`, `user` `.csv` |
| `src/sds/ExtractStudents.py` | Student list for the Entra scripts | `azure-ad-students.csv` |
| `src/powershell/*.ps1` | Entra sync through Microsoft Graph: `create-students`, `set-students-accountenabled`, `set-students-password`, `delete-groups` | Changes in Entra |
| `src/extract/ExtractForRegistration.py` | Registration and family sheet (`--xlsx` for Excel) | `registrations.csv` or `.xlsx` |
| `src/extract/ValidateRegistrationData.py` | Prints rule violations in names (stray whitespace, punctuation, and so on) | Console |
| `src/extract/AatgConvert.py` | Converts AATG result HTML reports (from Quia) into a CSV for certificates | CSV |
| `src/extract/MergeCSVFiles.py`, `FilterAndProjectCSVFiles.py` | General tools: join two CSVs, or run SQL over CSVs | CSV |
| `src/extract/SycamoreExtract.py`, `src/clever/` | Legacy. Kept for reference. | n/a |

## Prerequisites

- Python 3 on Windows.
- A **Sycamore API access token**. Create your own in Sycamore:
  *My Organizer → Utilities → Applications → Access Tokens*.
  Never save it in the repo.
- GSSB's **Sycamore school ID**. It's in the
  [repo README](https://github.com/it-gssb/sycamore#operation).
- For PowerShell: an Entra admin account, and the Microsoft Graph module:

    ```powershell
    Set-ExecutionPolicy -ExecutionPolicy Bypass -Scope CurrentUser
    Install-Module Microsoft.Graph -Scope CurrentUser
    ```

## How to run

Install the dependencies once, from the repo root:

```powershell
py -m venv .venv
.venv\Scripts\pip install pandas openpyxl phonenumbers urllib3 certifi
```

!!! note

    `src/extract/requirements.txt` pins old versions (for example
    pandas 1.5.3) that may not install on current Python. If it fails,
    use the unpinned install above.

Run each script **from its own folder**: the scripts import `lib` through
`..`. Every Python extract takes the same arguments:

| Argument | Meaning |
| :------- | :------ |
| `--school` | Sycamore school ID |
| `--token` | Sycamore API token |
| `--cache DIR` | Folder for cached API responses. Reused on later runs. |
| `--reload` | Ignore the cache and download everything again |
| `--out DIR` | Output folder. Created if it's missing. |

=== "SDS import files"

    ```powershell
    cd src\sds
    ..\..\.venv\Scripts\python ExtractFromSycamoreToSDS21.py --school <id> --token <token> --cache cache --out outv21
    ```

    Then upload the files in `outv21` to SDS.

=== "Entra student sync"

    ```powershell
    cd src\powershell
    ..\..\.venv\Scripts\python ..\sds\ExtractStudents.py --school <id> --token <token> --cache cache --out out
    powershell .\create-students.ps1 -csvFile out\azure-ad-students.csv
    powershell .\set-students-accountenabled.ps1 -csvFile out\azure-ad-students.csv
    ```

    The PowerShell scripts do a **dry run by default**. Read the output,
    then run them again with `-dryRun 0`. Add `-verboseOutput 1` for more
    detail.

=== "Registration sheet"

    ```powershell
    cd src\extract
    ..\..\.venv\Scripts\python ExtractForRegistration.py --school <id> --token <token> --cache cache --out out --xlsx
    ```

!!! warning "The cache holds personal data"

    The `cache` folder stores raw Sycamore responses for every family.
    Delete it when you're done, and use `--reload` when you need fresh data.

## Start-of-year tasks

1. Delete old SDS class groups, keeping one year of history:
   `powershell .\delete-groups.ps1 -prefixToDelete Exp0824_`. Change the
   prefix to match the SDS end-of-year prefix from two years back.
2. Reset student passwords to match the welcome letters:
   `powershell .\set-students-password.ps1 -csvFile out\azure-ad-students.csv`.
3. Update the school-year custom field names at the top of
   `ExtractForRegistration.py` (for example `Parent Participation 2026/27`).

Both scripts dry-run first. Then run them again with `-dryRun 0`.

## Useful context

- [Repo README](https://github.com/it-gssb/sycamore#readme) and
  [PowerShell README](https://github.com/it-gssb/sycamore/tree/master/src/powershell)
- Sycamore API endpoints in use: `src/lib/SycamoreEntity.py` and
  `src/lib/SycamoreCache.py`
- [SDS v2.1 CSV format](https://learn.microsoft.com/en-us/schooldatasync/sds-v2.1-csv-file-format)
- [Microsoft Graph PowerShell](https://learn.microsoft.com/en-us/powershell/microsoftgraph/installation)

## Owner

GSSB IT team.

## Last verified

2026-10-24, by GSSB IT team (from reading the code, not from a test run).
