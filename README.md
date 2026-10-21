# Tracepie public documentation

Zola source for the future `https://tracepie.com` documentation site.
**Deployment is intentionally disabled.** The active workflow only checks and
builds the site. It has no Pages deployment steps or write permissions.

## Local development

Install Zola **0.23.6** from [Zola's official releases](https://github.com/getzola/zola/releases/tag/v0.23.6), then:

```sh
git clone --recurse-submodules https://github.com/tracepie/docs.git
cd docs
zola serve
```

Open the printed local URL (normally `http://127.0.0.1:1111`). For an existing
clone, run `git submodule update --init` before building.

```sh
zola check --skip-external-links
zola build
```

The generated site is in ignored `public/`. Add guides under `content/docs/`
with TOML front matter, a title, description and weight. Use Zola `@/…` links
for internal pages so the build validates them.

## Theme and inputs

[Quiet](https://github.com/johnnybravo-xyz/zola-quiet) is an MIT-licensed Git
submodule pinned to `54c7f2905ede6909057836952dc1dfddb0fb05c0`. Its license is
retained in `themes/zola-quiet/LICENSE`. Both typography/TUI skins and light/dark
modes remain available. The ASCII sidebar is disabled in `config.toml` and the
shared `site.html` template omits the entire sidebar block, including its TOC.
Navigation appears above the document.

`templates/base.html` retains Quiet's MIT-licensed base template with two small
changes: load `docs.css` in the head and omit the Atom link when feeds are
disabled. Compare this override with upstream when updating the theme.

Zola's version is pinned in `.zola-version`; CI verifies the upstream Linux
archive's SHA-256 before extraction. GitHub Actions checkout is pinned by commit.
Initial public copy is a brief synthesis of Tracepie's existing documentation;
internal architecture, roadmap, research and implementation files are not copied.

## Domain and eventual publication

See [DNS and Pages setup](ops/github-pages.md) for configuration and status.
`config.toml` uses the intended canonical URL and `static/CNAME` contains
`tracepie.com`. GitHub Actions publishing uses the repository's Pages domain
setting, not the CNAME file alone.

Publishing requires a separate explicit request. At that point, add a reviewed
Pages deployment workflow, upload only `public/`, and enforce HTTPS after
GitHub issues its certificate. Do not select branch-based publishing of this
source tree.
