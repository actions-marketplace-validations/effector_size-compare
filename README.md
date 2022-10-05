# size-compare

Compare size changes of your bundle over the time and new Pull Requests.

## Usage

1. [Create new GIST](https://gist.github.com/) and add `README.md` file with any content.
2. Add ID of the gist into Secrets as `SIZE_HISTORY_GIST_ID`.
3. [Create personal access token](https://github.com/settings/tokens) with `gist` permission. **Copy the token**.
4. Add the token into Secrets as `SIZE_COMPARE_TOKEN`.
5. Create new Workflow file `.github/workflows/size-compare.yml`:

```yaml
name: SizeCompare CI

on:
  push:
    branches:
      - main
  pull_request:
    types: [opened, reopened, synchronize, ready_for_review]
  workflow_dispatch:

jobs:
  size-compare:
    runs-on: ubuntu-latest
    steps:
      - name: 🛎️ Checkout
        uses: actions/checkout@v3

      # Add here your setup, installation, and build steps

      - name: 🚛 Size compare
        uses: effector/size-compare@v1.0.0
        with:
          gist_id: ${{ secrets.SIZE_HISTORY_GIST_ID }}
          gist_token: ${{ secrets.SIZE_COMPARE_TOKEN }}
          github_token: ${{ secrets.GITHUB_TOKEN }}
          files: |
            dist/**.js
            !dist/**.js.map
```

## Inputs

| Name           | Required | Description                                                                                                                    |
| -------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------ |
| `gist_id`      | Yes      | Identifier of the GIST to save size history to                                                                                 |
| `gist_token`   | Yes      | Personal Access Token to read/write hist                                                                                       |
| `github_token` | No       | Token used to post comments for PRs. If not passed, comments posted from the `gist_token` owner name                           |
| `files`        | Yes      | List of [glob patterns](https://github.com/actions/toolkit/tree/main/packages/glob#patterns) of files must be checked for size |
