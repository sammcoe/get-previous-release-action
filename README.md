# get-previous-release-action

A Github Action for getting the previous (the one BEFORE the most recent) release tag from git and outputting that for use in other actions

## Outputs

### tag

The release tag of the previous release

## Example usage

### Basic

```yml
- name: Get previous release tag
  id: previous-release
  uses: actions/get-previous-release-action@v1
```

### Updating the most recent release

```yml
update_release:
  name: Update Release
  runs-on: ubuntu-latest
  steps:
    - uses: actions/checkout@v2
      with:
        ref: main
        fetch-depth:
    - uses: rlespinasse/github-slug-action@v3.x
    - name: Get previous release tag
      id: previous-release
      uses: sammcoe/get-previous-release-action@v1
    - name: Generate Changelog
      id: changelog
      uses: metcalfc/changelog-generator@v1.0.0
      with:
        mytoken: ${{ secrets.GITHUB_TOKEN }}
        base-ref: ${{ steps.previous-release.outputs.tag }}
    - name: Updating Release
      uses: ncipollo/release-action@v1.8.6
      with:
        body: |
          Changes in this Release: 
          ${{ steps.changelog.outputs.changelog }}

        token: ${{ secrets.GITHUB_TOKEN }}
        name: Release ${{ env.GITHUB_REF_SLUG }}
        allowUpdates: true
```
