# coolify

Reusable GitHub Actions workflows for deploying to the Coolify instance behind `coolify.codersclanil.com`.

## deploy-git

Pins the commit on a Git-built Coolify application through the API, deploys it and waits until the deployment finishes.

Caller, in the app repository (`.github/workflows/deploy.yml`):

```yaml
name: deploy

on:
  push:
    branches: [main]
  workflow_dispatch:
    inputs:
      commit:
        description: 'Full commit sha to roll back to. Empty deploys the branch head.'
        type: string
        default: ''

jobs:
  deploy:
    uses: coders-clan/coolify/.github/workflows/deploy-git.yml@v1
    with:
      commit: ${{ inputs.commit }}
    secrets: inherit
```

Set the secrets once at organisation level (Settings, Secrets and variables, Actions, visibility All repositories) and every repo in coders-clan gets them without any setup. A new app repo then only needs the caller file above and the variable `APP_UUID`.

The secrets and variables read: secrets `COOLIFY_TOKEN` (Read, Write, Deploy), `CF_ACCESS_CLIENT_ID`, `CF_ACCESS_CLIENT_SECRET`, `COOLIFY_URL` (secret or variable), and the variable `APP_UUID` (or pass `app-uuid`).

Inputs: `app-uuid`, `commit`, `environment` (default `production`), `timeout-minutes` (default 40).

Rollback is a manual run with `commit` set to an earlier full sha.
