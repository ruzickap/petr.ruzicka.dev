# Petr Ruzicka's Personal Website (petr.ruzicka.dev)

This repository contains the source code for Petr Ruzicka's personal website
(petr.ruzicka.dev), built with Hugo and the LucentLink-Hugo theme. The website
showcases his work as a DevOps specialist and SysAdmin, as well as his
photography.

[![Build Status](https://github.com/ruzickap/petr.ruzicka.dev/actions/workflows/gh-pages-build.yml/badge.svg)](https://github.com/ruzickap/petr.ruzicka.dev/actions/workflows/gh-pages-build.yml)

## Live Site

You can visit the live site at [https://petr.ruzicka.dev/](https://petr.ruzicka.dev/).

## Getting Started

To get a local copy up and running, follow these simple steps.

### Prerequisites

Make sure you have Git installed on your system.

### Cloning

1. Clone the repo:

   ```bash
   git clone --recurse-submodules https://github.com/ruzickap/petr.ruzicka.dev.git
   ```

2. Navigate to the project directory:

   ```bash
   cd petr.ruzicka.dev || exit
   ```

## Theme

This site uses the [LucentLink-Hugo](https://github.com/cx48/LucentLink-Hugo)
theme for Hugo.

## Installing Hugo

Hugo is required to build and run this website. You can install it as follows:

### Mac

```bash
brew update && brew install hugo
```

Just download binary from [https://github.com/gohugoio/hugo/releases](https://github.com/gohugoio/hugo/releases)

## Running Locally

Once you have cloned the repository and installed Hugo, you can run the site locally.

- Run local web server:

  ```bash
  hugo server --buildDrafts
  ```

- Build the site like CI does (output goes to `public/`):

  ```bash
  hugo build --minify --printPathWarnings --printI18nWarnings --panicOnWarning
  ```
