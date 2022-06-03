# Dynamic Ruby versions action

GitHub Action to build Ruby versions dynamically. You don’t need to create a PR to add a new Ruby version to CI matrixes.

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
        # with:
        #   type: 'cruby' # or 'cruby-jruby', 'cruby-truffleruby', 'all'

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

The case to add outdated versions:

```yaml
steps:
  - id: versions
    uses: ybiquitous/dynamic-ruby-versions-action@v1
    with:
      add: '"2.5", "2.6"' # must be comma-separated strings with double quotes
```
