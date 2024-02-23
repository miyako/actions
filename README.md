# Bump workflow

Increment the version in `package.json` at the root of the current repository.

* runner: `ubuntu-latest`
* trigger: `workflow_call` only
* inputs: `mode`: patch, minor, major
* outputs: `version`: bumped version

## Note

The version information from `package.json` is used by the [`compiler`](https://github.com/miyako/4d-class-compiler) project when `localbuild` is specified.
   
# Create Release workflow

Create a new release for the current repository.

* runner: `ubuntu-latest`
* trigger: `workflow_call` only
* inputs: `version`: release name, tag name
* outputs: `upload_url`: the url to upload assets 

# Get Tool action

Cross-platfrom workflow to download `tool4d` from 4D.com.

* runner: `macos` `windows` `self-hosted`
* trigger: `workflow_call` only
* inputs: see below
* outputs: `tool4d_download_url` `tool4d_executable_path`

e.g.

```yml
jobs:

  test:
    runs-on: [macos-latest]
    steps:
      - name: get
        uses: miyako/4D/.github/actions/get-tool@v1
        with:
          platform: macos
          branch: 20.x
          version: 20.2
          build: 100959
```

## Remarks

`curl` on Windows seem to block on the default `pwsh`. Using `shell: bash` instead.
