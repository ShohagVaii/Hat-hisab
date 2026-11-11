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

Current archive: **Fin Track v1.7.1+24** (Arena-fix batch — money line-total
integer arithmetic fixing a silent ৳1 floor error, account-scoped sync pull
watermarks with (updated_at, id) pagination, honest sync status, two-device
conflict protection with conditional (CAS) push writes, backup export scoped
to the exporting account with a foreign-backup import warning, note-editor
autosave/delete serialization fixing duplicate notes, and pulled-row dedupe
aliases so merged wallets/categories no longer orphan child rows. App source
only; the established two-digit part names are kept.)

The workflow `.github/workflows/build-apk.yml` builds the release APK on
every push to `main` (and from the Actions tab with "Run workflow"). It runs
the tests first, then publishes the APK to the repository's **Releases**
as `hat-hisab-v1.7.1.apk` under tag `v1.7.1`.
