# [Bump](https://github.com/miyako/4D/blob/v1/.github/workflows/bump.yml) workflow

Increment the version in `package.json` at the root of the current repository.

* runner: `ubuntu-latest`
* trigger: `workflow_call` only
* inputs: `mode`: patch, minor, major
* outputs: `version`: bumped version

## Note

The version information from `package.json` is used by the [`compiler`](https://github.com/miyako/4d-class-compiler) project when `localbuild` is specified.
   
# [Create Release](https://github.com/miyako/4D/blob/v1/.github/workflows/create-release.yml) workflow

Create a new release for the current repository.

* runner: `ubuntu-latest`
* trigger: `workflow_call` only
* inputs: `version`: release name, tag name
* outputs: `upload_url`: the url to upload assets 

# [Get Tool](https://github.com/miyako/4D/blob/v1/.github/actions/get-tool/action.yml) action

Cross-platfrom workflow to download `tool4d` from 4D.com.

* runner: `macos` `windows` `self-hosted`
* trigger: `workflow_call` only
* inputs: see below
* outputs: `tool4d_download_url` `tool4d_executable_path`

## Note

You should checkout your repository before running this action.

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

e.g. with a `strategy`

```yml
jobs:     

  get:
    strategy:
      fail-fast: false
      matrix:
        TOOL4D_PLATFORM: ["windows-latest", "macos-latest"]
        TOOL4D_BRANCH: [20.x]
        TOOL4D_VERSION: [20.2]
        TOOL4D_BUILD: [latest] 
    runs-on: ${{ matrix.TOOL4D_PLATFORM }}
    steps:
    
      - name: get tool4d
        id: get
        uses: miyako/4D/.github/actions/get-tool@v1
        with:
          platform: ${{ matrix.TOOL4D_PLATFORM }}
          branch: ${{ matrix.TOOL4D_BRANCH }}
          version: ${{ matrix.TOOL4D_VERSION }}
          build: ${{ matrix.TOOL4D_BUILD }}
```

## Remarks

`curl` on Windows seem to block on the default `pwsh`. Using `shell: bash` instead.

# [Build macOS](https://github.com/miyako/4D/blob/v1/.github/actions/build-macos/action.yml) action

runner: `[macos, self-hosted]`
trigger: workflow_call only
outputs: `asset_path` `asset_name` `upload_url`

Compile, build, archive, sign, notarise, staple, macOS product on self-hosted runner and the [`compiler`](https://github.com/miyako/4d-class-compiler) project.

The created asset should be uploaded to GitHub by the caller workflow with its own `secrets.GITHUB_TOKEN`.

e.g.

https://github.com/miyako/4d-topic-cicd/blob/main/.github/workflows/publish.yml
