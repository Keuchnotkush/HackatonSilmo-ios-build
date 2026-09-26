# HackatonSilmo iOS build

This public repo exists only to build the private
[youkill081/HackatonSilmo](https://github.com/youkill081/HackatonSilmo) KMP app for iOS
on GitHub's macOS runners and upload it to TestFlight. It contains **no app code and no
secrets**. The source is checked out at build time with a read-only deploy key, and
everything sensitive is stored as an Actions secret.

## Run a build

**Actions → iOS → TestFlight → Run workflow**, then choose the branch to build (default
`ios`). After Apple finishes processing the upload (about 5-15 minutes), install the
build from the TestFlight app on your iPhone.

## Secrets (Settings → Secrets and variables → Actions)

| Secret | What it is |
|---|---|
| `SOURCE_DEPLOY_KEY` | Private half of a read-only deploy key on youkill081/HackatonSilmo |
| `GPR_TOKEN` | Classic GitHub PAT with only `read:packages` (downloads the Maverick SDK) |
| `SDK_KEY_B64` | Everysight `sdk.key`, base64-encoded |
| `APPLE_TEAM_ID` | 10-character Apple Developer Team ID |
| `ASC_KEY_ID` | App Store Connect API key ID |
| `ASC_ISSUER_ID` | App Store Connect API issuer ID |
| `ASC_KEY_P8_B64` | The API key's `.p8` file, base64-encoded |

## Safety rules for this repo

- The workflow runs only when triggered manually (`workflow_dispatch`), which requires
  write access to this repo. Pull requests from forks never receive secrets.
- Never add an `upload-artifact` step: the `.ipa` contains `sdk.key`, and anyone can
  download artifacts from a public repo.
- Never `echo` a secret, including a decoded or transformed one. Build logs are public.
