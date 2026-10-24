# About GSSB

Background for new volunteers: what the school is, who runs what, and what
happens when during the year. Read this first if you have no context.

!!! note "Sources"

    Facts come from the public [gssb.org](https://www.gssb.org/) website
    (October 2026). Items marked *(inferred)* are guesses from the code and
    commit history in the [it-gssb repos](it-scripts/index.md). Please
    correct them if you know better.

## What GSSB is

**German Saturday School Boston** teaches German to children aged 4–17.
Classes run on Saturdays from September to May, about 30 school days a
year. They're held at William Diamond Middle School ("DMS") in Lexington,
MA.

- It's volunteer-run, governed by a 9-member **School Committee**
  (*Schulkomitee*, "SK" in the code), made up mostly of parents.
- The committee reports to the **Boylston Schul-Verein (BSV)** in
  Walpole, MA, which authorizes the school. Graduation takes place at BSV.
- GSSB is a **PASCH** partner school, which means it can offer German
  government language diplomas (DSD, IVA).

## Who's who

These are roles, not people. Current names are on
[gssb.org/administration](https://www.gssb.org/administration).

| Role | Involved in | Contact |
| :--- | :---------- | :------ |
| **IT / Registrar** (School Committee) | Sycamore, student accounts, all scripts here | `it@gssb.org`, `registrar@gssb.org` |
| Principal and Vice Principal (Academics) | Classes, teachers, exams | `principal@gssb.org` |
| Treasurer (School Committee) | Fees, invoicing rules | `finance@gssb.org` |
| Controller (staff) | Registration and financial operations | `accounting@gssb.org` |
| Parent Helper Coordinator (School Committee) | Parent volunteer assignments | `ParentHelpers@gssb.org` |
| Front Desk Manager (staff) | Daily administration | `administration@gssb.org` |
| Committee Chair | Decisions, priorities | `committee@gssb.org` |

## The school year and IT's part in it

This uses the 2026/27 dates from the
[school calendar](https://www.gssb.org/school-calendar-26-27). Check it
each year, because dates move.

| When | What happens | IT task | Repo |
| :--- | :----------- | :------ | :--- |
| **March** | Returning families register | Registration runs in Sycamore | n/a |
| **April** | New and wait-listed families register | Same | n/a |
| **Spring, after registration** *(inferred)* | One invoice per family, due within 2 weeks. Parent Helper discount applied | Post charges and create invoices in Sycamore | [accounting](it-scripts/accounting.md) |
| **Summer, before school starts** | Acceptance package goes out: class assignment and parent-helper role | Registration sheet, then one mail-merged letter per family | [sycamore](it-scripts/sycamore.md), [pdffiller](it-scripts/pdffiller.md) |
| **Late August to early September** | First day of school (Sep 12) | Delete last-but-one year's SDS groups, sync student accounts, reset passwords to match welcome letters, upload SDS files for Teams classes | [sycamore](it-scripts/sycamore.md) |
| **During the year** | Late registrations and withdrawals | Run the Entra student sync again | [sycamore](it-scripts/sycamore.md) |
| **October–November** *(inferred)* | Getting ready for the AATG exam | Bulk-create student accounts with the test provider | [AATGRegistration](it-scripts/aatg-registration.md) |
| **November / January / March** | DSD II written, DSD II oral, DSD I written | None known | n/a |
| **December** | Holiday celebration (Dec 19), with a visit from Nikolaus | The registration sheet includes the Nikolaus-permission field | [sycamore](it-scripts/sycamore.md) |
| **January (3 Saturdays)** | AATG National German Exam, grades 6–10 | Exam-day account problems | n/a |
| **Spring** *(inferred)* | AATG results and certificates | Convert result reports to CSV, then mail-merge certificates to families | [sycamore](it-scripts/sycamore.md) (`AatgConvert.py`), [pdffiller](it-scripts/pdffiller.md) |
| **April** | IVA written and oral exams | None known | n/a |
| **May** | Last school days, graduation at BSV | None known | n/a |

## Glossary

| Term | Meaning |
| :--- | :------ |
| **Sycamore** | [Sycamore School](https://www.sycamoreschool.com/), the student information system. Families, students, classes, registration, and billing all live here. It's the source of truth. |
| **Parent Portal** | The family-facing side of Sycamore ([gssb.org/parent-portal](https://www.gssb.org/parent-portal)) |
| **Entra ID** | Microsoft 365 directory with the `student.gssb.org` student accounts |
| **SDS** | Microsoft [School Data Sync](https://learn.microsoft.com/en-us/schooldatasync/). Imports Sycamore class lists to create Teams classes. |
| **AATG** | American Association of Teachers of German. Runs the yearly [National German Exam](https://www.aatg.org/?page=NGE) (levels 2–4 at GSSB). |
| **Lingco** | The online test provider used for the AATG exam (2022–2023) |
| **Quia** | The earlier AATG test platform. `AatgConvert.py` parses its HTML result reports. |
| **DSD I / DSD II** | Deutsches Sprachdiplom: German government language diplomas at B1, and at B2/C1. Prep classes start in grade 7. |
| **IVA** | Internationale Vergleichsarbeit: a German test at level A2 |
| **PASCH** | [Schools: Partners for the Future](https://www.pasch-net.de/), Germany's partner-school network |
| **Parent Helper** | Required parent volunteering, about 200 roles a year. Families rank their choices at registration, and taking a role earns a tuition discount (`PCFul` and `PCHal` in [accounting](it-scripts/accounting.md)). |
| **TAP** | Tuition Assistance Program. Requires parent-helper participation. |
| **SK** | *Schulkomitee*, the School Committee. Members get a tuition credit (`SK100`). |
| **BSV** | Boylston Schul-Verein, which authorizes GSSB |
| **DMS** | William Diamond Middle School, where classes are held |
| **Nikolaus** | St. Nicholas visits at the December celebration. Families give permission at registration. |

## Useful links

- [Admissions](https://www.gssb.org/admissions) and
  [Registration FAQ](https://www.gssb.org/registration-faq)
- [Tuition and financial aid](https://www.gssb.org/tuitition)
- [Curriculum and exams](https://www.gssb.org/academics)
- [Handbook and policies](https://www.gssb.org/handbook-and-policies)

## Owner

GSSB IT team.

## Last verified

2026-10-24, by GSSB IT team (from gssb.org and the repos).
