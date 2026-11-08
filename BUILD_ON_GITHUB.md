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

Current archive: **Fin Track v1.6.0+22** (third run — the second run
unpacked and passed tests but the runner again failed to download the
Gradle 8.7 distribution zip at the APK step; source unchanged)
(re-push with the established two-digit part names — the first v1.6.0 push accidentally used three-digit
part names, leaving two interleaved part sets in archive/ and breaking
the workflow's unpack step; this push restores the two-digit set and
empties the stray three-digit files in the same commit)
(Phase 6: feature batch
in Settings → Profile — current-password re-authentication via Supabase,
then updateUser; clearly separated from the app PIN, with logged-out and
network-error states in Bengali).

The workflow `.github/workflows/build-apk.yml` builds the release APK on
every push to `main` (and from the Actions tab with "Run workflow"). It runs
the tests first, then publishes the APK to the repository's **Releases**
as `hat-hisab-v1.5.10.apk` under tag `v1.5.10`.
