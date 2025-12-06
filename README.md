# Run <TOOL> via pre-commit + reviewdog

> Template README — replace `<TOOL>` and `<HOOK_ID>` with your actual tool and pre-commit hook name.

This GitHub Action runs a single [`pre-commit`](https://pre-commit.com/) hook on a diff range and reports results to pull requests
using [reviewdog](https://github.com/reviewdog/reviewdog).

Typical use case:

- Run the `<HOOK_ID>` pre-commit hook (e.g. `actionlint-oneline`)
- Annotate problems directly on the PR diff
- Fail the job if violations are found

## Requirements

- A `.pre-commit-config.yaml` in your repository with the `<HOOK_ID>` hook enabled
- GitHub Actions enabled on the repository
- `secrets.GITHUB_TOKEN` available (default on GitHub-hosted runners)

## Inputs

| Name           | Required | Description                                         |
|----------------|----------|-----------------------------------------------------|
| `from-ref`     | ✅        | Base git ref (e.g. PR base SHA)                     |
| `to-ref`       | ✅        | Head git ref (e.g. PR head SHA)                     |
| `github-token` | ✅        | GitHub token for reviewdog (`secrets.GITHUB_TOKEN`) |

## Outputs

| Name       | Description                                  |
|------------|----------------------------------------------|
| `exitcode` | Exit code of the `<HOOK_ID>` pre-commit hook |

## Usage

In your workflow (example for a pull request):

```yaml
name: Lint with <TOOL>

on:
  pull_request:

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Run <TOOL> via pre-commit + reviewdog
        uses: <OWNER>/<REPO>@v1
        with:
          from-ref: ${{ github.event.pull_request.base.sha }}
          to-ref: ${{ github.event.pull_request.head.sha }}
          github-token: ${{ secrets.GITHUB_TOKEN }}
````

## Versioning

It’s recommended to pin to the major version:

```yaml
uses: <OWNER>/<REPO>@v1
```

For fully reproducible behavior, pin to an exact tag:

```yaml
uses: <OWNER>/<REPO>@v1.0.0
```
