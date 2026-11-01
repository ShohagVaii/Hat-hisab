# Building the APK on GitHub Actions

> Repo layout note: the project itself lives in `archive/` as a split ZIP
> (`hat-hisab-app.zip.part-00` … `part-11`) because the upload connector
> cannot push a whole folder tree at once. The workflow joins the parts,
> unpacks the project, and fetches the Hind Siliguri fonts from Google
> Fonts (they are the only files not inside the archive). Everything else,
> including the Inter fonts and launcher icons, is in the archive byte for
> byte. To restore the project locally, download all parts, join them
> (`cat hat-hisab-app.zip.part-* | base64 -d > hat-hisab-app.zip`) and unzip.
> Note: each part file holds base64 text (the upload path stores text), which
> is why the decode step is needed.

Current archive: **FinTrack হাট হিসাব v1.4.1+9** (fix-all release: sync pull
root-cause fix, docs/08 account flows, finance budget/goal sync, PDF redesign
with CREDIT: SHOHAG VAII + download QR, UI fixes).

The workflow `.github/workflows/build-apk.yml` builds the release APK on
every push to `main` (and from the Actions tab with "Run workflow"). It runs
the tests first, then publishes the APK to the repository's **Releases**
page, so it can be downloaded and installed directly.

## One-time setup: signing secrets

The release keystore is never committed. Add these 4 repository secrets
under Settings → Secrets and variables → Actions → New repository secret:

| Secret name | Value |
|---|---|
| `KEYSTORE_B64` | The release keystore file, base64-encoded (see below) |
| `KEYSTORE_PASSWORD` | The keystore (store) password |
| `KEY_ALIAS` | The key alias inside the keystore |
| `KEY_PASSWORD` | The key password |

The passwords and alias are in the `keystore.properties` kept with the
signing-key backup (`hat-hisab-signing-key-SEPARATE.zip`). Until these
secrets exist, the workflow stops at the "Check signing secrets" step —
that failure is expected on the first run.

To base64-encode the keystore on Android (Termux):

```bash
base64 -w 0 hat-hisab-release.keystore > keystore.b64.txt
```

Paste the whole content of `keystore.b64.txt` as the `KEYSTORE_B64` value.
