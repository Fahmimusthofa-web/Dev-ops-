# Blameless Postmortem - Insiden Kegagalan Deployment Manual

## Ringkasan Insiden

Pada simulasi serah-terima Minggu 1, tim Operations tidak dapat menjalankan aplikasi
Sentra di lingkungan yang bersih hanya dengan berpegang pada `HANDOVER.md`.
Aplikasi baru berhasil berjalan setelah dependensi dan lingkungan Python dilengkapi
secara manual. [ISI: status akhir simulasi - berhasil / gagal dalam 15 menit]

## Kronologi (timeline)

[ISI dari Lembar Kerja JOB 2: waktu mulai, galat pertama beserta pesannya,
setiap percobaan perbaikan, dan waktu selesai.]

## Dampak (waktu terbuang, jumlah kegagalan)

- Lead Time manual: [ISI] menit
- Jumlah kegagalan (failed attempts): [ISI]
- Pertanyaan yang seharusnya diajukan kepada Dev: [ISI]

## Akar Masalah pada SISTEM (bukan pada orang)

1. Artefak serah-terima tidak lengkap secara struktural: berkas dependensi
   (`requirements.txt`) tidak termasuk dalam paket yang diserahkan, dan tidak ada
   mekanisme yang memeriksa kelengkapan paket sebelum diserahkan.
2. Prosedur serah-terima hanya berupa teks bebas. Tidak ada format baku yang
   mewajibkan versi Python, pembuatan virtual environment, dan port disebutkan.
3. Lingkungan Dev dan Ops tidak dijamin identik, sehingga keberhasilan di satu
   mesin tidak menjadi bukti keberhasilan di mesin lain ("it works on my machine").
4. Tidak ada verifikasi otomatis (smoke test) pada sisi penerima; kegagalan baru
   diketahui setelah dijalankan manual.

## Tindakan Perbaikan (action items) + penanggung jawab peran

| Tindakan | Penanggung jawab |
|---|---|
| Mengganti prosedur manual dengan `setup.sh` yang membuat venv, memasang dependensi terkunci, dan menjalankan health check | Developer |
| Mengunci versi dependensi pada `requirements.txt` dan menyertakannya dalam repository | Developer |
| Menjalankan `./setup.sh` pada mesin bersih sebagai syarat sebelum serah-terima | Operations |
| Menyimpan seluruh artefak di version control, bukan dikirim sebagai berkas lepas | Developer dan Operations |

## Pelajaran yang Diambil

- Kegagalan serah-terima adalah masalah desain proses, bukan kelalaian individu;
  mengganti personel tidak akan mencegahnya terulang.
- Prosedur yang dituliskan sebagai skrip dapat diuji, ditinjau, dan dijalankan
  ulang dengan hasil yang sama oleh siapa pun.
- Pengukuran (Lead Time, jumlah kegagalan) membuat pemborosan terlihat dan
  menjadi dasar keputusan otomasi.
