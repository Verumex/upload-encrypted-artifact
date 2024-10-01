# `@verumex/upload-encrypted-artifact`

Upload encrypted artifact from your workflow runs. Internally powered by [@actions/upload-artifact](https://github.com/actions/upload-artifact).
The uploaded artifact will be encrypted using provided GPG keys. The provided
keys will be used as recipients for the artifact.

See also [download-encrypted-artifact](https://github.com/Verumex/download-encrypted-artifact)

- [`@verumex/upload-encrypted-artifact`](#)
  - [Usage](#usage)
    - [Inputs](#inputs)

  - [Examples](#examples)
    - [Upload an individual file](#upload-an-individual-file)
    - [Upload using Multiple Paths and Exclusions](#upload-using-multiple-paths-and-exclusions)

## Usage

### Inputs

```yaml
- uses: Verumex/upload-encrypted-artifact@v1
  with:
    # A single, or list of public GPG keys of the recipients. Used to encrypt
    # the artifact
    # Required
    gpg_key:

    # Name of the artifact to upload.
    # Optional. Default is 'artifact'
    name:

    # A file, directory or wildcard pattern that describes what to upload
    # Required.
    path:

    # The desired behavior if no files are found using the provided path.
    # Available Options:
    #   warn: Output a warning but do not fail the action
    #   error: Fail the action with an error message
    #   ignore: Do not output any warnings or errors, the action does not fail
    # Optional. Default is 'warn'
    if-no-files-found:

    # Duration after which artifact will expire in days. 0 means using default retention.
    # Minimum 1 day.
    # Maximum 90 days unless changed from the repository settings page.
    # Optional. Defaults to repository settings.
    retention-days:

    # The level of compression for Zlib to be applied to the artifact archive.
    # The value can range from 0 to 9.
    # For large files that are not easily compressed, a value of 0 is recommended for significantly faster uploads.
    # Optional. Default is '6'
    compression-level:

    # If true, an artifact with a matching name will be deleted before a new one is uploaded.
    # If false, the action will fail if an artifact for the given name already exists.
    # Does not fail if the artifact does not exist.
    # Optional. Default is 'false'
    overwrite:

    # Whether to include hidden files in the provided path in the artifact
    # The file contents of any hidden files in the path should be validated before
    # enabled this to avoid uploading sensitive information.
    # Optional. Default is 'false'
    include-hidden-files:
```

## Examples

### Upload an Individual File

```yaml
steps:
  - run: mkdir -p path/to/artifact
  - run: echo hello > path/to/artifact/world.txt
  - uses: Verumex/upload-encrypted-artifact@v1
    with:
      gpg_key: ${{ vars.GPG_KEY }}
      name: my-artifact
      path: path/to/artifact/world.txt
```

### Upload using Multiple Paths and Exclusions

```yaml
- uses: actions/upload-encrypted-artifact@v1
  with:
    gpg_key: |
      ${{ vars.GPG_KEY1 }}
      ${{ secrets.GPG_RECIPIENT_KEY }}
    name: my-artifact
    path: |
      path/output/bin/
      path/output/test-results
      !path/**/*.tmp
```
