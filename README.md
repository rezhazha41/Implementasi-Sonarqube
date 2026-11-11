# Implementasi SonarQube untuk Perpus

Repositori ini menyimpan konfigurasi analisis. Source aplikasi tetap berada di repo private [`rezhazha41/apps-perpus`](https://github.com/rezhazha41/apps-perpus); workflow mengambil branch `main` saat dijalankan, lalu menjalankan Flutter analyzer, test dengan coverage LCOV, dan SonarQube. Workflow tidak menyalin source aplikasi ke repo tugas publik.

## Yang dianalisis

- Kode aplikasi: `apps/mobile/lib`
- Package lokal: `packages/data_access/lib`, `packages/domain/lib`, dan `packages/ui_kit/lib`
- Test aplikasi dan data access: `apps/mobile/test` dan `packages/data_access/test`
- File hasil generate, cache, dan build dikecualikan dari temuan.
- Coverage LCOV dibaca dari `apps/mobile/coverage/lcov.info`.

## Prasyarat SonarQube

1. Siapkan SonarQube Server yang dapat dijangkau GitHub-hosted runner dan memiliki analyzer Dart aktif serta mendukung sintaks Dart yang dipakai source Perpus.
2. Buat project dengan project key `perpus-mobile-static-analysis`.
3. Buat token analisis khusus project tersebut.
4. Pada repository ini, tambahkan:
   - Actions variable `SONAR_HOST_URL` berisi URL server SonarQube, tanpa slash di akhir.
   - Actions secret `SONAR_TOKEN` berisi token project.
5. Tambahkan Actions secret `PERPUS_SOURCE_TOKEN` berupa fine-grained token read-only yang hanya memiliki akses **Contents: read** ke repo private `apps-perpus`.
6. Push ke `main` atau jalankan workflow **Flutter static analysis (SonarQube)** secara manual dari tab Actions.

> **Penting:** simpan `PERPUS_SOURCE_TOKEN` dan `SONAR_TOKEN` hanya sebagai GitHub Actions Secrets; jangan menaruh token di file, commit, workflow input, atau log. Jangan gunakan service-role key Supabase sebagai token SonarQube.

## Cara membaca hasil

Run Actions harus melewati Flutter analyzer, suite test, dan langkah **Scan with SonarQube**. Setelah itu buka dashboard project di server SonarQube. Tinjau Bugs, Vulnerabilities, Security Hotspots, Code Smells, coverage, dan Quality Gate. Workflow gagal jika URL atau token belum disetel; kegagalan konfigurasi bukan hasil analisis kode.

SonarQube melengkapi, bukan menggantikan, `flutter analyze` dan test. Source Perpus saat ini mendeklarasikan Dart `^3.13.0`. [Dokumentasi bahasa Dart SonarQube](https://docs.sonarsource.com/sonarqube-server/analyzing-source-code/languages/dart) yang diperiksa mencantumkan dukungan penuh sampai Dart 3.8; karena itu kompatibilitas analyzer terhadap sintaks 3.13 harus dipastikan sebelum hasil scan dipakai sebagai penilaian final. Jika scanner melaporkan sintaks yang belum didukung, perbarui analyzer SonarQube sebelum menafsirkan temuan.

## Berkas implementasi

- [`sonar-project.properties`](sonar-project.properties): scope source/test, exclusions, dan lokasi coverage.
- [`.github/workflows/sonarqube.yml`](.github/workflows/sonarqube.yml): pipeline GitHub Actions.
- [`docs/bukti-pengujian.md`](docs/bukti-pengujian.md): status bukti dan panduan lampiran tugas.
