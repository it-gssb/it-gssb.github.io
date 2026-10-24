# pdffiller

Repo: [it-gssb/pdffiller](https://github.com/it-gssb/pdffiller).
A Java command-line tool for bulk mail merge. For each row of an Excel
sheet it fills one or more PDF forms, can encrypt the results, and emails
them to the addresses in that row.

**When:** summer (acceptance letters with class and parent-helper
assignments) and spring (AATG certificates). See the
[yearly calendar](../about-gssb.md#the-school-year-and-its-part-in-it).

## Purpose

- **AATG results**: send each family their child's raw-score sheet and the
  right certificate (gold, silver, bronze, achievement, or participation).
- **Enrollment letters**: one email per family with the class assignments
  for every child. This uses row grouping.
- Any other "personalized PDF to every family" job.

## What's in it

| Path | What it is |
| :--- | :--------- |
| `src/main/java/org/gssb/pdffiller/` | The app. Entry point: `BulkNotificator`. |
| `src/test/resources/2018/` | A complete sample project: config, PDF templates, Mustache email bodies, and a test spreadsheet with fake data |
| `README.md` | **Full user guide**: every config key, grouping rules, form choices |

A mail-merge project is a folder with `config\*.properties`, `sources\`
(PDF forms, `.mustache` email bodies, `.xlsx` data), and `generated\` (the
output).

## How to run

1. Download `pdffiller-<version>.zip` from
   [Releases](https://github.com/it-gssb/pdffiller/releases) and unzip it.
   The newest release (0.1.3, 2019) is older than the code on `master`. To
   use the current code, build it yourself:

    ```powershell
    .\gradlew.bat build
    # creates build\distributions\pdffiller-<version>.zip
    ```

2. Create a project folder. Copy `src\test\resources\2018\` as a starting
   point.
3. Do a dry run. This generates the PDFs and logs the emails without
   sending them:

    ```powershell
    bin\pdffiller.bat -c <project>\config\my.properties -s
    ```

4. Check the files in `generated\`. Then send for real:

    ```powershell
    bin\pdffiller.bat -c <project>\config\my.properties -p <email-password>
    ```

| Option | Meaning |
| :----- | :------ |
| `-c` | Config file (required) |
| `-s` | Simulate: log emails instead of sending them |
| `-p` | SMTP account password. Turns on sending. |
| `-m` | Master key: encrypt PDFs (for rows that have a key in `excel.secret_column`) |
| `-g` | Restart from this group value, for example after an SMTP failure partway through |

!!! warning

    Keep the email password and master key out of `.properties` files.
    Pass them only with `-p` and `-m`.

## How it fits with the other repos

The spreadsheet columns are the interface. Use the column names from
`sycamore`'s `ExtractForRegistration.py` in your config and templates:

| Config key | Typical value |
| :--------- | :------------ |
| `excel.target_email_columns` | `PrimaryParentEmail, SecondaryParentEmail` |
| `excel.group_columns` | `FamilyCode, ...`, so each family gets one email. The first column is the grouping key. |
| Mustache fields | `{{StudentFirstName}}`, `{{Class}}`, `{{Room}}`, `{{TeacherName}}`, `{{ParentNames}}`, and so on |
| `-g` restart value | A `FamilyCode`. Emails go out sorted by it. |

For AATG certificates, the sheet also needs `Award` (to pick the
certificate), `Level`, and a key column for encryption. The sample config
uses `Name`, `LehrerIn` (teacher), and `Schule` (school). Rename the
columns, or change the `mappings.*` lines to match.

Sending needs an SMTP account for a GSSB sender address, with its host,
port, and address in the config. Ask IT which account to use. The
password goes only on the command line.

## Useful context

- [README user guide](https://github.com/it-gssb/pdffiller#readme)
- [Mustache syntax](https://mustache.github.io/mustache.5.html), used for
  file names, subjects, and email bodies
- Input sheets usually come from [sycamore](sycamore.md)
  (`ExtractForRegistration.py`, or `AatgConvert.py` for AATG results).

## Owner

GSSB IT team.

## Last verified

2026-10-24, by GSSB IT team (from reading the code, not from a test run).
