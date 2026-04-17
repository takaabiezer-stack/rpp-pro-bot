# ⚡ QUICK START - Deploy RPP Pro Bot dalam 5 Menit

## 🎯 Target: Bot aktif di Telegram dalam 5 menit

---

## ✅ Checklist API Keys (Siapkan dulu!)

Ambil 3 API keys ini sebelum mulai:

### 1️⃣ Telegram Bot Token
```
👉 Chat @BotFather di Telegram
→ /newbot
→ Follow prompts
→ Copy token (akan diberikan di akhir)
```

### 2️⃣ Claude API Key
```
👉 https://console.anthropic.com
→ Login
→ API Keys section
→ Create → Copy key
```

### 3️⃣ Canva API Key
```
👉 https://developer.canva.com
→ Login
→ Create app
→ Copy API key
```

---

## 🚀 Deploy dalam 3 Langkah

### LANGKAH 1: Fork Repository (1 menit)

1. Go to: https://github.com/yourname/rpp-pro-bot
2. Click **Fork** (top right)
3. GitHub akan copy repo ke akun kamu

---

### LANGKAH 2: Setup Railway (2 menit)

1. Go to: https://railway.app
2. Click **"Create New Project"**
3. Click **"Deploy from GitHub repo"**
4. **Authorize** GitHub
5. **Select forked repository** (rpp-pro-bot)
6. Railway akan auto-detect Node.js
7. Tunggu build selesai (usually 2-3 menit)

---

### LANGKAH 3: Add Environment Variables (2 menit)

1. Di Railway dashboard, klik **Variables** tab
2. Click **"Add Variable"** dan fill in:

```
TELEGRAM_BOT_TOKEN = [paste token dari @BotFather]
CLAUDE_API_KEY = [paste Claude API key]
CANVA_API_KEY = [paste Canva API key]
NODE_ENV = production
```

3. **Click "Generate" untuk Redis** (Railway otomatis set REDIS_URL)
4. **Click "Generate" untuk MongoDB** (Railway otomatis set MONGODB_URI)

5. Click **Deploy**

---

## ✨ Done! Bot Sudah Live!

Cek di Railway dashboard - tunggu sampai status "Success" (green).

Sekarang test di Telegram:
1. Search bot kamu: `@[botname]`
2. Click **Start** atau kirim `/start`
3. Kirim `/rpp` untuk mulai generate RPP

---

## 🎓 Contoh: Bagaimana Guru Menggunakan Bot?

```
Guru: /rpp
Bot: Halo! Saya siap bantu buat RPP.
     Pertanyaan 1: Mata pelajaran?
     
Guru: Matematika
Bot: Pertanyaan 2: Kelas berapa?

Guru: Kelas 7

Guru: [jawab 32 pertanyaan lebih]

Bot: [tunggu 30 detik]
Bot: ✅ RPP SIAP!
    🔗 RPP Lengkap
    🔗 Aktivitas Siswa
    🔗 Agenda Pembelajaran
    
Guru: [Buka dokumen di Canva, edit, download, cetak]
```

---

## 🆘 Troubleshooting Cepat

### Bot tidak merespons
- ✅ Cek di Railway: Status harus "Success"
- ✅ Cek Telegram bot token benar
- ✅ Tunggu 1 menit setelah deploy

### Environment variables error
- ✅ Double-check spelling (case-sensitive!)
- ✅ Jangan ada spaces di awal/akhir
- ✅ Redeploy setelah edit variables

### Deployment gagal
- ✅ Cek logs di Railway (click "Logs" tab)
- ✅ Error akan terlihat di sana
- ✅ Common issues: API key invalid, REDIS/MONGO not connected

---

## 📚 Dokumentasi Lengkap

Setelah bot berjalan, baca:
- `README.md` - Dokumentasi lengkap
- `DEPLOYMENT_GUIDE.md` - Opsi deployment lain
- `index.js` - Source code utama
- `data/questions.js` - Semua 34 pertanyaan

---

## 🎉 Selamat!

Bot kamu sekarang siap membantu guru-guru membuat RPP berkualitas!

**Share bot link:**
```
https://t.me/[botname]
```

---

## 🚀 Next Steps (Optional)

### Tambah custom features:
- [ ] Command `/history` - lihat RPP sebelumnya
- [ ] Command `/settings` - user preferences
- [ ] Share to WhatsApp / Google Drive
- [ ] Export ke format lain (DOCX, PDF)

### Monitor & maintain:
- [ ] Setup error logging (Sentry)
- [ ] Monitor API usage
- [ ] Collect user feedback
- [ ] Plan improvements

---

## 💬 Need Help?

- **GitHub:** Create issue di repo
- **Telegram:** Join bot user group
- **Email:** support@example.com

---

**Happy teaching! 🎓**

*RPP Pro Bot - Membuat perencanaan pembelajaran menjadi mudah*
