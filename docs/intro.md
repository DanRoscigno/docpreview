---
sidebar_position: 1
---

## Overview

This is a tiny example of how to use Vercel for PR previews with a Docusaurus site that requires outside files.  Vercel's automatic build process for Docusaurus clones a repo and runs yarn build.  Our usecase requires grabbing files from a second repo and putting them into place.  In mid-2022 Vercel added support for running their build process via CI.  By using this, we can add a step to clone the second repo (or grab a tarfile), unack it, and copy the files into place.  The example workflow in this repo grabs ClickHouse/ClickHouse and copies one markdown file into the demo Docusaurus site and then continues with Vercel's typical build.

## Setup in your Docusaurus source repo
- vercel login
- vercel link
- copy the **preview** example CI job from [Vercel Examples GitHub Actions](https://github.com/vercel/examples/blob/main/ci-cd/github-actions/.github/workflows/preview.yaml)
- Add in the needed step(s) to the **preview** example CI job. In the listing below just the `Download Reference Docs` step is added.  Note that this is an abbreviated copy of our normal reference docs download, I am just copying in one file (syntax.md):
```yaml
name: GitHub Actions Vercel Preview Deployment
env:
  VERCEL_ORG_ID: ${{ secrets.VERCEL_ORG_ID }}
  VERCEL_PROJECT_ID: ${{ secrets.VERCEL_PROJECT_ID }}
on:
  push:
    branches-ignore:
      - main
jobs:
  Deploy-Preview:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Install Vercel CLI
        run: npm install --global vercel@canary
      - name: Pull Vercel Environment Information
        run: vercel pull --yes --environment=preview --token=${{ secrets.VERCEL_TOKEN }}
      - name: Download Reference Doc
        run:  |
          curl https://codeload.github.com/ClickHouse/ClickHouse/tar.gz/master | tar -xz -C ./ --strip=2 "ClickHouse-master/docs/"
          cp ./en/sql-reference/syntax.md docs/
      - name: Build Project Artifacts
        run: vercel build --token=${{ secrets.VERCEL_TOKEN }}
      - name: Deploy Project Artifacts to Vercel
        run: vercel deploy --prebuilt --token=${{ secrets.VERCEL_TOKEN }} --debug
```
- Add secrets to the repo for VERCEL_TOKEN, VERCEL_ORG_ID, VERCEL_PROJECT_ID.  The token gets generated in the Vercel website (maybe their is a CLI for this, I did not check).  The ORG and PROJECT IDs are generated for you and placed in .vercel/project.json during the earlier `vercel link` step.

Note: I do not think that there is any setup to be done in Vercel, I think that `vercel link` creates the project for you, when I test this I will come back and edit.
## Setup in Vercel
- Create a project.  This project does not need to be associated with a GitHub repo, the GitHub action will communicate with Vercel when needed.
- Set the Framework Preset to Docusaurus 2
- Set the Node.js version to 18.x


Settings:
## Docusaurus readme is below

## Website

This website is built using [Docusaurus 2](https://docusaurus.io/), a modern static website generator.

### Installation

```
$ yarn
```

### Local Development

```
$ yarn start
```

This command starts a local development server and opens up a browser window. Most changes are reflected live without having to restart the server.

### Build

```
$ yarn build
```

This command generates static content into the `build` directory and can be served using any static contents hosting service.

### Deployment

Using SSH:

```
$ USE_SSH=true yarn deploy
```

Not using SSH:

```
$ GIT_USER=<Your GitHub username> yarn deploy
```

If you are using GitHub pages for hosting, this command is a convenient way to build the website and push to the `gh-pages` branch.
