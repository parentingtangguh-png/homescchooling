# CLAUDE.md — Homeschooling Komunitas Curriculum Project

> Single source of truth untuk cara kerja Claude dan status progres dokumen kurikulum.  
> **Perbarui file ini setiap kali ada perubahan versi, keputusan baru, atau koreksi baru.**

---

## 1. Arsitektur Dokumen

Hierarki derivasi (root → daun):

```
Konstitusi (Profil Lulusan) v1.5     ← root; FINAL
    ↓
Peta Kompetensi Fase C v1.1          ← turunan pertama; FINAL
    ↓
Milestone Fase B v1.1                ← turunan kedua; FINAL
    ↓
Milestone Fase A v1.0                ← turunan ketiga; FINAL
    ↓
Peta CP dan Elemen v0.1 rev.25       ← peta teknis lintas fase; DRAF (pra-v1.0; verifikasi + closure selesai; PL5↔Seni ditutup rev.23; K-mapping PL5 dikonsistensikan rev.25; blocker: cabang Seni)
    ↓
Indikator Asesmen                    ← belum disusun
```

**Aturan derivasi:** Perubahan di level atas selalu memerlukan version bump di semua dokumen turunan yang terpengaruh. Dokumen final (bukan draf) tidak boleh diubah substantif tanpa menaikkan versi.

---

## 2. Status Versi Dokumen (per 26 September 2026)

| File | Versi Isi | Status | Nama File Fisik |
|---|---|---|---|
| Konstitusi (Profil Lulusan) | **v1.5** | Final | `profil_lulusan_homeschooling_komunitas_v1_5.md` |
| Peta Kompetensi Fase C | **v1.1** | Final | `peta_kompetensi_fase_c_v1_1.md` |
| Milestone Fase B | **v1.1** | Final | `milestone_fase_b_v1_1.md` |
| Milestone Fase A | **v1.0** | Final | `milestone_fase_a_v1_0.md` |
| Peta CP dan Elemen | **v0.1 rev.25** | Draf — pra-v1.0; seluruh primary-source verification selesai (rev.11–18); final consistency closure selesai (rev.19–22); PL5↔Seni residual ditutup (rev.23); K-mapping PL5↔Seni dikonsistensikan (rev.25); tidak ada known issue substantif tersisa; **blocker tunggal: keputusan cabang Seni**; review ditutup rev.23 | `peta_cp_dan_elemen_v0_1.md` |
| Indikator Asesmen | — | Belum disusun | — |

**Invariant:** Nama file fisik harus selalu sinkron dengan versi isi dokumen. Jika versi isi naik, rename file sekaligus.

---

## 3. Keputusan Kurikulum yang Sudah Final

| Keputusan | Lokasi resmi | Catatan |
|---|---|---|
| PAI adalah satu-satunya mata pelajaran agama | Konstitusi v1.5, Profil 1 — Dasar | Keputusan desain internal; bukan klaim demografis |
| `PL1.01` bersifat Islam-spesifik | Konstitusi v1.5, PL1.01 | Diubah dari rumusan generik di v1.4 |
| Basis CP umum: Kepka BSKAP 046/H/KR/2025 | Semua dokumen | Berlaku untuk semua mapel kecuali Agama |
| Basis CP PAI: Kepka BKPDM 020/2026 | Semua dokumen | Hanya mengubah CP Agama dan Budi Pekerti |
| Tidak ada CP IPAS di Fase A | Peta CP Bagian 8.3; Milestone A Bagian 7.1 | KI-PD Fase A = progression internal |
| PM 13/2025 tidak memblokir | Peta CP Bagian 14.A.3 | Mengatur pendekatan, bukan substansi CP |

---

## 4. Keputusan yang Masih Terbuka

| Isu | Lokasi | Tindakan yang diperlukan |
|---|---|---|
| Pilihan cabang seni | Peta CP Bagian 14.C.1 | Tetapkan jalur seni per peserta didik atau komunitas |
| Bahasa Inggris SD | Peta CP Bagian A.4 | Wajib mulai 2027/2028; pemetaan CP belum dilakukan |

---

## 5. Status Pemetaan PAI — SELESAI (rev.17)

Teks Kepka BKPDM 020/2026 sudah diverifikasi per fase A/B/C. Semua klaim PAI sudah dikonfirmasi:

| Elemen PAI | Hubungan ke kode internal | Kategori final |
|---|---|---|
| Al-Quran dan Hadis | `PL1.01` (aspek: Al-Quran) | **PARSIAL** |
| Aqidah | `PL1.01` (aspek: aqidah/keimanan) | **PARSIAL** |
| Fiqih | `PL1.01` (aspek: ibadah) | **PARSIAL** |
| Akhlak | `PL1.02`–`PL1.08` | **LANGSUNG** *(dikonfirmasi per fase A/B/C)* |
| SPI | `PL2.01`, `KI-PD.D.10` | **KONTEKSTUAL** |
| Gabungan Al-Quran + Aqidah + Fiqih → `PL1.01` | — | **LANGSUNG** *(gabungan 3 elemen)* |

`PL1.01` dan `PL1.02`–`PL1.08` **bukan INTERNAL** — keduanya memiliki hubungan nasional via CP PAI dan tidak boleh masuk tabel INTERNAL di Bagian 14.B Peta CP.

---

## 6. Cara Kerja Claude (Wajib Diikuti)

### 6.1 Sebelum mengedit

Untuk setiap pola yang akan diperbaiki, **grep exhaustif dulu** di seluruh file yang relevan:

```bash
grep -n "pola_yang_dicari" *.md
```

Jangan mulai mengedit sebelum tahu semua kemunculan pola tersebut.

### 6.2 Saat mengedit

- Edit **semua kemunculan sekaligus**, bukan hanya yang dilaporkan dalam audit
- Untuk perubahan versi: cari semua referensi versi lama (`grep -rn "v1\.0\|v1\.4" *.md`) dan perbarui seluruhnya
- Untuk perubahan kategori (L/P/K/I/BT): cek **semua tabel dan matriks**, bukan hanya satu bagian

### 6.3 Setelah mengedit

Selalu lakukan **zero-residue sweep**:

```bash
# Contoh: setelah mengubah kategori LANGSUNG → PARSIAL untuk PL1.01
grep -n "PL1\.01.*| L |" *.md   # harus kosong
grep -n "| L |.*PL1\.01" *.md   # harus kosong
```

Laporkan hasil sweep kepada pengguna sebelum menyatakan pekerjaan selesai.

### 6.3a Jangan menyamakan nama elemen CP nasional dengan nama kode internal

Kesalahan yang berulang dari rev.14 sampai rev.21 dan ditutup pada rev.22: nama elemen CP nasional diperlakukan sebagai sinonim kode internal, lalu kategori disimpulkan dari kemiripan nama, bukan dari substansi CP.

| Nama elemen CP nasional | **Bukan** otomatis | Uji yang benar |
|---|---|---|
| Berpikir dan Bekerja Secara Artistik | `KI-SEN.05` Desain | Apakah CP menuntut merancang, menerapkan desain, atau menyusun komposisi? |
| Mengalami | `KI-SEN.01` Eksplorasi media | Apakah CP menuntut menguji coba/mengeksplorasi alat, bahan, atau material? |
| Merefleksikan | `PL4.07` Menerima umpan balik | Apakah CP menuntut *menerima* masukan, atau hanya *memberi* umpan balik? |
| Berdampak | `KI-SEN.06` Penciptaan karya | Apakah CP menuntut karya, atau hanya sikap/afektif? |

**Aturan:** nama kode internal selalu diambil **persis** dari `peta_kompetensi_fase_c_v1_1.md`. Sinonim yang memperluas makna dilarang. Setiap pasangan kode ↔ CP dinilai dari substansi CP.

Aturan yang sama berlaku untuk ringkasan CP: ringkasan tidak boleh mengulang nama kode internal (itu membuat kategori melingkar dan tidak dapat diaudit), dan substansi satu elemen tidak boleh bocor ke ringkasan elemen lain.

### 6.3b Verifikasi derivasi secara mekanis

Untuk matriks yang merupakan derivasi (Bagian 10.5, Matriks 12, Matriks 13 dari Bagian 10), jangan memverifikasi dengan membaca. Parse tabel sumber, hitung ulang kategori terkuat per kode, lalu bandingkan dengan tabel turunan secara program. Rev.22 menggunakan cara ini dan menemukan bahwa pembacaan manual pada rev.20–21 melewatkan banyak baris.

### 6.4 Konsistensi lintas dokumen

Setiap kali ada perubahan di satu dokumen, periksa apakah dokumen lain perlu sinkronisasi:

| Jika diubah di... | Periksa konsistensi di... |
|---|---|
| Konstitusi | Semua 4 dokumen turunan |
| Peta C | Milestone B, Milestone A, Peta CP |
| Milestone B | Milestone A, Peta CP |
| Peta CP (kategori/kode) | Milestone A (crosswalk), Milestone B (crosswalk) |

### 6.5 Versi dan nama file

Kapan menaikkan versi:
- **Dokumen final** (non-draf): naik versi jika ada perubahan substantif apapun
- **Dokumen draf**: tambah rev internal (rev.1, rev.2, ...) sampai siap jadi v1.0
- Nama file fisik **harus di-rename** bersamaan dengan kenaikan versi isi

### 6.6 Bahasa klaim

| Konteks | Redaksi yang benar | Redaksi yang salah |
|---|---|---|
| Keputusan mapel agama | "jalur mata pelajaran agama ditetapkan sebagai PAI" | "seluruh peserta didik beragama Islam" |
| Status verifikasi belum selesai | "provisional LANGSUNG — menunggu primary source" | "LANGSUNG" (tanpa kualifikasi) |
| IPAS Fase A | "tidak ada CP IPAS di Fase A; progression internal" | "CP IPAS Fase A" |

---

## 7. Blokir Menuju v1.0 Peta CP

**Status fase:** Audit internal DITUTUP per rev.10. Primary-source verification SELESAI per rev.11–18. Fase **pra-v1.0 — final consistency closure** SELESAI per rev.19–22 (rev.19–20 status + Matriks A/B; rev.21 Matriks Fase C; rev.22 regenerasi Bagian 10 Seni dan Bagian 6.3 BI dari teks primer, ditambah regenerasi seluruh matriks turunannya). PL5↔Seni residual **DITUTUP per rev.23**. Audit kelengkapan PL5 **DITUTUP per rev.24**. K-mapping PL5↔Seni **DIKONSISTENSIKAN per rev.25** — metodologi per-pasangan KONTEKSTUAL diterapkan konsisten; `PL5.07` Fase C naik dari K ke P. Review ditutup rev.23.

Peta CP belum dapat dinaikkan ke v1.0 sampai:

1. **Kepka BSKAP 046/H/KR/2025** — verifikasi urutan berikut:
   - ~~PJOK~~ → **SELESAI (rev.11)**. PL5 heterogen per kode/fase; PL6.03=I; PL7 dipecah L/P/K; PL3.08/09 dikoreksi per elemen. Konflik PL5↔PJOK DITUTUP.
   - ~~Matematika Fase C~~ → **SELESAI (rev.12)**. Lima elemen konten dikoreksi dari teks primer hlm. 114–116; KI-NUM.09/.10 dipecah jadi PARSIAL via elemen proses; KI-NUM.08 tetap LANGSUNG.
   - ~~IPAS Fase C~~ → **SELESAI (rev.13)**. KI-PD.P.02/.09=P; KI-PD.D.04 Fase C=I; KI-PD.D.08=P; D.01–.03/.05–.07/.09–.12=L; PL3.05/.06/.07 Fase C=L via IPAS.
   - ~~Seni A–C~~ → **SELESAI (rev.14–16)**. Seni C selesai rev.14–15; Seni A/B diregenerasi dari teks primer dan semua baris ^s dihapus (rev.16); Berpikir Artistik A/B dikoreksi dari L ke P/K per cabang.
   - ~~Pendidikan Pancasila A/B/C~~ → **SELESAI (rev.16)**. PL2.05 direlokasi ke UUD NRI C; KI-PD.D.07 Fase A BT-A ditutup via PP NKRI A; KI-PD.D.10 Fase C PARSIAL ditambah.
   - ~~Bahasa Indonesia A/B/C~~ → **SELESAI (rev.16)**. KI-LIT.06 Fase B/C turun dari L ke P (dikonfirmasi dari cp-data.json).
   - ~~Matematika A/B~~ → **SELESAI (rev.16)**. Simbol =, nilai tidak diketahui, kelipatan/faktor/persen dikonfirmasi dari teks primer.
   - ~~IPAS A/B~~ → **SELESAI (rev.16)**. KI-PD.D.07 Fase A BT-A ditutup. Gaya Fase B = gap nasional tanpa kode — dicatat di Bagian 8.2.
2. ~~**Kepka BKPDM 020/2026**~~ — teks penuh CP PAI per fase (A/B/C) sudah diverifikasi → **SELESAI (rev.17)**. Akhlak A/B/C = LANGSUNG; PL1.01 gabungan = LANGSUNG; Fikih B/C dikoreksi; SPI B dikoreksi.
3. ~~**Regenerasi Matriks 12 dan 13**~~ dari hasil verifikasi → **SELESAI (rev.17)** untuk PAI (Matriks 12.1 pL→L; Matriks 13 PAI), **disinkronkan rev.20–21** untuk Seni Fase A/B lalu Fase C, dan **diregenerasi penuh rev.22** — Matriks 12.4, 12.13, 12.8, 12.9 PL9.09, serta 60 baris Seni dan 3 baris BI Berbicara di Matriks 13 kini merupakan derivasi terverifikasi dari Bagian 10 dan Bagian 6.3.
4. **Keputusan jalur seni** per peserta didik atau komunitas → pemetaan KI-SEN dapat diselesaikan
5. ~~Milestone A naik ke v1.0~~ → **SELESAI (v1.0 per 22 September 2026)**
6. ~~Audit internal~~ → **DITUTUP (rev.10 per 22 September 2026)**

**Blocker tersisa menuju v1.0:**
- Keputusan cabang Seni per peserta didik/komunitas

---

## 8. Riwayat Audit

| Audit | Tanggal | Hasil | Tindakan |
|---|---|---|---|
| Audit pertama | 2026-09-22 | LAYAK SETELAH REVISI SUBSTANTIF | Koreksi 5 kategori; Konstitusi naik ke v1.5 |
| Audit kedua | 2026-09-22 | Amendemen PAI DITERIMA; paket belum bersih | Koreksi 17 titik; rev.4 Peta CP; sweep zero-residue |
| Audit ketiga | 2026-09-22 | 17 koreksi terkonfirmasi; 2 residu baru ditemukan | Koreksi 9 titik (rev.5); Milestone A siap v1.0 setelah koreksi minor |
| Audit keempat | 2026-09-22 | Rev.5 diterima; 2 kasus pending diputuskan; blocker Peta CP diklarifikasi | Rev.6 Peta CP; Milestone A naik ke **v1.0** |
| Audit kelima | 2026-09-22 | Rev.6 diterima; Milestone A v1.0 FINAL; 8 item rev.7 ditemukan | Rev.7 Peta CP (selesai); Milestone A **FINAL** |
| Audit keenam | 2026-09-22 | Rev.7 diterima sebagian; 5 residu ditemukan: 2 internal murni + 3 pending primary-source | Rev.8: koreksi 14.A.2 dan 14.B; 3 konflik ditandai eksplisit |
| Audit ketujuh | 2026-09-22 | Rev.8 dikonfirmasi; 2 residu redaksional di 16.2 dan 16.4 | Rev.9: koreksi PL9 di 16.2 + Catatan PL9 Matriks + 16.4 KI-PD.D |
| Audit kedelapan | 2026-09-22 | Rev.9 dikonfirmasi; 3 residu kecil: Matriks 13 IPAS-A, 16.6, tiga kalimat Milestone A lama | Rev.10: semua diselesaikan — **AUDIT INTERNAL DITUTUP** |
| Primary-source PJOK | 2026-09-22 | Kepka 046/H/KR/2025 diverifikasi; konflik PL5↔PJOK ditutup; PL7 dipecah; PL6.03=I; PL3.08/09 dikoreksi | Rev.11: Bagian 9.1–9.4, 9.2a; Matriks 12.3/12.5/12.6/12.7; 12 baris PJOK Matriks 13 |
| Primary-source Matematika Fase C | 2026-09-22 | Kepka 046/H/KR/2025 hlm. 114–116 diverifikasi; 5 ringkasan Fase C dikoreksi; KI-NUM.09/.10=P via elemen proses; KI-NUM.08=L | Rev.12: Bagian 7.1–7.6; Matriks 12.11; 5 baris Mat-C Matriks 13; 14.C.2, 16.9, 17 |
| Primary-source IPAS Fase C | 2026-09-22 | Kepka 046/H/KR/2025 diverifikasi; KI-PD.D.04 Fase C=I; D.08=P; P.02/.09=P; PL3.05/.06/.07 Fase C=L; klaim berlebih dihapus | Rev.13: Bagian 8.1 C, 8.4; Matriks 12.3/12.12; 4 baris IPAS-C Matriks 13; 14.B/C.2, 16.4/16.9, 17 |
| Primary-source Seni A–C empat cabang | 2026-09-22 | Kepka 046/H/KR/2025 diverifikasi; keempat cabang branch-conditional; konflik PL4.07 dan PL9↔Seni ditutup; KI-SEN.08=K; 10.6 menjadi crosswalk; residu rev.13 ditutup (D.05 listrik, 14.C.2) | Rev.14: Bagian 10 rewrite; Matriks 12.4/12.9/12.12/12.13; 60 baris Seni Matriks 13; 14.B/C.2, 16.2/16.4/16.9, 17 |
| Koreksi Seni pasca-audit (rev.15) | 2026-09-22 | Bagian 10 diregenerasi: urutan elemen resmi, per-pair rows, teks primer Musik A/B/C dan Rupa B/C; Tari C Menciptakan P→L; PL4.09 Fase C dikoreksi L→P; Teater C Menciptakan/Berdampak dibedakan; Matriks 13 Tari/Rupa/Musik C dikoreksi; Bagian 4.2/14.C.1/17 poin 4 diperbaiki | Rev.15 |
| Review eksternal korpus (pasca-rev.15) | 2026-09-22 | Pemeriksaan seluruh 7 file; ditemukan: Seni A/B belum bersih (^s tidak valid), PP/BI/Mat-A/B/IPAS-A/B belum pernah diaudit end-to-end; gaya IPAS B = gap nasional tanpa kode; KI-LIT.09 konflik internal 6.4 vs 6.5; KI-LIT.06 Fase C overclaim; CLAUDE.md stale. Rekomendasi: rev.16 normalisasi sebelum PAI. | Rev.16 dimulai |
| Normalisasi primary-source rev.16 | 2026-09-22 | cp-data.json diverifikasi untuk PP/BI/Mat-A/B/IPAS-A/Seni-A/B: PL2.05 direlokasi ke UUD NRI C; KI-PD.D.07 Fase A BT-A ditutup; KI-LIT.06 B/C turun ke P; simbol=/nilai-tidak-diketahui/kelipatan-faktor-persen dikonfirmasi; 32 baris Seni A/B 4 cabang diregenerasi dari teks primer; semua ^s dihapus; Berpikir Artistik A/B dikoreksi dari L ke P/K | **Rev.16 SELESAI** — blocker tersisa: PAI (Kepka 020/2026) dan keputusan cabang Seni |
| Primary-source PAI rev.17 | 2026-09-22 | Kepka BKPDM 020/2026 diverifikasi dari SlideShare: Akhlak A/B/C = LANGSUNG dikonfirmasi; Fikih B dikoreksi (azan/salat jumat/taklīf); Fikih C dikoreksi (puasa/makanan halal-haram/zakat — tidak ada haji); SPI B: PL2.02 dan KI-PD.D.11 dihapus; PL1.01 per elemen = PARSIAL, gabungan = LANGSUNG; notasi pL dihapus dari legenda; "provisional" dihapus dari seluruh dokumen | **Rev.17 SELESAI** — blocker tersisa: keputusan cabang Seni |
| Keputusan Gaya IPAS B rev.18 | 2026-09-22 | Gaya IPAS Fase B ditetapkan sebagai *documented national gap* — minimum nasional wajib tanpa kode `KI-PD.D` khusus; tidak dipetakan ke `.D.05`, tidak menambah `.D.13`; bukan blocker v1.0. Bagian 8.2 dan 16.9 pertanyaan 7 diperbarui. | **Rev.18 SELESAI** — blocker tersisa: keputusan cabang Seni |
| Housekeeping konsistensi status rev.19 | 2026-09-22 | Header status dan Bagian 4.2 diperbarui (semua verifikasi selesai); Bagian 16.4 baris "belum pernah diverifikasi" dicoret; Bagian 17 daftar langkah tersisa dan item 8–9 ditandai SELESAI; review ditutup. | **Rev.19 SELESAI** — blocker tunggal: keputusan cabang Seni |
| Sinkronisasi derivatif Matriks 12/13 rev.20 | 2026-09-22 | Matriks 12.4 PL4.09 A/B: L→K (semua cabang); Matriks 13 Seni A/B 8 baris Menciptakan: PL4.05 dihapus; 8 baris Merefleksikan: PL4.09 L→K, status L→L/K; IPAS B Pemahaman dipecah: D.01–.12 Tercakup + Gaya documented national gap; Bagian 17 status `Draf — Primary-Source Verification` → `Draf — Pra-v1.0`; item 9 ditutup pasca-sinkronisasi; CLAUDE.md fase aktif diperbarui. | **Rev.20 SELESAI** — catatan: sapu A/B saja; residu C ditemukan pasca-rev.20 |
| Sinkronisasi derivatif Matriks 12/13 Fase C rev.21 | 2026-09-22 | Matriks 12.4 PL4.01 C: L semua → L(Musik/Rupa/Tari); P(Teater); Matriks 12.13 KI-SEN.01 C: L(Musik/Rupa); P(Tari/Teater) → L semua cabang; Matriks 13 Tari C Menciptakan: baris PL4.05 dihapus; Matriks 13 Teater C Menciptakan: PL4.05 dihapus — hanya KI-SEN.06, PL4.04; CLAUDE.md Bagian 9 diperbarui (kasus pending Seni dieksplisitkan). | **Rev.21 SELESAI** — blocker tunggal: keputusan cabang Seni |
| Final consistency closure rev.22 | 2026-09-22 | **Semantic drift KI-SEN/PL4 ditemukan dan ditutup.** Nama elemen CP nasional selama ini diperlakukan sebagai sinonim kode internal (`Berpikir Artistik`→`KI-SEN.05`, `Mengalami`→`KI-SEN.01`, `Merefleksikan`→`PL4.07`, `Berdampak`→`KI-SEN.06`). Seluruh 60 baris sumber Bagian 10 diregenerasi dari teks CP primer; 4 kebocoran substansi antar-elemen dan 1 penambahan makna tanpa dasar primer dihapus; Bagian 10.0 (definisi kanonik + aturan uji kategori) ditambahkan; Bagian 10.5 diregenerasi menjadi 3 tabel (A/B/C × 17 kode × 4 cabang); Matriks 12.4, 12.13, dan 60 baris Seni Matriks 13 diregenerasi sebagai derivasi dan **diverifikasi secara mekanis**. Bagian 6.3 BI A/B/C dibongkar per pasangan: `PL8.06` C L→K, `PL8.07` C L→P, `PL8.09` C L→P; Matriks 12.8 dan Matriks 13 BI disinkronkan. Matriks 12.9 `PL9.09` dipecah per cabang per fase. Temuan tambahan (`PL5`↔Tari B/C) dicatat, tidak dipetakan. 137 kode utuh; tidak ada dokumen FINAL diedit. | **Rev.22 SELESAI** — review ditutup; known issue BI C ditutup; blocker tunggal: keputusan cabang Seni |
| Penutupan residual PL5↔Seni rev.23 | 2026-09-26 | Audit seluruh 4 cabang × 3 fase × PL5.01–.10: hanya Tari B dan Tari C Berpikir Artistik mengandung tuntutan kolaborasi substantif. Tari B: `PL5.01` L dan `PL5.08` P ditambahkan ke Bagian 10.3. Tari C: `PL5.01` L dan `PL5.06` L ditambahkan ke Bagian 10.3. Catatan "dicatat, bukan dipetakan" dihapus. Matriks 12.5 (`PL5.01`/`.06`/`.08`) dan Matriks 13 (Tari B/C Berpikir Artistik) disinkronkan. Kategori tidak berubah — PJOK sudah memberi kategori setara atau lebih kuat. 137 kode tetap utuh. | **Rev.23 SELESAI** — review ditutup; blocker tunggal: keputusan cabang Seni |
| Micro-closure audit kelengkapan PL5 rev.24 | 2026-09-26 | PL5.10=P ditambahkan untuk Tari B dan Tari C Berpikir Artistik ("tujuan bersama"/"berperan aktif dalam kelompok" — standar sama dengan PJOK B/C "keberhasilan kelompok"). PL5.01=P ditambahkan untuk Teater C Mengalami ("permainan peran berkelompok" + "aksi dan reaksi" — tujuan CP melatih akting → PARSIAL). Bagian 10.5 scope diklarifikasi: hanya 17 kode domain Seni (PL4.01–.09, KI-SEN.01–.08); PL5 lintas-domain dirangkum di Matriks 12.5. Matriks 12.5 dan Matriks 13 disinkronkan. Kategori akhir tidak berubah. 137 kode tetap utuh. | **Rev.24 SELESAI** — audit PL5 ditutup; blocker tunggal: keputusan cabang Seni |
| K-mapping PL5↔Seni rev.25 | 2026-09-26 | Audit menemukan K-mapping tidak konsisten dengan definisi KONTEKSTUAL dan preseden Bagian 9.2. Prinsip: sumber PJOK yang lebih kuat tidak menghapus hubungan per-pasangan K di Bagian 10 dan Matriks 13. **Tari B Berpikir Artistik:** `PL5.02`–`PL5.07` K ditambahkan. **Tari C Berpikir Artistik:** `PL5.08` P dan `PL5.02`–`PL5.05`, `PL5.07` K ditambahkan. **Teater C Mengalami:** `PL5.06` P dan `PL5.07` P dan `PL5.02`–`PL5.05`, `PL5.08`, `PL5.10` K ditambahkan. **Perubahan kategori:** `PL5.07` Fase C naik dari K ke P (Teater C: "improvisasi" + "aksi dan reaksi" = tuntutan eksplisit). Matriks 12.5 (semua sumber baru), Matriks 13, Catatan PL5, Bagian 14.B, Bagian 17 item 12 disinkronkan. 137 kode tetap utuh. | **Rev.25 SELESAI** — blocker tunggal: keputusan cabang Seni |

### Koreksi Audit Ketiga (rev.5 Peta CP)

| # | Lokasi | Masalah | Tindakan |
|---|---|---|---|
| 1 | Milestone A Bagian 8.2 | Kata "IPAS" masih muncul sebagai sumber constraint Fase A | Dihilangkan; kalimat ditulis ulang |
| 2 | Peta CP Bagian 5.6 | "berstatus LANGSUNG" tanpa qualifier provisional untuk Akhlak | Ditambah "*provisional* LANGSUNG — menunggu verifikasi" |
| 3–4 | Peta CP Bagian 6.4 Menulis Fase A/B | `PL6.09`, `PL6.10` dalam baris LANGSUNG; bertentangan dengan Bagian 6.5 | Dipecah: baris LANGSUNG (PL8/KI-LIT) + baris KONTEKSTUAL (PL6.09–10) |
| 5–7 | Peta CP Bagian 7.2 Aljabar A/B/C | `PL3.04`–`PL3.09` dalam baris LANGSUNG; bertentangan dengan Bagian 7.6 | Dipecah: LANGSUNG (KI-NUM) + KONTEKSTUAL (PL3) |
| 8 | Peta CP Bagian 7.4 Geometri Fase B | `PL3.04`–`PL3.05` dalam baris LANGSUNG; bertentangan dengan Bagian 7.6 | Dipecah |
| 9–11 | Peta CP Bagian 7.5 Analisis Data A/B/C | `PL3.03`–`PL3.06` dalam baris LANGSUNG; bertentangan dengan Bagian 7.6 | Dipecah |

---

## 9. Kasus Pending — Butuh Keputusan Eksplisit

Satu kasus pending aktif: **keputusan jalur/cabang seni** per peserta didik atau komunitas — lihat Bagian 4 (Keputusan Terbuka) dan Bagian 7 (Blocker v1.0 Peta CP).

Setelah rev.25, ini adalah **satu-satunya keputusan kurikuler yang masih terbuka** sebelum Peta CP dapat dinaikkan ke v1.0. Secara teknis pemetaan sudah selesai: kategori per cabang per fase tersedia di Bagian 10.5 Peta CP, sehingga penetapan cabang tinggal memilih kolom yang berlaku — tidak diperlukan pemetaan ulang.
