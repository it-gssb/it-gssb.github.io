# GSSB IT Docs

Source for the GSSB IT documentation site, published at **https://it-gssb.github.io/**.

Pages are Markdown files in the [`docs/`](docs/) folder. Every push to `main`
rebuilds and redeploys the site automatically via GitHub Actions.

> **This repository and the site are public.** Never add passwords, secrets,
> or personal data about students, parents, or staff.

## Editing a page (no local setup needed)

1. On the live site, click the **Edit this page** (pencil) icon at the top of
   any page — or browse to the file under `docs/` here on GitHub and click the
   pencil icon.
2. Make your changes in the GitHub web editor.
3. Click **Commit changes…**, choose **Create a new branch and start a pull
   request**, and describe what you changed.
4. Open the pull request. Once it is reviewed and merged into `main`, the site
   updates within a few minutes.

To add a new page, copy a template from `docs/templates/` to the right folder,
fill it in, and add it to the `nav` section of `mkdocs.yml` in the same PR.
