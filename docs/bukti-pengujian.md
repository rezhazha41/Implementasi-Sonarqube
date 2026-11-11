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
| Validasi pipeline GitHub Actions | FAIL | Run `42973329729` gagal pada checkout source private karena `PERPUS_SOURCE_TOKEN` belum disetel; Flutter analyzer, test, dan scan belum berjalan |
| Analisis di server SonarQube | BLOCKED | URL server dan token project belum tersedia di GitHub Actions |
| Screenshot dashboard hasil analisis | NOT RUN | Ambil setelah scan berhasil |

Untuk membuka blocker, tambahkan secret `PERPUS_SOURCE_TOKEN` dengan izin read-only `Contents` pada repo `apps-perpus`, secret `SONAR_TOKEN`, dan variable `SONAR_HOST_URL`. Setelah itu jalankan ulang workflow dan ganti status berdasarkan run baru.

Status PASS pada konfigurasi hanya membuktikan file implementasi tersedia; bukan bukti scan SonarQube berhasil.
