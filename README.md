# 🎓 RPP Pro Bot - Telegram RPP Generator

**Sistem otomatis untuk membuat RPP profesional dengan AI, fully differentiated, dan siap pakai.**

## 🚀 Fitur Utama

✅ **Fully Differentiated** - 3 learning paths (advanced/core/foundational)
✅ **Inclusive** - Akomodasi ABK & kebutuhan khusus siswa  
✅ **Standards-Aligned** - Kurikulum Merdeka/K13/International
✅ **Evidence-Based** - Berdasarkan kondisi siswa actual
✅ **34 Pertanyaan Terstruktur** - Capture full context pembelajaran
✅ **Export ke Canva** - 3 dokumen editable siap pakai
✅ **MongoDB Integration** - Simpan history RPP

---

## 📋 Prasyarat

- Node.js v16+ atau Docker
- Telegram Bot Token (dari @BotFather)
- Claude API Key (dari Anthropic)
- Canva API Key (dari Canva Developer)
- MongoDB (local atau cloud)
- Redis (untuk caching)

---

## 🔧 Setup Local Development

### 1. Clone & Setup

```bash
# Clone repository
git clone https://github.com/yourname/rpp-pro-bot.git
cd rpp-pro-bot

# Install dependencies
npm install

# Copy env template
cp .env.example .env

# Edit .env dengan API keys kamu
nano .env
```

### 2. Setup Environment Variables

Edit `.env`:

```env
# Telegram Bot
TELEGRAM_BOT_TOKEN=your_bot_token_from_botfather

# Claude API
CLAUDE_API_KEY=your_claude_api_key
CLAUDE_MODEL=claude-opus-4-6

# Canva API
CANVA_API_KEY=your_canva_api_key

# Database
MONGODB_URI=mongodb+srv://user:pass@cluster.mongodb.net/rpp-pro-db
REDIS_URL=redis://localhost:6379

# Server
PORT=3000
NODE_ENV=development
```

### 3. Setup MongoDB & Redis

**Option A: Local Installation**

```bash
# Install MongoDB (macOS)
brew install mongodb-community

# Install Redis (macOS)
brew install redis

# Start services
brew services start mongodb-community
brew services start redis
```

**Option B: Docker Compose**

```bash
docker-compose up -d
```

### 4. Run Bot Locally

```bash
# Development (dengan auto-reload)
npm run dev

# Production
npm start
```

**Bot siap di Telegram!** Kirim `/start` untuk test.

---

## 🚢 Deploy ke Cloud

### Option 1: Railway (Recommended)

**Paling mudah & cepat!**

1. Signup di [railway.app](https://railway.app)
2. Connect GitHub repository
3. Add environment variables:
   - `TELEGRAM_BOT_TOKEN`
   - `CLAUDE_API_KEY`
   - `CANVA_API_KEY`
4. Deploy!

```bash
# Atau via CLI
npm install -g @railway/cli
railway login
railway link
railway up
```

### Option 2: Heroku

```bash
# Login
heroku login

# Create app
heroku create rpp-pro-bot

# Add buildpack
heroku buildpacks:set heroku/nodejs

# Set environment variables
heroku config:set TELEGRAM_BOT_TOKEN=xxx
heroku config:set CLAUDE_API_KEY=xxx
heroku config:set CANVA_API_KEY=xxx
heroku config:set MONGODB_URI=xxx
heroku config:set REDIS_URL=xxx

# Deploy
git push heroku main

# View logs
heroku logs --tail
```

### Option 3: Docker & Cloud Run (GCP)

```bash
# Build image
docker build -t rpp-pro-bot .

# Push ke GCP
docker tag rpp-pro-bot gcr.io/YOUR_PROJECT/rpp-pro-bot
docker push gcr.io/YOUR_PROJECT/rpp-pro-bot

# Deploy ke Cloud Run
gcloud run deploy rpp-pro-bot \
  --image gcr.io/YOUR_PROJECT/rpp-pro-bot \
  --platform managed \
  --region asia-southeast2 \
  --set-env-vars TELEGRAM_BOT_TOKEN=xxx,CLAUDE_API_KEY=xxx
```

### Option 4: VPS (DigitalOcean, Linode, etc)

```bash
# SSH ke VPS
ssh root@your_vps_ip

# Setup Node
curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs

# Clone & setup
git clone https://github.com/yourname/rpp-pro-bot.git
cd rpp-pro-bot
npm install
cp .env.example .env
nano .env  # Edit dengan API keys

# Install PM2 untuk auto-restart
npm install -g pm2
pm2 start index.js --name "rpp-pro-bot"
pm2 startup
pm2 save

# View logs
pm2 logs rpp-pro-bot
```

---

## 📊 Database Schema

### MongoDB Collections

**rpp_history**
```javascript
{
  _id: ObjectId,
  userId: Number,
  chatId: Number,
  subject: String,
  grade: String,
  basicCompetency: String,
  learningModel: String,
  duration: Number,
  studentCount: Number,
  specialNeeds: String,
  performanceDistribution: {
    high: Number,
    average: Number,
    struggling: Number
  },
  canvaLinks: {
    rpp: { designId, shareUrl, editUrl },
    activities: { designId, shareUrl, editUrl },
    agenda: { designId, shareUrl, editUrl }
  },
  createdAt: Date,
  updatedAt: Date,
  status: String
}
```

**user_settings**
```javascript
{
  _id: ObjectId,
  userId: Number,
  preferences: {
    theme: String,
    language: String,
    notifications: Boolean
  },
  createdAt: Date,
  updatedAt: Date
}
```

---

## 🎯 Menggunakan Bot

### Chat Commands

```
/start    - Mulai bot
/rpp      - Generate RPP baru
/help     - Bantuan
/history  - Lihat RPP sebelumnya
```

### Flow

1. **Ketik `/rpp`**
2. **Jawab 34 pertanyaan** dalam 5 bagian:
   - Bagian A: Input Dasar (6Q)
   - Bagian B: Kondisi Siswa (13Q)
   - Bagian C: Strategi Pembelajaran (6Q)
   - Bagian D: Kurikulum (4Q)
   - Bagian E: Dukungan & Diferensiasi (5Q)
3. **Tunggu 30-60 detik**
4. **Terima 3 dokumen Canva:**
   - 📋 RPP Lengkap
   - ✏️ Aktivitas Siswa
   - ⏱️ Agenda Pembelajaran

---

## 🐛 Troubleshooting

### Bot tidak merespons

```bash
# Check Redis connection
redis-cli ping
# Should return: PONG

# Check MongoDB connection
mongo mongodb://localhost:27017/rpp-pro-db
# Should connect successfully

# Check bot logs
npm run dev
# Look for error messages
```

### Claude API Error

```
Error 401: Invalid API key
→ Check CLAUDE_API_KEY di .env

Error 429: Rate limit
→ Wait dan retry, atau upgrade plan
```

### Canva API Error

```
Error 400: Invalid request
→ Check CANVA_API_KEY dan format

Error 403: Forbidden
→ API key tidak punya permission untuk create designs
```

### Memory issues

```bash
# Increase Node memory
NODE_OPTIONS=--max-old-space-size=4096 npm start

# Atau di Docker
docker run -e NODE_OPTIONS="--max-old-space-size=4096" rpp-pro-bot
```

---

## 📈 Monitoring

### View Logs

```bash
# Local
npm run dev

# Railway
railway logs

# Heroku
heroku logs --tail

# PM2
pm2 logs rpp-pro-bot

# Docker
docker logs rpp-pro-bot
```

### Health Check

Bot meng-expose health endpoint:

```bash
curl http://localhost:3000/health
# Response: { "status": "ok" }
```

---

## 🔐 Security

- ✅ Environment variables untuk semua secrets
- ✅ Redis untuk session management
- ✅ MongoDB dengan authentication
- ✅ Rate limiting (built-in di Telegram)
- ✅ Input validation & sanitization
- ✅ HTTPS required untuk production

**Tips:**
- Never commit `.env` file
- Rotate API keys regularly
- Use VPN untuk development
- Enable 2FA di services
- Monitor logs untuk suspicious activity

---

## 📝 API Documentation

### Telegram Commands

```
/start        - Inisialisasi bot
/rpp          - Start RPP generation wizard
/help         - Help & documentation
/history      - Show RPP history (coming soon)
/settings     - User preferences (coming soon)
```

### Webhook (Production)

Jika menggunakan webhook instead of polling:

```bash
# Set webhook
curl -X POST https://api.telegram.org/botTOKEN/setWebhook \
  -d "url=https://yourdomain.com/telegram/webhook"

# Remove webhook
curl https://api.telegram.org/botTOKEN/deleteWebhook
```

---

## 🤝 Contributing

Kontribusi sangat welcome!

```bash
# Fork repository
# Create feature branch
git checkout -b feature/amazing-feature

# Commit changes
git commit -m 'Add amazing feature'

# Push to branch
git push origin feature/amazing-feature

# Open Pull Request
```

---

## 📄 License

MIT License - feel free to use for commercial projects

---

## 📞 Support

- **Email:** support@example.com
- **GitHub Issues:** [Create issue](https://github.com/yourname/rpp-pro-bot/issues)
- **Telegram Group:** [Join group](https://t.me/rppprobot)
- **Documentation:** [Full docs](https://docs.example.com)

---

## 🙏 Acknowledgments

- Claude AI by Anthropic
- Telegram Bot API
- Canva API
- Node.js community
- Para guru Indonesia yang luar biasa! 🎓

---

**Happy teaching! 🚀**

*RPP Pro Bot - Making great lesson plans effortless*
