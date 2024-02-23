# Bump

Increment the version in `package.json` at the root of the current repository.

* runner: `ubuntu-latest`
* trigger: `workflow_call` only
* inputs: `mode`: patch, minor, major
* outputs: `version`: bumped version
   
# Create Release

Create a new release for the current repository.

* runner: `ubuntu-latest`
* trigger: `workflow_call` only
* inputs: `version`: release name, tag name
* outputs: `upload_url`: the url to upload assets 

# Get Tool

Cross-platfrom workflow to download `tool4d` from 4D.com.

## Remarks

`curl` on Windows seem to block on the default `pwsh`. Using `shell: bash` instead.

## Usage

Use `matrix` to run the same code for Mac and Windows.

e.g.

```yml
jobs:

  get:
    strategy:
      matrix:
        TOOL4D_PLATFORM: ["windows-latest", "macos-latest"]
        TOOL4D_BRANCH: [20.x]
        TOOL4D_VERSION: [20.2]
        TOOL4D_BUILD: [latest] 
      # env is unavailable in a strategy
    uses: miyako/4D/.github/workflows/get-tool.yml@v1
    with:
      platform: ${{ matrix.TOOL4D_PLATFORM }}
      labels: ${{ toJSON(matrix.TOOL4D_PLATFORM) }} # must be JSON
      branch: ${{ matrix.TOOL4D_BRANCH }}
      version: ${{ matrix.TOOL4D_VERSION }}
      build: ${{ matrix.TOOL4D_BUILD }}
```

Note that `env` is unavailable in a `strategy`. Use `matrix` instead.
