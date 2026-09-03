# nourabdelfattah.github.io

Personal academic website for **Nourhan E. H. Abdelfattah, Ph.D.** - computational
and translational biologist (cancer immunology, tumor microenvironment, neuroinflammation, single-cell & spatial
multi-omics).

Live at <https://nourabdelfattah.github.io>.

Built with [Jekyll](https://jekyllrb.com/) and the
[al-folio](https://github.com/alshedivat/al-folio) theme.

## Pages

| Page         | Source                                                            |
| ------------ | ----------------------------------------------------------------- |
| About / home | `_pages/about.md`                                                 |
| Publications | `_bibliography/papers.bib` (rendered by `_pages/publications.md`) |
| CV           | `_data/cv.yml` (rendered by `_pages/cv.md`)                       |
| Projects     | `_projects/*.md` (index at `_pages/projects.md`)                  |

Site-wide identity and settings live in `_config.yml`.

## Local preview

With Docker (builds from `./Dockerfile`, Ruby 3.3):

```bash
docker compose up
# serves http://localhost:8080 with live reload
```

Or with a local Ruby 3.2–3.3 toolchain (not 3.4+/4.x — a plugin dependency fails there):

```bash
bundle install
bundle exec jekyll serve
```

### Behind a TLS-intercepting network (e.g. hospital/corporate firewall)

`bundle install` inside Docker can fail with `Could not verify the SSL certificate
for https://rubygems.org/`. Export this machine's trusted CAs and let the
container use them:

```bash
security find-certificate -a -p /Library/Keychains/System.keychain > .ca-bundle.pem
security find-certificate -a -p ~/Library/Keychains/login.keychain-db >> .ca-bundle.pem
security find-certificate -a -p /System/Library/Keychains/SystemRootCertificates.keychain >> .ca-bundle.pem
```

A gitignored `docker-compose.override.yml` (already in this repo) points
`SSL_CERT_FILE` at that bundle, so `docker compose up` then works. Both files are
gitignored and never deployed. GitHub Actions builds on GitHub's runners and is
unaffected.

## Deployment

Pushing to `main` triggers `.github/workflows/deploy.yml`, which builds the site
and publishes `_site/` to the `gh-pages` branch. In **Settings → Pages**, set the
source to **Deploy from a branch → `gh-pages` / root**.

## Updating content

- **New paper:** add a BibTeX entry to `_bibliography/papers.bib`. Add
  `selected={true}` to feature it on the home page.
- **CV change:** edit `_data/cv.yml`.
- **New project:** add a file under `_projects/` with `category: released` or
  `category: in development`.
- **Headshot:** replace `assets/img/prof_pic.jpg`.

---

Theme upstream: <https://github.com/alshedivat/al-folio> (MIT License, retained in `LICENSE`).
