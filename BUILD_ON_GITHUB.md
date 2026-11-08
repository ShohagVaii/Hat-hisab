# Building the APK on GitHub Actions

> Repo layout note: the project itself lives in `archive/` as a split ZIP
> (`hat-hisab-app.zip.part-00` … `part-13`) because the upload connector
> cannot push a whole folder tree at once. The workflow joins the parts,
> unpacks the project, and fetches the Hind Siliguri fonts from Google
> Fonts (they are the only files not inside the archive). Everything else,
> including the Inter fonts and launcher icons, is in the archive byte for
> byte. To restore the project locally, download all parts, join them
> (`cat hat-hisab-app.zip.part-* | base64 -d > hat-hisab-app.zip`) and unzip.
> Note: each part file holds base64 text (the upload path stores text), which
> is why the decode step is needed.

Current archive: **Fin Track v1.5.10+21** (third run of the same archive —
the first two v1.5.10 runs failed at the "Build release APK" step because
the runner could not download the Gradle 8.7 distribution zip; tests
passed both times and the app source is unchanged). (Phase 5: account password change
in Settings → Profile — current-password re-authentication via Supabase,
then updateUser; clearly separated from the app PIN, with logged-out and
network-error states in Bengali).

The workflow `.github/workflows/build-apk.yml` builds the release APK on
every push to `main` (and from the Actions tab with "Run workflow"). It runs
the tests first, then publishes the APK to the repository's **Releases**
as `hat-hisab-v1.5.10.apk` under tag `v1.5.10`.
