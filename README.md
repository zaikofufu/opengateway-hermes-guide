# 🚀 Panduan Lengkap: OpenGateway + Hermes Agent

> **Mendapatkan akses gratis ke Xiaomi MiMo v2.5 Pro melalui OpenGateway (gitlawb)**

[![OpenGateway](https://img.shields.io/badge/OpenGateway-gitlawb.com-000?style=for-the-badge)](https://gitlawb.com/opengateway)
[![Hermes Agent](https://img.shields.io/badge/Hermes-Agent-FFD700?style=for-the-badge)](https://github.com/NousResearch/hermes-agent)
[![License](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)](LICENSE)

---

## Daftar Isi

- [Apa Itu OpenGateway?](#apa-itu-opengateway)
- [Persyaratan](#persyaratan)
- [Langkah 1: Dapatkan API Key](#langkah-1-dapatkan-api-key)
- [Panduan Platform](#panduan-platform)
  - [Hermes Agent (Lokal)](#1-hermes-agent-lokal)
  - [Hermes Agent (Gateway — Telegram, Discord, dll.)](#2-hermes-agent-gateway--telegram-discord-dll)
  - [Hermes Agent (VPS/Cloud)](#3-hermes-agent-vpscloud)
  - [OpenClaw](#4-openclaw)
  - [Claude Code](#5-claude-code)
  - [Codex CLI (OpenAI)](#6-codex-cli-openai)
  - [Continue.dev (VS Code)](#7continuedev--vs-code)
  - [Cline (VS Code)](#8cline--vs-code)
  - [Cursor IDE](#9cursor-ide)
  - [LobeChat](#10lobechat)
  - [Open WebUI](#11open-webui)
  - [LibreChat](#12librechat)
  - [ChatBox (Desktop)](#13chatbox--desktop)
  - [Python (OpenAI SDK)](#14python--openai-sdk)
  - [LangChain / LlamaIndex](#15langchain--llamaindex)
  - [cURL / HTTP Murni](#16curl--http-murni)
- [Model yang Tersedia](#model-yang-tersedia)
- [Pemecahan Masalah](#pemecahan-masalah)
- [Pertanyaan yang Sering Diajukan](#pertanyaan-yang-sering-diajukan)

---

## Apa Itu OpenGateway?

**OpenGateway** adalah gerbang inferensi LLM terbuka oleh [gitlawb](https://gitlawb.com):

- **Satu endpoint kompatibel OpenAI** — ganti URL dasar, routing model otomatis
- **Routing multi-penyedia** — Xiaomi MiMo, GMI Cloud, dan lainnya
- **Manajemen API Key** — buat, atur, cabut dari dasbor
- **Dasbor penggunaan real-time** — pantau token usage secara langsung
- **Gratis selama jendela kemitraan**

| Nilai | Keterangan |
|---|---|
| Endpoint | `https://opengateway.gitlawb.com/v1` |
| Protokol | OpenAI-compatible (`/v1/chat/completions`) |
| Autentikasi | Bearer token (`Authorization: Bearer ogw_live_xxx`) |
| Dasbor | https://gitlawb.com/opengateway/dashboard |
| Penggunaan Global | https://gitlawb.com/opengateway/usage/global |

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

1. Buka **https://gitlawb.com/opengateway**
2. Klik **"sign in to generate a key"**
3. Masuk menggunakan akun **X (Twitter)**
4. Pada dasbor, klik **"Create Key"**
5. Salin kunci (format: `ogw_live_xxxxx`)

> ⚠️ Kunci ini **gratis** selama jendela kemitraan. Segera dapatkan!

Setelah mendapat kunci, simpan sebagai variabel lingkungan agar mudah digunakan:

```bash
# Tambahkan ke ~/.bashrc atau ~/.zshrc
export OPENGATEWAY_KEY="ogw_live_KUNCI_ANDA_DI_SINI"
source ~/.bashrc
```

---

## Panduan Platform

### 1. Hermes Agent (Lokal)

Jalankan Hermes langsung di terminal lokal.

**Instalasi (jika belum):**

```bash
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
source ~/.bashrc
```

**Konfigurasi — Opsi A: Edit config.yaml (Disarankan)**

```bash
nano ~/.hermes-2/config.yaml
```

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
    api_key: ogw_live_KUNCI_ANDA_DI_SINI
    name: Xiaomi MiMo
```

**Konfigurasi — Opsi B: CLI**

```bash
hermes config set model.provider xiaomi
hermes config set model.default mimo-v2.5-pro
hermes config set model.base_url https://opengateway.gitlawb.com/v1
hermes config set providers.xiaomi.api_key ogw_live_KUNCI_ANDA_DI_SINI
hermes config set providers.xiaomi.base_url https://opengateway.gitlawb.com/v1
hermes config set model.context_length 1000000
```

**Konfigurasi — Opsi C: Penyedia Kustom (tanpa mengganti xiaomi)**

```yaml
# Tambahkan di config.yaml
custom_providers:
  opengateway:
    base_url: https://opengateway.gitlawb.com/v1
    api_key: ogw_live_KUNCI_ANDA_DI_SINI
    name: OpenGateway
    default_model: mimo-v2.5-pro
```

```bash
hermes config set model.provider custom:opengateway
```

**Uji dan Jalankan:**

```bash
hermes
# Ketik di dalam chat: halo, uji koneksi
```

---

### 2. Hermes Agent (Gateway — Telegram, Discord, dll.)

Jalankan Hermes sebagai server yang merespons pesan dari platform perpesanan.

**Langkah 1: Konfigurasi OpenGateway (sama seperti di atas)**

**Langkah 2: Atur platform perpesanan**

```bash
# Wizard interaktif untuk mengatur Telegram, Discord, dll.
hermes gateway setup
```

**Langkah 3: Instal dan jalankan gateway**

```bash
# Instal sebagai layanan systemd (berjalan otomatis saat boot)
hermes gateway install

# Jalankan
hermes gateway start

# Periksa status
hermes gateway status
```

**Langkah 4: Mulai ulang setelah perubahan konfigurasi**

```bash
hermes gateway restart
```

Sekarang lo bisa chat dengan Hermes lewat Telegram/Discord/WhatsApp dan dia akan merespons menggunakan OpenGateway!

**Platform yang didukung gateway:**
- Telegram
- Discord
- Slack
- WhatsApp
- Signal
- Matrix
- Email
- Dan lainnya

---

### 3. Hermes Agent (VPS/Cloud)

Jalankan Hermes di server cloud agar bisa diakses dari mana saja.

**Langkah 1: Siapkan VPS**

```bash
# SSH ke VPS lo
ssh user@vps-ip

# Instal Hermes
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
source ~/.bashrc
```

**Langkah 2: Konfigurasi OpenGateway**

```bash
hermes config set model.provider xiaomi
hermes config set model.default mimo-v2.5-pro
hermes config set model.base_url https://opengateway.gitlawb.com/v1
hermes config set providers.xiaomi.api_key ogw_live_KUNCI_ANDA_DI_SINI
hermes config set providers.xiaomi.base_url https://opengateway.gitlawb.com/v1
```

**Langkah 3: Jalankan gateway**

```bash
hermes gateway install
hermes gateway start
```

**Langkah 4 (Opsional): Atur Telegram bot untuk akses dari mana saja**

```bash
hermes gateway setup
# Pilih Telegram, masukkan bot token
```

Sekarang lo bisa chat dengan Hermes dari Telegram di HP lo, sementara dia berjalan di VPS!

---

### 4. OpenClaw

OpenClaw adalah AI agent platform lain yang juga kompatibel dengan endpoint OpenAI.

**Konfigurasi:**

Di file konfigurasi OpenClaw (biasanya `~/.openclaw/config.yaml`):

```yaml
providers:
  - name: opengateway
    type: openai
    base_url: https://opengateway.gitlawb.com/v1
    api_key: ogw_live_KUNCI_ANDA_DI_SINI
    default_model: mimo-v2.5-pro
```

Atau melalui environment variable:

```bash
export OPENAI_API_BASE=https://opengateway.gitlawb.com/v1
export OPENAI_API_KEY=ogw_live_KUNCI_ANDA_DI_SINI
```

**Migrasi dari OpenClaw ke Hermes:**

```bash
hermes claw migrate
```

---

### 5. Claude Code

Claude Code oleh Anthropic mendukung custom API endpoints.

**Konfigurasi:**

```bash
# Set environment variable
export ANTHROPIC_BASE_URL=https://opengateway.gitlawb.com/v1
export ANTHROPIC_API_KEY=ogw_live_KUNCI_ANDA_DI_SINI

# Jalankan Claude Code
claude
```

> ⚠️ Claude Code dirancang untuk model Anthropic. Kompatibilitas dengan model lain mungkin bervariasi.

---

### 6. Codex CLI (OpenAI)

Codex CLI oleh OpenAI mendukung custom base URL.

**Konfigurasi:**

```bash
# Set environment variable
export OPENAI_BASE_URL=https://opengateway.gitlawb.com/v1
export OPENAI_API_KEY=ogw_live_KUNCI_ANDA_DI_SINI

# Jalankan Codex
codex
```

Atau di file konfigurasi Codex (`~/.codex/config.toml`):

```toml
model = "mimo-v2.5-pro"
provider = "openai"
```

---

### 7. Continue.dev (VS Code)

Continue adalah ekstensi AI untuk VS Code.

**Langkah 1:** Instal ekstensi "Continue" dari VS Code Marketplace

**Langkah 2:** Edit `~/.continue/config.json`:

```json
{
  "models": [
    {
      "title": "MiMo v2.5 Pro (OpenGateway)",
      "provider": "openai",
      "model": "mimo-v2.5-pro",
      "apiBase": "https://opengateway.gitlawb.com/v1",
      "apiKey": "ogw_live_KUNCI_ANDA_DI_SINI"
    }
  ],
  "tabAutocompleteModel": {
    "title": "MiMo v2.5 Flash",
    "provider": "openai",
    "model": "mimo-v2-flash",
    "apiBase": "https://opengateway.gitlawb.com/v1",
    "apiKey": "ogw_live_KUNCI_ANDA_DI_SINI"
  }
}
```

---

### 8. Cline (VS Code)

Cline adalah ekstensi AI coding untuk VS Code.

**Langkah 1:** Instal ekstensi "Cline" dari VS Code Marketplace

**Langkah 2:** Buka pengaturan Cline, pilih provider "OpenAI Compatible"

**Langkah 3:** Isi konfigurasi:

| Field | Nilai |
|---|---|
| Base URL | `https://opengateway.gitlawb.com/v1` |
| API Key | `ogw_live_KUNCI_ANDA_DI_SINI` |
| Model | `mimo-v2.5-pro` |

---

### 9. Cursor IDE

Cursor mendukung custom API endpoints.

**Langkah 1:** Buka Cursor → Settings → Models

**Langkah 2:** Klik "OpenAI API Key" dan masukkan:

| Field | Nilai |
|---|---|
| API Key | `ogw_live_KUNCI_ANDA_DI_SINI` |

**Langkah 3:** Override OpenAI Base URL:

```
https://opengateway.gitlawb.com/v1
```

**Langkah 4:** Tambahkan model `mimo-v2.5-pro` ke daftar model

---

### 10. LobeChat

LobeChat adalah UI chat sumber terbuka.

**Langkah 1:** Buka LobeChat → Settings → Language Model

**Langkah 2:** Aktifkan "OpenAI" dan isi:

| Field | Nilai |
|---|---|
| OpenAI API Key | `ogw_live_KUNCI_ANDA_DI_SINI` |
| API Proxy URL | `https://opengateway.gitlawb.com/v1` |

**Langkah 3:** Di chat, pilih model `mimo-v2.5-pro`

**Self-hosted (Docker):**

```bash
docker run -d -p 3210:3210 \
  -e OPENAI_API_KEY=ogw_live_KUNCI_ANDA_DI_SINI \
  -e OPENAI_PROXY_URL=https://opengateway.gitlawb.com/v1 \
  lobehub/lobe-chat
```

---

### 11. Open WebUI

Open WebUI adalah antarmuka ChatGPT yang di-hosting sendiri.

**Docker:**

```bash
docker run -d -p 3000:8080 \
  -e OPENAI_API_KEY=ogw_live_KUNCI_ANDA_DI_SINI \
  -e OPENAI_API_BASE_URL=https://opengateway.gitlawb.com/v1 \
  ghcr.io/open-webui/open-webui:main
```

**Setelah berjalan:**
1. Buka `http://localhost:3000`
2. Buka Settings → Connections
3. Tambahkan OpenAI API dengan URL dan kunci di atas
4. Pilih model `mimo-v2.5-pro` di chat

---

### 12. LibreChat

LibreChat adalah klon ChatGPT sumber terbuka.

**Konfigurasi di `.env`:**

```env
OPENAI_API_KEY=ogw_live_KUNCI_ANDA_DI_SINI
OPENAI_BASE_URL=https://opengateway.gitlawb.com/v1
```

**Atau di `librechat.yaml`:**

```yaml
endpoints:
  custom:
    - name: "OpenGateway"
      apiKey: "ogw_live_KUNCI_ANDA_DI_SINI"
      baseURL: "https://opengateway.gitlawb.com/v1"
      models:
        default: ["mimo-v2.5-pro"]
      titleConvo: true
      titleModel: "mimo-v2-flash"
```

---

### 13. ChatBox (Desktop)

ChatBox adalah klien AI desktop lintas platform.

**Langkah 1:** Unduh dari https://chatboxai.app

**Langkah 2:** Buka Settings → Model Provider

**Langkah 3:** Pilih "OpenAI API" dan isi:

| Field | Nilai |
|---|---|
| API Key | `ogw_live_KUNCI_ANDA_DI_SINI` |
| API Host | `https://opengateway.gitlawb.com/v1` |

**Langkah 4:** Pilih model `mimo-v2.5-pro`

---

### 14. Python (OpenAI SDK)

Gunakan pustaka OpenAI Python untuk akses programmatik.

**Instalasi:**

```bash
pip install openai
```

**Kode:**

```python
from openai import OpenAI

client = OpenAI(
    base_url="https://opengateway.gitlawb.com/v1",
    api_key="ogw_live_KUNCI_ANDA_DI_SINI"
)

response = client.chat.completions.create(
    model="mimo-v2.5-pro",
    messages=[
        {"role": "user", "content": "Jelaskan relativitas dalam satu paragraf."}
    ],
    max_tokens=500
)

print(response.choices[0].message.content)
```

**Streaming:**

```python
stream = client.chat.completions.create(
    model="mimo-v2.5-pro",
    messages=[{"role": "user", "content": "Ceritakan sebuah dongeng."}],
    stream=True
)

for chunk in stream:
    if chunk.choices[0].delta.content:
        print(chunk.choices[0].delta.content, end="")
```

---

### 15. LangChain / LlamaIndex

**LangChain:**

```python
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(
    base_url="https://opengateway.gitlawb.com/v1",
    api_key="ogw_live_KUNCI_ANDA_DI_SINI",
    model="mimo-v2.5-pro"
)

response = llm.invoke("Apa itu machine learning?")
print(response.content)
```

**LlamaIndex:**

```python
from llama_index.llms.openai import OpenAI

llm = OpenAI(
    api_base="https://opengateway.gitlawb.com/v1",
    api_key="ogw_live_KUNCI_ANDA_DI_SINI",
    model="mimo-v2.5-pro"
)

response = llm.complete("Jelaskan cara kerja transformer.")
print(response)
```

---

### 16. cURL / HTTP Murni

Dasar dari semua integrasi — permintaan HTTP langsung.

**Permintaan dasar:**

```bash
curl https://opengateway.gitlawb.com/v1/chat/completions \
  -H "Authorization: Bearer ogw_live_KUNCI_ANDA_DI_SINI" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "mimo-v2.5-pro",
    "messages": [
      {"role": "system", "content": "Anda adalah asisten yang membantu."},
      {"role": "user", "content": "Apa ibukota Indonesia?"}
    ],
    "temperature": 0.7,
    "max_tokens": 500
  }'
```

**Streaming (Server-Sent Events):**

```bash
curl https://opengateway.gitlawb.com/v1/chat/completions \
  -H "Authorization: Bearer ogw_live_KUNCI_ANDA_DI_SINI" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "mimo-v2.5-pro",
    "messages": [{"role": "user", "content": "Hitung 1 sampai 10"}],
    "stream": true
  }'
```

**Dengan jq untuk parsing JSON:**

```bash
curl -s https://opengateway.gitlawb.com/v1/chat/completions \
  -H "Authorization: Bearer $OPENGATEWAY_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model":"mimo-v2.5-pro","messages":[{"role":"user","content":"halo"}]}' \
  | jq -r '.choices[0].message.content'
```

---

## Model yang Tersedia

| Model | Penyedia | Keterangan | Kecepatan |
|---|---|---|---|
| `mimo-v2.5-pro` | Xiaomi MiMo | Unggulan, paling canggih | Sedang |
| `mimo-v2.5` | Xiaomi MiMo | Versi standar | Sedang |
| `mimo-v2-pro` | Xiaomi MiMo | Pro generasi sebelumnya | Sedang |
| `mimo-v2-omni` | Xiaomi MiMo | Multimodal (teks + visi) | Sedang |
| `mimo-v2-flash` | Xiaomi MiMo | Cepat dan efisien | Cepat |
| `mimo-v2.5-tts` | Xiaomi MiMo | Teks ke ucapan | — |
| `google/gemini-3.1-flash-lite-preview` | GMI Cloud | Google Gemini | Cepat |

> 💡 **Rekomendasi**: Gunakan `mimo-v2.5-pro` untuk tugas kompleks dan `mimo-v2-flash` untuk tugas ringan yang butuh kecepatan.

---

## Pemecahan Masalah

### Kesalahan "unauthenticated"

```bash
# Pastikan kunci benar (harus diawali ogw_live_)
echo "$OPENGATEWAY_KEY" | head -c 10

# Uji langsung
curl -s https://opengateway.gitlawb.com/v1/chat/completions \
  -H "Authorization: Bearer $OPENGATEWAY_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model":"mimo-v2.5-pro","messages":[{"role":"user","content":"tes"}],"max_tokens":5}'
```

### Pesan "Not found"

Endpoint yang benar adalah `https://opengateway.gitlawb.com/v1/chat/completions`. Endpoint `/v1/models` tidak tersedia.

### Model tidak dikenali

Gunakan nama pendek: `mimo-v2.5-pro`, bukan `xiaomi/mimo-v2.5-pro-20260422`.

### Gateway Hermes tidak merespons

```bash
hermes gateway restart
hermes gateway status
```

### Koneksi terputus di VPS

```bash
# Jalankan dengan nohup atau screen
hermes gateway run &

# Atau gunakan systemd
hermes gateway install
hermes gateway start
```

---

## Pertanyaan yang Sering Diajukan

### Berapa lama masa gratisnya?

Selama **jendela kemitraan** — durasi pasti tidak dipublikasikan. Segera manfaatkan!

### Apakah ada batas token?

Dasbor tidak menampilkan batas eksplisit. Gateway ini telah menangani **lebih dari 954 miliar token** dari seluruh pengguna tanpa laporan pembatasan.

### Platform mana yang kompatibel?

Semua platform yang mendukung endpoint kompatibel OpenAI. Ini mencakup hampir semua alat AI populer saat ini.

### Apakah aman menggunakan kunci ini?

Kunci disimpan di mesin Anda sendiri. OpenGateway hanya merutekan permintaan ke penyedia model — mereka tidak menyimpan isi percakapan Anda.

### Bisakah saya menggunakan beberapa model sekaligus?

Ya. Anda dapat mengonfigurasi model default dan model cadangan (fallback) di Hermes:

```bash
hermes config set model.default mimo-v2.5-pro
hermes fallback add mimo-v2-flash
```

---

## Statistik Penggunaan Global

Pantau secara real-time di: **https://gitlawb.com/opengateway/usage/global**

| Metrik | Nilai |
|---|---|
| Total Permintaan | Lebih dari 10,6 juta |
| Model Terpopuler | mimo-v2.5-pro (9,9 juta permintaan) |
| Total Token | Lebih dari 954 miliar |
| Rata-rata Token/Permintaan | Sekitar 96 ribu |
| Penyedia | Xiaomi MiMo, GMI Cloud |

---

## Berkontribusi

Kontribusi sangat diperbuka! Silakan buka isu atau kirimkan permintaan tarik.

## Lisensi

MIT

---

**Dibuat dengan ❤️ oleh zaikofufu + Hermes Agent**
