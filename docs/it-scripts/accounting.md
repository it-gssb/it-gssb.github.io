# accounting

Repo: [it-gssb/accounting](https://github.com/it-gssb/accounting).
A Java tool that bills every Sycamore family for the school year. It works
out each family's charges from a CSV, then drives Firefox (Selenium) to
enter the charges in Sycamore and create the invoice. Sycamore's API can't
post charges, so the tool fills in the web pages instead.

**When:** once a year, after registration closes (spring). Each family
gets one invoice, payable within 2 weeks. **Who:** IT, together with the
Treasurer (`finance@gssb.org`) and the Controller (`accounting@gssb.org`).

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
  and `ApplicationGradeCode`, one row per student. Other columns are
  ignored. *(Inferred: these look like a Sycamore admissions/applications
  export. No script in the org produces them.)*
  <!-- TODO: confirm which Sycamore report produces this CSV. -->

## Billing rules (as coded)

For each family, `Account.calculateCharges`:

1. Reads the family's students' grades from `ApplicationGradeCode`:
   `-1` is preschool, `0` kindergarten, `1`–`11` grades, and `12` means
   "10th, no DSD 2". This matches the [sycamore grade map](sycamore.md#how-the-code-works).
   Grade 12 is **charged as grade 8**. The grades are then sorted from
   lowest to highest.
2. Adds the registration fee (`RegFe`) once.
3. Adds tuition per student. The **first (lowest-grade)** student gets the
   1st-child rate and the others get the 2nd/3rd-child rate. Grade 11
   (DSD II, second year) gets the half-year rates (`_1stDS`, `_2_3DS`).
4. Adds supplements: preschool and kindergarten (`PreKF`), 1st grade
   (`_1grad`), and grades 9–11 (`DSDSp`).
5. Adds a parent-helper credit if `ParentHelperChoice1` isn't empty and
   isn't `Non Participation`. It's the full credit (`PCFul`), or the half
   credit (`PCHal`) if the first student is in grade 11.
6. Adds the committee credit (`SK100`) if the family code is in
   `sk.family_codes`.

`WebCharger` then opens `familycharge.php?fid=<FamilyID>`, sets the
account to `1 : Registration`, and adds the charges:

- The first 5 charge types are picked from Sycamore's charge-code
  dropdown by label. **The labels in `ChargeType.java` must match the
  Sycamore charge codes exactly.**
- Any further charges become free-text lines, using the amount from the
  config.
- Finally it submits the form, unless `-s` is set.

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
