# GitHub organization move: tracking

The project moved from `github.com/Kintyre/*` to `github.com/ksconf/*`.

| Old                                            | New                                         | Status of old URL |
|------------------------------------------------|---------------------------------------------|-------------------|
| `github.com/Kintyre/ksconf`                    | `github.com/ksconf/ksconf`                  | redirects         |
| `github.com/Kintyre/ksconf-pre-commit`         | `github.com/ksconf/ksconf-pre-commit`       | redirects         |
| `github.com/Kintyre/ansible-collection-splunk` | `github.com/ksconf/ansible-collection-splunk` | redirects       |

This file tracks what is done and what remains.
It is a working document and can be deleted once everything below is checked off.


## Done

- [x] All `github.com/Kintyre/ksconf` URLs in README, docs, packaging metadata, Splunk app, `Dockerfile`, `Vagrantfile`, and `.splunkbase/`.
- [x] All `ksconf-pre-commit` references (docs, `.pre-commit-hooks.yaml` comment, `xmlformat.py` warning message, `setup.py` comment, and the `repository-dispatch` target in `.github/workflows/build.yml`).
- [x] The `sed` migration commands in `docs/source/git.rst` now handle old and new URLs (`Kintyre/ksconf`, `ksconf/ksconf`, and `*-pre-commit`, with an optional `.git`).
- [x] Fixed the README heading anchor in `.splunkbase/details.md`.
- [x] Changelog entry (v0.13.11 DRAFT).
- [x] README: replaced the broken Read the Docs badge URL and removed the retired Snyk badge.
- [x] `docs/source/common`: the `cdillc.splunk` link now points at `github.com/ksconf/ansible-collection-splunk`.
      The collection name `cdillc.splunk` itself is unchanged (see below).
- [x] The local `origin` remote already points at `github.com/ksconf/ksconf`.


## Remaining: in this repo

### Still pointing at the old GitHub org (intentionally, for now)

| Where | Why it is left alone |
|-------|----------------------|
| `README.md` line 5: CodeCov badge, `codecov.io/gh/Kintyre/ksconf` | The new path (`codecov.io/gh/ksconf/ksconf`) currently shows "unknown". The old path still has data (88%). Re-register/upload under the new name, then switch. |
| `README.md` line 6: Coveralls badge, `coveralls.io/.../github/Kintyre/ksconf` | Same: the new path shows "unknown", the old one shows 90%. |
| `README.md` line 7: AppVeyor badge, `ci.appveyor.com/project/lowell80/ksconf` | Not part of the org move (personal account). Not checked. `appveyor.yml` and `.travis.yml` are still in the repo; decide whether the AppVeyor badge is still wanted. |

### Names and branding (decide case by case; some are historic)

Emails updated manually by hand are not listed. The remaining occurrences:

| Where | What |
|-------|------|
| `appveyor.yml:19` | `automation@kintyre.co` (git config for CI) |
| `.splunkbase/details.md:68` | `hello@kintyre.co` (commercial support contact) |
| `docs/source/contact_us.rst:12` | `hello@kintyre.co` |
| `.github/ISSUE_TEMPLATE/bug.md:35-36` | `lowell@kintyre.co`, "Copyright (c) 2018 Kintyre Solutions, Inc." |
| `splunk_app/ksconf/default/app.conf:20` | `email = lowell@kintyre.co` |
| `ksconf/version.py:8`, `docs/source/conf.py:24` | Copyright "Kintyre Solutions" |
| `tests/test_cli_unarchive.py:36` | `automated-tests@bogus.kintyre.co` (fake test address; probably leave) |
| `docs/source/install.rst:180-181` | Sample output from an old release. Historic; probably leave. |
| `README.md:66,68` | `kintyre.rocks` short links to old talks. Check they still resolve. |

### `cdillc.splunk` Ansible collection name

These mention the collection name, not the GitHub org.
Change them only if the collection is renamed.

- `docs/source/api_ref.rst:14`
- `docs/source/common:19` (link text)
- `ksconf/builder/cache.py:25`
- `ksconf/archive.py:73`
- `docs/source/changelog.rst` (historic entries)

### Legacy `kintyre-splunk-conf` PyPI package (keep for now)

The old package name is kept for backwards compatibility.
Plan: remove it some time **after at least one release from the new repo**.

- `setup.py:13,17` (`BUILD_OLD_PACKAGE`, long-description note)
- `.github/workflows/build.yml:83,149-160` (uninstall step, "Build and publish legacy kintyre-splunk-conf")
- `ksconf/__main__.py:32` (user-facing `pip install` hint)
- `docs/source/install_advanced.rst:188`, `docs/source/install.rst:184` (old package names in examples)
- `docs/source/changelog.rst` (historic entries; do not change)


## Remaining: outside this repo

These can't be changed from a commit.

- [x] **`PRE_COMMIT_PAT` secret.** The release workflow dispatches `bumpversion-event` to `ksconf/ksconf-pre-commit`.
      Regenerated on 2026-10-05 as the fine-grained token `ksconf-pre-commit-push`, **expires 2027-10-06**.
      Renew it before then, or the automatic version bump in the hooks repo will silently stop (this is likely what happened after v0.13.9).
      The new token has not been exercised yet; confirm that an "Update to ksconf X" commit appears in `ksconf-pre-commit` after the next tagged release.
- [x] `ksconf-pre-commit`: `new_release.yml` now requests `contents: write` (the org's default workflow token is read-only), and `v0.13.10` was bumped manually via a `bumpversion-event`.
- [ ] **Release workflow secrets in general.** Check the PyPI publish credentials and any other secrets exist in `ksconf/ksconf`.
      `KSCONF_PYPI_TOKEN` and `PYPI_PASSWORD` were last updated about 4 years ago and `CODECOV_TOKEN` about 3 years ago, so verify they still work.
- [ ] **CodeCov and Coveralls.** Add the repo under the new organization, then update the badges in `README.md`.
- [ ] **Read the Docs.** Confirm the project's repository URL and webhook point at `ksconf/ksconf`.
- [ ] **Splunkbase listing** (app 4383). Update the source/support links.
- [ ] **PyPI project page.** URLs come from `project_urls` in `setup.py`, so they refresh on the next release.
- [ ] **ksconf-pre-commit repo.** Its own README/docs/CI may still reference `Kintyre/ksconf`.
- [ ] **GitHub features.** Check discussions, issue/PR templates, branch protection, and Pages/Actions settings carried over.
- [ ] **External references.** Blog posts, conference slides, Splunk Answers, and package indexes will keep the old links (they redirect).
