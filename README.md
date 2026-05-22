# 🚀 OpenGateway + Hermes Agent — Free LLM Setup Guide

> **Dapetin Xiaomi MiMo v2.5 Pro GRATIS di Hermes Agent lewat OpenGateway (gitlawb)**

[![OpenGateway](https://img.shields.io/badge/OpenGateway-gitlawb.com-000?style=for-the-badge)](https://gitlawb.com/opengateway)
[![Hermes Agent](https://img.shields.io/badge/Hermes-Agent-FFD700?style=for-the-badge)](https://github.com/NousResearch/hermes-agent)
[![License](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)](LICENSE)

---

## 📋 Table of Contents

- [Apa itu OpenGateway?](#apa-itu-opengateway)
- [Apa itu Hermes Agent?](#apa-itu-hermes-agent)
- [Prasyarat](#prasyarat)
- [Step 1: Dapatkan API Key](#step-1-dapatkan-api-key)
- [Step 2: Install Hermes Agent](#step-2-install-hermes-agent)
- [Step 3: Konfigurasi OpenGateway di Hermes](#step-3-konfigurasi-opengateway-di-hermes)
- [Step 4: Test Koneksi](#step-4-test-koneksi)
- [Step 5: Restart Gateway](#step-5-restart-gateway)
- [Available Models](#available-models)
- [Troubleshooting](#troubleshooting)
- [FAQ](#faq)

---

## Apa itu OpenGateway?

**OpenGateway** adalah open LLM inference gateway oleh [gitlawb](https://gitlawb.com). Fitur utama:

- **Satu endpoint OpenAI-compatible** — ganti base URL aja, model routing otomatis
- **Multi-provider routing** — Xiaomi MiMo, GMI Cloud, dan lebih banyak lagi
- **API key management** — generate, scope, revoke dari dashboard
- **Real-time usage dashboard** — lihat token usage lo secara live
- **GRATIS selama partnership window** — auth optional hari ini, required soon

Endpoint: `https://opengateway.gitlawb.com/v1`

## Apa itu Hermes Agent?

**Hermes Agent** adalah AI agent framework open-source oleh [Nous Research](https://nousresearch.com):

- Self-improving skills system
- Persistent memory across sessions
- Multi-platform (Telegram, Discord, Slack, WhatsApp, CLI)
- 200+ model providers
- Built-in cron scheduler
- Subagent delegation

---

## Prasyarat

| Requirement | Version |
|---|---|
| OS | Linux / macOS / WSL2 |
| Python | 3.11+ |
| Node.js | 22+ (recommended 24) |
| Internet | Yes |

---

## Step 1: Dapatkan API Key

1. Buka **https://gitlawb.com/opengateway**
2. Klik **"sign in to generate a key"**
3. Login dengan akun **X (Twitter)** lo
4. Di dashboard, klik **"Create Key"**
5. Copy key-nya (format: `ogw_live_xxxxx`)

> ⚠️ Key ini **GRATIS** selama partnership window. Auth optional hari ini, tapi bakal jadi required soon. Buruan ambil!

---

## Step 2: Install Hermes Agent

Kalau belum install Hermes:

```bash
# One-liner install
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash

# Reload shell
source ~/.bashrc

# Verify
hermes --version
```

---

## Step 3: Konfigurasi OpenGateway di Hermes

### Option A: Edit config.yaml langsung (Recommended)

```bash
# Buka config
nano ~/.hermes-2/config.yaml
```

Edit bagian `model` dan `providers`:

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
    api_key: ogw_live_YOUR_KEY_HERE  # ← Ganti dengan key lo
    name: Xiaomi MiMo
```

### Option B: Pakai CLI

```bash
# Set provider
hermes config set model.provider xiaomi

# Set model
hermes config set model.default mimo-v2.5-pro

# Set base URL
hermes config set model.base_url https://opengateway.gitlawb.com/v1

# Set API key
hermes config set providers.xiaomi.api_key ogw_live_YOUR_KEY_HERE

# Set base URL di provider juga
hermes config set providers.xiaomi.base_url https://opengateway.gitlawb.com/v1
```

### Option C: Custom Provider (kalau mau beda nama)

Kalau lo mau OpenGateway sebagai provider terpisah (bukan replace xiaomi):

```yaml
# Di ~/.hermes-2/config.yaml, tambah di bagian providers:
custom_providers:
  opengateway:
    base_url: https://opengateway.gitlawb.com/v1
    api_key: ogw_live_YOUR_KEY_HERE
    name: OpenGateway
    default_model: mimo-v2.5-pro
```

Terus set model pakai custom provider:

```bash
hermes config set model.provider custom:opengateway
```

---

## Step 4: Test Koneksi

### Test via curl (langsung)

```bash
curl https://opengateway.gitlawb.com/v1/chat/completions \
  -H "Authorization: Bearer ogw_live_YOUR_KEY_HERE" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "mimo-v2.5-pro",
    "messages": [
      {"role": "user", "content": "hello, gateway"}
    ]
  }'
```

Expected response:

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

### Test via Hermes

```bash
# Start Hermes
hermes

# Di dalam chat, ketik:
hello, test koneksi OpenGateway
```

Kalau respon berarti sukses! ✅

---

## Step 5: Restart Gateway

Kalau lo pake Hermes gateway (Telegram, Discord, dll):

```bash
# Restart gateway supaya config baru ke-load
hermes gateway restart

# Verify gateway running
hermes gateway status
```

---

## Available Models

| Model | Provider | Description |
|---|---|---|
| `mimo-v2.5-pro` | Xiaomi MiMo | Flagship, paling powerful |
| `mimo-v2.5` | Xiaomi MiMo | Standard version |
| `mimo-v2-pro` | Xiaomi MiMo | Older pro model |
| `mimo-v2-omni` | Xiaomi MiMo | Multimodal (text + vision) |
| `mimo-v2-flash` | Xiaomi MiMo | Fast & efficient |
| `mimo-v2.5-tts` | Xiaomi MiMo | Text-to-speech |
| `google/gemini-3.1-flash-lite-preview` | GMI Cloud | Google Gemini |

> 💡 **Recommended**: `mimo-v2.5-pro` — flagship model, paling banyak dipake (9.9M+ requests di global usage)

---

## Troubleshooting

### "unauthenticated" error

```bash
# Cek key bener
echo "ogw_live_YOUR_KEY_HERE" | head -c 10
# Pastiin awalannya: ogw_live_

# Test langsung
curl -s https://opengateway.gitlawb.com/v1/chat/completions \
  -H "Authorization: Bearer ogw_live_YOUR_KEY_HERE" \
  -H "Content-Type: application/json" \
  -d '{"model":"mimo-v2.5-pro","messages":[{"role":"user","content":"hi"}],"max_tokens":5}'
```

### "Not found. Use /v1/<provider>/<path>"

Endpoint yang bener: `https://opengateway.gitlawb.com/v1/chat/completions`
Bukan: `https://opengateway.gitlawb.com/v1/models` (ini gak ada)

### Model gak dikenal

Pastikan model name bener:
```bash
# ✅ Benar
"model": "mimo-v2.5-pro"

# ❌ Salah
"model": "mimo-v2.5-pro-20260422"  # ini auto-resolve dari API
```

### Context window terlalu kecil

Di config.yaml, set:
```yaml
model:
  context_length: 1000000  # 1M tokens
```

### Gateway gak mau restart

```bash
# Force kill dan restart
hermes gateway stop
hermes gateway start
```

---

## FAQ

### Berapa lama gratis-nya?

Selama **partnership window** — durasi pasti gak di-publish. Buruan ambil selagi bisa!

### Ada batas token?

Dashboard gak nampilin exact quota per key. Tapi dari global usage, gateway ini nanganin **954B+ tokens** across all users. Sejauh ini gak ada yang kena limit.

### Bisa dipake di OpenClaw juga?

Bisa! OpenClaw juga support custom OpenAI-compatible endpoint. Set base URL ke `https://opengateway.gitlawb.com/v1` dan pakai key yang sama.

### Bisa dipake di Claude Code / Codex?

Bisa, selama tool-nya support OpenAI-compatible endpoint. Tinggal ganti base URL.

### Bedanya sama langsung pake Xiaomi API?

OpenGateway nge-route lewat gateway mereka, jadi:
- Lo gak perlu punya Xiaomi API key sendiri
- Gratis selama partnership window
- Ada global usage dashboard
- Tapi tergantung availability gateway-nya

---

## Global Usage Stats

Live stats bisa dilihat di: **https://gitlawb.com/opengateway/usage/global**

| Metric | Value |
|---|---|
| Total Requests | 10.6M+ |
| Top Model | mimo-v2.5-pro (9.9M requests) |
| Total Tokens | 954B+ |
| Avg Tokens/Request | ~96K |
| Providers | Xiaomi MiMo, GMI Cloud |

---

## Contributing

PR welcome! Kalau lo punya update atau fix, buka issue atau PR.

## License

MIT

---

**Made with ❤️ by zaikofufu + Hermes Agent**

> "Is this answer immediately executable?" — if no, rewrite it. — zaikoagent 🔥
