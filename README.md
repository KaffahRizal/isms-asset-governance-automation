# 🔐 WF-01 — Otomasi Tata Kelola Aset SMKI (ISO 27001:2022)

> Sistem otomasi manajemen inventaris aset informasi berbasis **n8n** yang terintegrasi dengan **Groq AI**, **Google Sheets**, dan **Telegram** — sesuai standar ISO/IEC 27001:2022 Annex A.

---

## 📁 Isi Repositori

```
📦 WF-01-Asset-Management-SMKI/
├── WF-01___Asset_Management_SMKI.json   # Workflow n8n (import langsung)
├── Laporan_SMKI_WF01.pdf                # Laporan lengkap dengan screenshot
├── Presentasi_SMKI_WF01.pdf             # Slide presentasi project
└── README.md
```

---

## 🎯 Tujuan

Membangun sistem otomasi berbasis n8n untuk:
- Mengelola **inventaris aset SMKI** secara terpusat
- Melakukan **analisis risiko & pemetaan kontrol ISO 27001** berbantuan AI secara otomatis
- Menjalankan **monitoring kepatuhan** dan audit berkala tanpa intervensi manual
- Mengirimkan **notifikasi dan laporan governance** secara real-time melalui Telegram

---

## 🛠️ Teknologi yang Digunakan

| Komponen | Teknologi |
|----------|-----------|
| Orchestrator | n8n Self-Host |
| Input Data | n8n Form Trigger |
| Database | Google Sheets (ASSET_INVENTORY + AUDIT_LOG) |
| Model AI | Groq — `llama-3.1-8b-instant` |
| Notifikasi | Telegram Bot |
| Standar Referensi | ISO/IEC 27001:2022 Annex A |

---

## ⚙️ Alur Workflow

Workflow WF-01 terdiri dari **dua alur utama** dan satu **error handler**:

### Alur 1 — Pendaftaran Aset Baru (Event-driven)
```
[Form Trigger]
    → [Siapkan Data Aset Baru]      # Generate Asset ID + auto-klasifikasi
    → [Basic LLM Chain (Groq AI)]   # Analisis risiko + pemetaan kontrol ISO 27001
    → [Code: Parse Hasil AI]        # Bersihkan & parse output JSON dari AI
    → [Google Sheets: Append Row]   # Simpan ke ASSET_INVENTORY
    → [Telegram: Konfirmasi]        # Notifikasi tim SMKI
```

### Alur 2 — Audit Bulanan (Scheduled — setiap tgl 1 pukul 08:00)
```
[Cron Trigger]
    → [Google Sheets: Baca Data]    # Ambil seluruh inventaris aset
    → [Code: Evaluasi Status]       # Hitung hari → OK / DUE_SOON / OVERDUE
    → [IF: Perlu Reminder?]
        ├── [IF: Status OVERDUE?]
        │       ├── True  → [Telegram: Alert OVERDUE ke Manager]
        │       └── False → [Telegram: Reminder DUE_SOON ke Owner]
        └── [Google Sheets: Update Status]
    → [Code: Generate Laporan]      # Agregasi statistik bulanan
    → [Telegram: Kirim Laporan]     # Monthly Governance Report
    → [Google Sheets: Simpan Log]   # Rekam ke AUDIT_LOG
```

### Error Handler
```
[Error Trigger] → [Telegram: Alert Error ke Admin]
```

---

## ✨ Fitur Utama

- **🆔 Auto Asset ID** — Generate ID unik format `AST-YYYYMMDD-XXXX` otomatis
- **🏷️ Auto-Klasifikasi** — Klasifikasi aset otomatis (Confidential / Restricted / Internal / Public) berdasarkan tipe aset
- **🤖 AI Compliance Mapping** — Analisis risk level & rekomendasi maks. 5 kontrol Annex A ISO 27001:2022 via Groq AI
- **📊 Centralized Inventory** — Semua data tersimpan di Google Sheets sebagai single source of truth
- **⏰ Audit Otomatis** — Cron bulanan mengevaluasi status review setiap aset (OK / DUE_SOON / OVERDUE)
- **🔔 Smart Alerting** — Eskalasi OVERDUE ke Manager, reminder DUE_SOON ke Asset Owner
- **📋 Monthly Report** — Laporan governance bulanan otomatis berisi statistik distribusi risiko & klasifikasi
- **🚨 Error Handler** — Notifikasi error otomatis ke admin via Telegram

---

## 🚀 Cara Menggunakan

### 1. Import Workflow ke n8n
```
n8n Dashboard → Workflows → Import from File
→ pilih WF-01___Asset_Management_SMKI.json
```

### 2. Konfigurasi Credentials
Hubungkan credential berikut di n8n:
- **Google Sheets OAuth2** — untuk baca/tulis ASSET_INVENTORY dan AUDIT_LOG
- **Groq API Key** — untuk model `llama-3.1-8b-instant`
- **Telegram Bot Token** — untuk notifikasi dan laporan

### 3. Siapkan Google Sheets
Buat spreadsheet dengan dua tab:
- `ASSET_INVENTORY` — inventaris aset utama
- `AUDIT_LOG` — rekam jejak laporan bulanan

Kolom ASSET_INVENTORY yang dibutuhkan:
```
Asset ID | Nama Aset | Tipe Aset | Klasifikasi | Asset Owner | Departemen |
Lokasi | Nilai Aset | AI Risk Level | AI ISO Controls | AI Reason |
Tanggal Registrasi | Tanggal Review Terakhir | Status Review
```

### 4. Aktivasi Workflow
```
n8n Dashboard → klik workflow → klik tombol Publish
```

---

## 📊 Contoh Data Aset

```json
{
  "asset_id": "AST-20260519-6653",
  "nama_aset": "Database PostgreSQL Pelanggan",
  "tipe_aset": "database",
  "klasifikasi": "Confidential",
  "asset_owner": "Budi Santoso",
  "departemen": "IT",
  "lokasi": "Data Center Jakarta",
  "nilai_aset": "Critical",
  "AI Risk Level": "High",
  "AI ISO Controls": "A.5.9, A.8.1, A.8.7, A.5.12, A.8.24",
  "AI Reason": "Database pelanggan memerlukan kontrol akses ketat dan enkripsi...",
  "tanggal_registrasi": "2026-05-19",
  "status_review": "OK"
}
```

---

## 📌 Status Review Aset

| Status | Kondisi | Tindakan Otomatis |
|--------|---------|-------------------|
| ✅ **OK** | < 330 hari sejak review | Tidak ada notifikasi |
| ⏰ **DUE_SOON** | 330–364 hari sejak review | Reminder ke Asset Owner |
| ❌ **OVERDUE** | ≥ 365 hari sejak review | Eskalasi alert ke Manager |

---

## 📄 Dokumentasi

- 📋 **Laporan Lengkap** — lihat `Laporan_SMKI_WF01.pdf` untuk dokumentasi screenshot setiap node dan konfigurasi detail
- 🎞️ **Slide Presentasi** — lihat `Presentasi_SMKI_WF01.pdf` untuk overview arsitektur dan demo workflow

---

## 👤 Tentang Project

| | |
|--|--|
| **Mata Kuliah** | AI for Business |
| **Institusi** | STT-NF |
| **Standar** | ISO/IEC 27001:2022 |
| **Workflow ID** | WF-01 — Asset Management SMKI |
