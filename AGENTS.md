# AI Agent Guidelines

## Project Overview

Hugo static site (personal website) using the LucentLink-Hugo theme
(git submodule). Content is Markdown with YAML front matter; layout
is Hugo HTML templates with Tailwind CSS. Hosted on GitHub Pages
with Cloudflare Pages previews.

## Build / Lint / Test Commands

```bash
# Clone (must include submodules for the theme)
git clone --recurse-submodules https://github.com/ruzickap/petr.ruzicka.dev.git

# Build the site (production)
hugo build --minify --printPathWarnings --printI18nWarnings --panicOnWarning

# Run local dev server with drafts
hugo server --buildDrafts

# Generate static output for deployment
hugo -d public
```

### Linting (run locally before pushing)

```bash
# Markdown linting (Rust-based, fast)
rumdl .

# Shell script / code block validation
shellcheck <script.sh>
shfmt --case-indent --indent 2 --space-redirects --diff <script.sh>

# JSON validation (comments allowed)
jsonlint --comments <file.json>

# Link checking (after building to public/)
lychee --root-dir ./public --no-progress public

# GitHub Actions workflow validation
actionlint

# Terraform (if applicable)
tflint
checkov --quiet -d .
trivy fs --severity HIGH,CRITICAL --ignore-unfixed .
```

There are **no unit tests** in this repo. Validation is performed
entirely through linting, link checking, and security scanning in CI.

## Code Style Guidelines

### General Formatting

- **Indentation**: 2 spaces everywhere (no tabs)
- **Line length**: Wrap at 72 characters in Markdown / commits
- **Trailing whitespace**: Remove it
- **Final newline**: Files must end with a single newline

### Markdown

- Linter: `rumdl` (not markdownlint). Config: `.rumdl.toml`
- Proper heading hierarchy (never skip levels)
- Language identifiers on all code fences (`bash`, `json`, etc.)
- Prefer code fences over inline code for multi-line examples
- `CHANGELOG.md` is auto-generated -- do not edit or lint it
- Shell code blocks (`bash`/`shell`/`sh`) are extracted and
  validated by shellcheck/shfmt during CI

### Shell Scripts and Bash Code Blocks

- Must pass `shellcheck` (SC2317 is excluded)
- Must pass `shfmt --case-indent --indent 2 --space-redirects`
- Use uppercase for variables: `${MY_VARIABLE}`
- CI shell default: `bash -euxo pipefail {0}`

### JSON / TypeScript / JavaScript

- JSON must pass `jsonlint --comments`
- TS/JS formatted with `prettier --html-whitespace-sensitivity=ignore`

### Hugo Templates (layouts/)

- Go template syntax: `{{ partial "name" . }}`
- Tailwind CSS utility classes; HTML5 doctype

### YAML / TOML Config

- Maintain `# keep-sorted start` / `# keep-sorted end` blocks
- Add inline comments explaining non-obvious settings

## GitHub Actions Workflows

- **Validate** with `actionlint` before committing
- **Permissions**: `permissions: read-all` at workflow level;
  override per-job only as needed
- **Pin actions** to full SHA with version comment:
  `uses: actions/checkout@<sha> # v6.0.2`
- **Shell default**: `bash -euxo pipefail {0}`
- **Timeout**: `timeout-minutes` on every job (5-10 min)
- **Runner**: `ubuntu-24.04-arm` (preferred) or `ubuntu-latest`

## Security Scanning (CI)

Checkov (skip `CKV_GHA_7`), DevSkim (ignore DS162092/DS137138;
exclude `CHANGELOG.md`), KICS (fail on HIGH), Trivy (HIGH/CRITICAL
only, ignore unfixed).

## Version Control

### Commit Messages

- Format: `<type>: <description>` (no scope parentheses)
- Types: `feat`, `fix`, `docs`, `chore`, `refactor`, `test`,
  `style`, `perf`, `ci`, `build`, `revert`
- Imperative mood, lowercase, no trailing period, max 72 chars
- Body lines wrapped at 72 characters
- Reference issues: `Fixes #123`, `Closes #456`

Example: `feat: add automated dependency updates`

### Branching

Follow [Conventional Branch](https://conventional-branch.github.io/)
(`feature/`, `bugfix/`, `hotfix/`, `release/`, `chore/`).
Lowercase, hyphens only, include issue numbers when applicable.

### Pull Requests

- Always create as **draft** initially
- Title follows conventional commit format
- Reference issues with `Fixes`, `Closes`, or `Resolves`

## Link Checking

Config in `lychee.toml` and `.lycheeignore`. Accepts 200/429
status codes. Caches results. Excludes template variables, shell
variables, `CHANGELOG.md`, `package-lock.json`, and private IPs.

## Dependency Management

Renovate (self-hosted): auto-merge minor/patch, manual for major.
Branch prefix: `chore/renovate/`. 3-day minimum release age.

## Quality Checklist

Before pushing, verify:

- [ ] `rumdl .` passes on all Markdown files
- [ ] Shell code blocks pass `shellcheck` and `shfmt`
- [ ] `actionlint` passes on modified workflow files
- [ ] Commit message follows conventional format
- [ ] 2-space indentation, no tabs, lines at 72 chars
- [ ] PR created as draft with semantic title
