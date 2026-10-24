# GSSB IT Docs

Welcome! This site collects how-to guides and fixes for the people who
look after IT at GSSB. The goal is simple: anyone should be able to pick up
a task by following a written, tested procedure, even if they have never
done it before.

## What you'll find here

- **[Common issues](common-issues/projector.md)**: quick fixes for everyday
  problems, like a projector that shows nothing.
- **[AATG](aatg/index.md)**: registering students for the National German
  Exam.
- **[IT scripts](it-scripts/index.md)**: what each repo in the
  [it-gssb](https://github.com/it-gssb) organization does, and how to run it.
- **[Templates & examples](templates/index.md)**: copy-and-fill starting
  points for new pages, plus examples of the formatting you can use.

## The one rule: nothing sensitive, ever

!!! danger "This repository and this site are public"

    Everything here is visible to anyone on the internet, **including the full
    edit history**. Deleting something later does not remove it from history.

    **Never** add:

    - Passwords, PINs, API keys, recovery codes, or Wi-Fi keys
    - Personal data about students, parents, or staff (names, emails, phone
      numbers, addresses, photos, grades, account details…)
    - Internal IP addresses, serial numbers, license keys, or anything else
      that would help someone attack our systems

    Write *where* a secret lives instead ("the admin password is in the
    password manager under *Office Printer*"), never the secret itself.
    If something sensitive slips in, tell an admin immediately. Don't just
    delete it.

## How to contribute

You don't need to install anything. Everything happens on GitHub:

1. Click the **Edit this page** icon (:material-file-edit-outline:) at the
   top right of any page.
2. Make your changes in GitHub's editor. Pages are written in
   [Markdown](https://www.markdownguide.org/basic-syntax/).
3. Click **Commit changes…**, choose **Create a new branch and start a pull
   request**, and briefly describe what you changed.
4. An automatic check builds the site from your change. If it shows a red ❌,
   open the check's details to see what's wrong.
5. Once your pull request is reviewed and merged, the live site updates
   within a few minutes.

To add a **new page**, pick a template from
[Templates & examples](templates/index.md), copy it into the right folder
under `docs/`, fill it in, and add it to the `nav` section of `mkdocs.yml`
in the same pull request.
