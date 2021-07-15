# API Docs

Repo for the Fast-Weigh API docs.

https://docs.fast-weigh.dev

## Build tooling

This repo uses the [Retype Static Site Generator](https://retype.com) to build the docs from mardown files.

Please reference the Retype docs & config for proper formatting and yml front-matter.

## Hosting

Pushing to the master branch triggers a build action that pushes the static site to Azure Static Site Hosting.

## How to contribute

You'll need Node & NPM (ships with Node install) to develop locally.

Local dev and build scripts can be found in the package.json file.

```npm start``` -> local dev
```npm run build``` -> test the build output

Pushing to the master branch runs the build command so there is no need to commit the output folder.