# BundleTool Action

[![Codacy Badge](https://api.codacy.com/project/badge/Grade/d78e880d85a947b1826b9bfde90df6bc)](https://app.codacy.com/gh/mukeshsolanki/bundletool-action?utm_source=github.com&utm_medium=referral&utm_content=mukeshsolanki/bundletool-action&utm_campaign=Badge_Grade_Settings)

![Thumbnail](thumbnails.jpeg)

This action will help you convert your aab to signed apk file.

## Inputs

### `aabFile`

**Required:** The relative path in your project where your Android bundle file will be located

### `base64Keystore`

**Required:** The base64 encoded signing key used to sign your apk

This action will directly decode this input to a file to sign your release with. You can prepare your key by running this command on linux systems.

```bash
openssl base64 < some_signing_key.jks | tr -d '\n' | tee some_signing_key.jks.base64.txt
```
Then copy the contents of the `.txt` file to your GH secrets

### `keystoreAlias`

**Required:** The alias of your signing key 

### `keystorePassword`

**Required:** The password to your signing keystore

### `keyPassword`

**Required:** The private key password for your signing keystore

## Outputs
Output variables are set both locally and in environment variables.

### `apkPath`
The path to the single release apk file that have been signed with this action.

## Example usage

### Single APK

The output variable `signedReleaseFile` can be used in a release action.

```yaml
steps:
  - name: Convert aab to apk
    id: convert_aab
    uses: mukeshsolanki/bundletool-action@v1.0.0
    with:
      aabFile: app/build/outputs/bundle/release/app-release.aab
      base64Keystore: ${{ secrets.BASE64_KEY }}
      keystorePassword: ${{ secrets.PASSWORD }}
      keystoreAlias: ${{ secrets.ALIAS }}
      keyPassword: ${{ secrets.PASSWORD }}

  - uses: actions/upload-artifact@v3
    with:
      name: release-apk
      path: ${{ steps.convert_aab.outputs.apkPath }}
```