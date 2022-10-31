# GitHub Action to Help Getting The Latest Release Info

Add this step in your workflow file
```yaml
- name: Gets latest created release info
  id: latest_release_info
  uses: jossef/action-latest-release-info@v1.2.1
  env:
    GITHUB_TOKEN: ${{ github.token }}
```

### Output Variables

- `id`: The Id of the release
- `html_url`: The URL users can navigate to in order to view the release
- `upload_url`: The URL for uploading assets to the release
- `tag_name`: The name of the tag linked with the release
- `name`: The name of the release
- `created_at`: The creation date of the release
- `draft`: The draft of the release

output variables can be accessed after the step is completed via `${{ steps.latest_release_info.outputs.<variable name> }}`


#### Example - Accessing Output Variables


```yaml
name: Build and Release

on:
  push:
    branches: [ master ]
  pull_request:
    branches: [ master ]

env:
  GITHUB_TOKEN: ${{ github.token }}

jobs:
  build:
    name: Build and Release
    runs-on: ubuntu-latest
    steps:
    - name: Checkout code
    - uses: actions/checkout@v2

    - name: Build
    # TODO fill in with a step that builds something into ./dist/output.tar

    - name: Automatic release
    # TODO fill in with a step that automates the release process (i'm using semantic releaser)

    - name: Gets latest created release info
      id: latest_release_info
      uses: jossef/action-latest-release-info@v1.2.1

    - name: Upload asset to github release page
      id: upload-release-asset
      uses: actions/upload-release-asset@v1
      with:
        upload_url: ${{ steps.latest_release_info.outputs.upload_url }}
        asset_path: ./dist/output.tar
        asset_name: output_${{ steps.latest_release_info.outputs.tag_name }}.tar
        asset_content_type: application/x-tar

```


## Credits
This repo was forked and modified. original - https://github.com/marketplace/actions/get-the-upload-url-for-a-release
