# get-pages-url

GitHub action to retrieve the GitHub Pages URL for a repository. This action attempts to fetch the URL using the GitHub API and falls back to constructing the URL based on repository naming conventions if the API request fails. No inputs are needed when retrieving the URL for the repository the action is run from.

This is a _composite_ action and hence must be run on a Linux-based runner like `ubuntu-latest` or similar.

Developed and maintained by Research Technology (RT), Tufts Technology Services (TTS), Tufts University.

## Inputs

- `repository`

  - Repository for which to get the GitHub Pages URL. Must be in `owner/repository` format (like `tuftsrt/get-pages-url`). Defaults to the current repository.
  - Default: `${{ github.repository }}`

- `token`

  - GitHub token with `read` permissions for the `pages` scope of specified repository. Defaults to the `GITHUB_TOKEN` secret of the current repository.
  - Default: `${{ github.token }}`

## Outputs

- `url`

  - The GitHub Pages URL for the repository.

## Usage Example

```yaml
name: build-main
on:
  push:
    branches:
      - main
jobs:
  build-main:
    runs-on: ubuntu-latest
    steps:
      - id: get-url
        uses: tuftsrt/get-pages-url@v1
      - uses: tuftsrt/sphinx-to-branch@v1
        env:
          BASEURL: ${{ steps.get-url.outputs.url }}
```

Note how the outputted URL can be passed onto a subsequent step as an environment variable.
