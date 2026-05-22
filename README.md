# 🚀 Panduan Lengkap: OpenGateway + Hermes Agent

> **Mendapatkan akses gratis ke Xiaomi MiMo v2.5 Pro melalui OpenGateway (gitlawb) di Hermes Agent**

[![OpenGateway](https://img.shields.io/badge/OpenGateway-gitlawb.com-000?style=for-the-badge)](https://gitlawb.com/opengateway)
[![Hermes Agent](https://img.shields.io/badge/Hermes-Agent-FFD700?style=for-the-badge)](https://github.com/NousResearch/hermes-agent)
[![License](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)](LICENSE)

---

## Daftar Isi

- [Apa Itu OpenGateway?](#apa-itu-opengateway)
- [Apa Itu Hermes Agent?](#apa-itu-hermes-agent)
- [Persyaratan](#persyaratan)
- [Langkah 1: Dapatkan API Key](#langkah-1-dapatkan-api-key)
- [Langkah 2: Instal Hermes Agent](#langkah-2-instal-hermes-agent)
- [Langkah 3: Konfigurasi OpenGateway di Hermes](#langkah-3-konfigurasi-opengateway-di-hermes)
- [Langkah 4: Uji Koneksi](#langkah-4-uji-koneksi)
- [Langkah 5: Mulai Ulang Gateway](#langkah-5-mulai-ulang-gateway)
- [Model yang Tersedia](#model-yang-tersedia)
- [Pemecahan Masalah](#pemecahan-masalah)
- [Pertanyaan yang Sering Diajukan](#pertanyaan-yang-sering-diajukan)

---

## Apa Itu OpenGateway?

**OpenGateway** adalah gerbang inferensi LLM terbuka yang dikembangkan oleh [gitlawb](https://gitlawb.com). Berikut adalah fitur-fitur utamanya:

- **Satu endpoint yang kompatibel dengan OpenAI** — cukup ganti URL dasar, dan routing model akan dilakukan secara otomatis
- **Routing multi-penyedia** — mendukung Xiaomi MiMo, GMI Cloud, dan penyedia lainnya
- **Manajemen API Key** — buat, atur cakupan, dan cabut kunci langsung dari dasbor
- **Dasbor penggunaan real-time** — pantau penggunaan token secara langsung
- **Gratis selama jendela kemitraan** — autentikasi bersifat opsional saat ini, namun akan segera diwajibkan

Endpoint: `https://opengateway.gitlawb.com/v1`

## Apa Itu Hermes Agent?

**Hermes Agent** adalah kerangka kerja AI agent sumber terbuka yang dikembangkan oleh [Nous Research](https://nousresearch.com):

- Sistem keterampilan yang terus meningkatkan diri
- Memori persisten lintas sesi
- Multi-platform (Telegram, Discord, Slack, WhatsApp, CLI)
- Mendukung lebih dari 200 penyedia model
- Penjadwal bawaan (cron)
- Delegasi sub-agent

---

## Persyaratan

| Kebutuhan | Versi |
|---|---|
| Sistem Operasi | Linux / macOS / WSL2 |
| Python | 3.11 atau lebih baru |
| Node.js | 22 atau lebih baru (disarankan 24) |
| Koneksi Internet | Diperlukan |

---

## Langkah 1: Dapatkan API Key

1. Buka halaman **https://gitlawb.com/opengateway**
2. Klik tombol **"sign in to generate a key"**
3. Masuk menggunakan akun **X (Twitter)** Anda
4. Pada dasbor, klik **"Create Key"**
5. Salin kunci yang dihasilkan (format: `ogw_live_xxxxx`)

> ⚠️ Kunci ini **gratis** selama jendela kemitraan berlangsung. Autentikasi bersifat opsional saat ini, namun akan segera diwajibkan. Segera dapatkan selagi tersedia!

---

## Langkah 2: Instal Hermes Agent

Jika Anda belum menginstal Hermes, jalankan perintah berikut:

```bash
# Instalasi dengan satu perintah
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash

# Muat ulang konfigurasi shell
source ~/.bashrc

# Verifikasi instalasi
hermes --version
```

---

## Langkah 3: Konfigurasi OpenGateway di Hermes

### Opsi A: Mengedit file config.yaml langsung (Disarankan)

```bash
# Buka file konfigurasi
nano ~/.hermes-2/config.yaml
```

Ubah bagian `model` dan `providers` sebagai berikut:

```yaml
model:
  default: mimo-v2.5-pro
  provider: xiaomi
  context_length: 1000000
  aliases:
    mimo: mimo-v2.5-pro
  base_url: https://opengateway.gitlawb.com/v1

providers:
  xiaomi:
    base_url: https://opengateway.gitlawb.com/v1
    default_model: mimo-v2.5-pro
    api_key: ogw_live_KUNCI_ANDA_DI_SINI  # ← Ganti dengan kunci Anda
    name: Xiaomi MiMo
```

### Opsi B: Menggunakan CLI

```bash
# Atur penyedia
hermes config set model.provider xiaomi

# Atur model default
hermes config set model.default mimo-v2.5-pro

# Atur URL dasar
hermes config set model.base_url https://opengateway.gitlawb.com/v1

# Atur API Key
hermes config set providers.xiaomi.api_key ogw_live_KUNCI_ANDA_DI_SINI

# Atur URL dasar pada penyedia
hermes config set providers.xiaomi.base_url https://opengateway.gitlawb.com/v1
```

### Opsi C: Penyedia Kustom

Jika Anda ingin menggunakan OpenGateway sebagai penyedia terpisah (tanpa mengganti konfigurasi xiaomi yang sudah ada):

```yaml
# Tambahkan pada bagian providers di ~/.hermes-2/config.yaml:
custom_providers:
  opengateway:
    base_url: https://opengateway.gitlawb.com/v1
    api_key: ogw_live_KUNCI_ANDA_DI_SINI
    name: OpenGateway
    default_model: mimo-v2.5-pro
```

Kemudian atur model untuk menggunakan penyedia kustom:

```bash
hermes config set model.provider custom:opengateway
```

---

## Langkah 4: Uji Koneksi

### Uji melalui curl (langsung)

```bash
curl https://opengateway.gitlawb.com/v1/chat/completions \
  -H "Authorization: Bearer ogw_live_KUNCI_ANDA_DI_SINI" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "mimo-v2.5-pro",
    "messages": [
      {"role": "user", "content": "halo, gateway"}
    ]
  }'
```

Respons yang diharapkan:

```json
{
  "id": "gen-xxx",
  "model": "xiaomi/mimo-v2.5-pro-20260422",
  "provider": "Xiaomi",
  "choices": [...],
  "usage": {
    "prompt_tokens": 253,
    "total_tokens": 263,
    "cost": 0
  }
}
```

### Uji melalui Hermes

```bash
# Jalankan Hermes
hermes

# Di dalam percakapan, ketik:
halo, uji koneksi OpenGateway
```

Jika mendapat respons, berarti konfigurasi berhasil! ✅

---

## Langkah 5: Mulai Ulang Gateway

Jika Anda menggunakan Hermes gateway (Telegram, Discord, dll.):

```bash
# Mulai ulang gateway agar konfigurasi baru dimuat
hermes gateway restart

# Verifikasi gateway berjalan
hermes gateway status
```

---

## Model yang Tersedia

| Model | Penyedia | Keterangan |
|---|---|---|
| `mimo-v2.5-pro` | Xiaomi MiMo | Model unggulan, paling canggih |
| `mimo-v2.5` | Xiaomi MiMo | Versi standar |
| `mimo-v2-pro` | Xiaomi MiMo | Model pro generasi sebelumnya |
| `mimo-v2-omni` | Xiaomi MiMo | Multimodal (teks + visi) |
| `mimo-v2-flash` | Xiaomi MiMo | Cepat dan efisien |
| `mimo-v2.5-tts` | Xiaomi MiMo | Konversi teks ke ucapan |
| `google/gemini-3.1-flash-lite-preview` | GMI Cloud | Google Gemini |

> 💡 **Rekomendasi**: `mimo-v2.5-pro` — model unggulan yang paling banyak digunakan (lebih dari 9,9 juta permintaan di penggunaan global)

---

## Pemecahan Masalah

### Kesalahan "unauthenticated"

```bash
# Pastikan kunci Anda benar
echo "ogw_live_KUNCI_ANDA_DI_SINI" | head -c 10
# Awal harus: ogw_live_

# Uji langsung
curl -s https://opengateway.gitlawb.com/v1/chat/completions \
  -H "Authorization: Bearer ogw_live_KUNCI_ANDA_DI_SINI" \
  -H "Content-Type: application/json" \
  -d '{"model":"mimo-v2.5-pro","messages":[{"role":"user","content":"halo"}],"max_tokens":5}'
```

### Pesan "Not found. Use /v1/<provider>/<path>"

Endpoint yang benar: `https://opengateway.gitlawb.com/v1/chat/completions`
Bukan: `https://opengateway.gitlawb.com/v1/models` (tidak tersedia)

### Model tidak dikenali

Pastikan nama model sudah benar:

```bash
# ✅ Benar
"model": "mimo-v2.5-pro"

# ❌ Salah
"model": "mimo-v2.5-pro-20260422"  # nama ini akan di-resolve otomatis oleh API
```

### Jendela konteks terlalu kecil

Atur pada file config.yaml:

```yaml
model:
  context_length: 1000000  # 1 juta token
```

### Gateway tidak dapat dimulai ulang

```bash
# Hentikan paksa dan jalankan kembali
hermes gateway stop
hermes gateway start
```

---

## Pertanyaan yang Sering Diajukan

### Berapa lama masa gratisnya?

Selama **jendela kemitraan** berlangsung — durasi pastinya tidak dipublikasikan. Segera manfaatkan selagi tersedia!

### Apakah ada batas penggunaan token?

Dasbor tidak menampilkan batas kunci secara eksplisit. Namun berdasarkan data penggunaan global, gateway ini telah menangani **lebih dari 954 miliar token** dari seluruh pengguna. Sejauh ini, tidak ada pengguna yang dilaporkan mencapai batas.

### Apakah bisa digunakan di OpenClaw?

Tentu bisa. OpenClaw juga mendukung endpoint kustom yang kompatibel dengan OpenAI. Atur URL dasar ke `https://opengateway.gitlawb.com/v1` dan gunakan kunci yang sama.

### Apakah bisa digunakan di Claude Code atau Codex?

Bisa, selama alat tersebut mendukung endpoint yang kompatibel dengan OpenAI. Cukup ganti URL dasar.

### Apa perbedaannya dengan langsung menggunakan API Xiaomi?

OpenGateway melakukan routing melalui gerbang mereka, sehingga:

- Anda tidak perlu memiliki kunci API Xiaomi sendiri
- Gratis selama jendela kemitraan
- Tersedia dasbor penggunaan global
- Namun ketersediaan bergantung pada gerbang tersebut

---

## Statistik Penggunaan Global

Statistik real-time dapat dilihat di: **https://gitlawb.com/opengateway/usage/global**

| Metrik | Nilai |
|---|---|
| Total Permintaan | Lebih dari 10,6 juta |
| Model Terpopuler | mimo-v2.5-pro (9,9 juta permintaan) |
| Total Token | Lebih dari 954 miliar |
| Rata-rata Token/Permintaan | Sekitar 96 ribu |
| Penyedia | Xiaomi MiMo, GMI Cloud |

---

## Berkontribusi

Kontribusi sangat diperbuka! Jika Anda memiliki pembaruan atau perbaikan, silakan buka isu atau kirimkan permintaan tarik.

## Lisensi

MIT

---

**Dibuat dengan ❤️ oleh zaikofufu + Hermes Agent**
