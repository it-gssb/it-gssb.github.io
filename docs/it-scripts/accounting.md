# accounting

Repo: [it-gssb/accounting](https://github.com/it-gssb/accounting).
A Java tool that bills every Sycamore family for the school year. It works
out each family's charges from a CSV, then drives Firefox (Selenium) to
enter the charges in Sycamore and create the invoice. Sycamore's API can't
post charges, so the tool fills in the web pages instead.

## Purpose

Yearly tuition invoicing. It replaces entering the charges for each family
by hand.

## What's in it

| Path | What it is |
| :--- | :--------- |
| `src/main/java/org/gssb/accounting/Charger.java` | Entry point and command line |
| `charge/Account.java` | **Billing rules**: tuition by child order and grade, grade supplements, parent-participation credits, school-committee credit, registration fee |
| `charge/ChargeType.java` | Charge codes and their Sycamore labels |
| `web/WebCharger.java` | Selenium steps: log in, open each family's charge page, add the lines, submit |
| `src/test/resources/.../sample.properties` | Example config with charge amounts and committee family codes |

**Inputs:**

- A config `.properties` file: `charge.<CODE> = <amount>` for every
  `ChargeType`, and `sk.family_codes = <comma list>` for the families that
  get the committee credit.
- A CSV with the columns `FamilyCode`, `FamilyID`, `ParentHelperChoice1`,
  and `ApplicationGradeCode`, one row per student.
  <!-- TODO: document which Sycamore report or script produces this CSV. -->

## Prerequisites

- Java 17 JDK and Firefox. Selenium gets the driver automatically.
- A Sycamore account that can post family charges.
- This year's charge amounts, from the school board or treasurer.

## How to run

```powershell
.\gradlew.bat installDist
build\install\accounting\bin\accounting.bat -c charges.properties -f families.csv -u <sycamore-user> -p <sycamore-password> -s
```

`-s` is **simulate**: it fills in the forms but doesn't submit them. Watch
a few families, then run again without `-s`.

!!! warning

    Billing rules and amounts change every year. Read `Account.java`,
    compare it against the current fee schedule, and update the code and
    tests before you run it. A real run posts charges to live family
    accounts.

!!! note

    It depends on Sycamore's page layout (form field names, login page).
    If Sycamore changes its UI, update `WebCharger.java`. The last fix of
    this kind was in March 2025.

## Owner

GSSB IT team, together with the GSSB treasurer.

## Last verified

2026-10-24, by GSSB IT team (from reading the code, not from a test run).
