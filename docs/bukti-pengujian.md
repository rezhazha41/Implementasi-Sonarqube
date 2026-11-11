# Bukti Tugas: Static Code Analysis SonarQube

## Identitas

- Proyek yang dianalisis: Perpus Flutter/Supabase.
- Repositori implementasi pipeline: `https://github.com/rezhazha41/Implementasi-Sonarqube`
- Source yang diperiksa workflow: `https://github.com/rezhazha41/apps-perpus`, branch `main`.
- Project key SonarQube: `perpus-mobile-static-analysis`.

## Bukti yang perlu dilampirkan setelah pipeline dijalankan

1. Screenshot tab GitHub Actions yang menunjukkan job berhasil, termasuk langkah analyzer, test, dan scan.
2. Screenshot dashboard SonarQube yang menampilkan nama project, waktu analisis, Quality Gate, jumlah bug/vulnerability/code smell, dan coverage.
3. Jangan tampilkan token, secret, email akun, data siswa, atau konfigurasi server privat pada screenshot.

## Status bukti

| Pemeriksaan | Status | Bukti |
|---|---|---|
| Konfigurasi SonarQube dan workflow ada | PASS | `sonar-project.properties` dan `.github/workflows/sonarqube.yml` |
| Validasi pipeline GitHub Actions | FAIL | Run `42973329729` gagal karena checkout source private tanpa token; run `42973609344` berhenti di preflight konfigurasi. Flutter analyzer, test, dan scan belum berjalan |
| Analisis di server SonarQube | BLOCKED | Secret `PERPUS_SOURCE_TOKEN`/`SONAR_TOKEN` dan variable `SONAR_HOST_URL` belum disetel |
| Screenshot dashboard hasil analisis | NOT RUN | Ambil setelah scan berhasil |

Run pertama membuktikan repository target dan workflow dapat dipicu, tetapi source `apps-perpus` bersifat private sehingga checkout anonim ditolak. Run kedua membuktikan preflight menghentikan pipeline sebelum checkout/test/scan bila konfigurasi belum tersedia.

Untuk membuka blocker, tambahkan secret `PERPUS_SOURCE_TOKEN` dengan izin read-only `Contents` pada repo `apps-perpus`, secret `SONAR_TOKEN`, dan variable `SONAR_HOST_URL`. Setelah itu pilih **Re-run all jobs** pada [workflow run terbaru](https://github.com/rezhazha41/Implementasi-Sonarqube/actions/runs/42973609344), lalu ganti status berdasarkan hasil baru.

Status PASS pada konfigurasi hanya membuktikan file implementasi tersedia; bukan bukti scan SonarQube berhasil.
