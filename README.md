# Bump workflow

Increment the version in `package.json` at the root of the current repository.

* runner: `ubuntu-latest`
* trigger: `workflow_call` only
* inputs: `mode`: patch, minor, major
* outputs: `version`: bumped version
   
# Create Release workflow

Create a new release for the current repository.

* runner: `ubuntu-latest`
* trigger: `workflow_call` only
* inputs: `version`: release name, tag name
* outputs: `upload_url`: the url to upload assets 

# Get Tool

Cross-platfrom workflow to download `tool4d` from 4D.com.

* runner: `macos` `windows` `self-hosted`
* trigger: `workflow_call` only
* inputs: see below
* outputs: `tool4d_download_url` `tool4d_executable_path`

e.g.

```yml
jobs:

  get-tool:
    uses: miyako/4D/.github/workflows/get-tool.yml@v1
    with:
      platform: macos
      labels: "[\"macos\", \"self-hosted\"]"
      branch: 20.x
      version: 20.2
      build: 100959
```

## Remarks

`curl` on Windows seem to block on the default `pwsh`. Using `shell: bash` instead.
