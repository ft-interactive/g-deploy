# ft-graphics-deploy [![Build Status][circle-image]][circle-url] [![npm](https://img.shields.io/npm/v/ft-graphics-deploy.svg)](https://npmjs.com/package/ft-graphics-deploy)

CLI for deploying built static websites to an S3 bucket.

## How to use

Requires Node 7.10 or higher.

### Command line usage

```
$ ft-graphics-deploy --help

  CLI for deploying FT Graphics projects

  > ft-graphics-deploy [FLAGS...]
  ────────────────────────────────────────────────────────────────────
  All flags are optional when this command is run from a typical FT
  Graphics project repo in CI.
  ────────────────────────────────────────────────────────────────────
  Vault settings
  If not provided these will be inferred from environment variables following
  each flag.
  --vault-endpoint ($VAULT_ENDPOINT)
  --vault-role ($VAULT_ROLE)
  --vault-secret ($VAULT_SECRET)
  --vault-secret-path ($VAULT_SECRET_PATH)

  AWS settings (Deprecated for FT projects — use Vault instead)
  If not provided, these settings are taken from env vars
  ("AWS_KEY_PROD", "AWS_SECRET_PROD", etc.)
    --aws-key
    --aws-secret
    --aws-region
    --bucket-name

  Upload settings
  If not provided, these are deduced from the git status in the CWD.
    --project-name
    --sha - unique reference for this commit
    --branch-name - name of the branch you are deploying
    --local-dir - what to upload; defaults to ./dist
    --preview - upload files to preview folder
    --assets-prefix - base for asset URLs; affects the rev-manifest and all
                      HTML/CSS files

  Other
    --help - show this help and exit
    --get-branch-url - instead of deploying, just print the URL it would deploy to
    --get-commit-url - as above, but get the commit-specific URL
    --confirm - skip the confirmation dialogue when deploying
```

## JavaScript API

The most straightforward way:

```js
import deploy from 'ft-graphics-deploy';

deploy(options).then(baseURLs => {
  console.log('uploaded to:', baseURLs);
});
```

For more fine-grained control:

```js
import { Deployer } from 'ft-graphics-deploy';

const deployer = new Deployer(options);

deployer.execute().then(baseURLs => {
  console.log('uploaded to:', baseURLs);
});
```

The JavaScript API does **not** do any git-sniffing or use any environment variables to configure the deployment – you must pass in all required options manually. See the [Deployer class](./src/Deployer.js) source for the full options.

## Development

Clone this repo and run `yarn` to install dependencies.

Add a `.env` file that defines `AWS_KEY_DEV`, `AWS_SECRET_DEV`, `AWS_REGION_DEV` and `BUCKET_NAME_DEV`. (These are used in tests.)

Run `yarn build -- --watch` and `yarn test -- --watch` in separate terminal tabs while developing. (The first one watches `src` and builds to `dist`. The second one runs ava tests in `dist`.)

### Publishing a new version to npm

* Make sure you're on master: `git checkout master`
* Update the version: `npm version patch` (or replace `patch` with `minor` or `major` as appropriate)
  * This updates package.json, commits this single-line change, and creates a new git tag
* Push to GitHub: `git push && git push --tags`

CircleCI will do the rest.

<!-- badge URLs -->

[circle-url]: https://circleci.com/gh/ft-interactive/ft-graphics-deploy
[circle-image]: https://circleci.com/gh/ft-interactive/ft-graphics-deploy.svg?style=svg
