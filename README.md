# Dynamic Ruby versions action

GitHub Action to build Ruby versions dynamically.

## Usage

The simplest case:

```yaml
jobs:
  ruby-versions:
    runs-on: ubuntu-latest
    outputs:
      versions: ${{ steps.versions.outputs.versions }}
    steps:
      - id: versions
        uses: ybiquitous/dynamic-ruby-versions-action@v1
  test:
    needs: ruby-versions
    runs-on: ubuntu-latest
    strategy:
      matrix:
        ruby: ${{ fromJson(needs.ruby-versions.outputs.versions) }}
    steps:
      - uses: actions/checkout@v3
      - uses: ruby/setup-ruby@v1
        with:
          ruby-version: ${{ matrix.ruby }}
      # more steps...
```
