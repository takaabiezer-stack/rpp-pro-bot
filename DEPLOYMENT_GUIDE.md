# 🚀 DEPLOYMENT GUIDE - RPP PRO BOT

## Pilih Metode Deployment

| Platform | Setup Time | Cost | Recommended |
|----------|-----------|------|-------------|
| **Railway** | 5 menit | Free tier | ✅ BEST |
| **Heroku** | 10 menit | Free (limited) | ✅ GOOD |
| **Docker + VPS** | 30 menit | $5-10/month | Advanced |
| **Cloud Run** | 15 menit | Pay-per-use | Good |

---

## ⚡ Fastest: Railway (5 menit)

### Step 1: Siapkan API Keys

#### 1.1 Telegram Bot Token

1. Chat @BotFather di Telegram
2. Kirim `/newbot`
3. Follow instructions
4. **Copy token** (akan terlihat seperti: `123456789:ABCdefGHIjkLMNOpqrSTUvwxYZ`)

#### 1.2 Claude API Key

1. Go to https://console.anthropic.com
2. Login / Sign up
3. Go to API Keys section
4. Click "Create API Key"
5. **Copy key**

#### 1.3 Canva API Key

1. Go to https://developer.canva.com
2. Login dengan Canva account
3. Create app
4. **Copy API key**

#### 1.4 MongoDB Atlas

1. Go to https://www.mongodb.com/cloud/atlas
2. Create free account
3. Create cluster
4. Get connection string (akan terlihat seperti: `mongodb+srv://user:pass@cluster.mongodb.net/dbname`)
5. **Copy connection string**

#### 1.5 Redis

Railway akan auto-provide Redis, jadi skip untuk saat ini.

---

### Step 2: Deploy ke Railway

1. **Go to https://railway.app**
2. **Sign up dengan GitHub**
3. **Connect your GitHub**
4. **Pilih repository** (fork dari repo ini dulu jika belum)
5. **Railway akan auto-detect** Node.js project
6. **Go to Variables tab** dan add:

```
TELEGRAM_BOT_TOKEN=your_token_here
CLAUDE_API_KEY=your_key_here
CANVA_API_KEY=your_key_here
MONGODB_URI=your_mongodb_connection_string
REDIS_URL=redis://localhost:6379
NODE_ENV=production
```

7. **Add Redis service:**
   - Click "Add Service"
   - Select "Redis"
   - It will auto-populate REDIS_URL

8. **Add MongoDB service:**
   - Click "Add Service"  
   - Select "MongoDB"
   - It will auto-populate MONGODB_URI

9. **Deploy** - Railway akan auto-deploy from main branch

10. **Verify** - Cek logs di Railway dashboard

✅ **Bot siap digunakan!** Kirim `/start` di Telegram

---

## 🔧 Alternative: Heroku (10 menit)

### Step 1: Install Heroku CLI

```bash
# macOS
brew tap heroku/brew && brew install heroku

# Windows / Linux
curl https://cli-assets.heroku.com/install.sh | sh

# Login
heroku login
```

### Step 2: Create Heroku App

```bash
# Create app
heroku create rpp-pro-bot

# Or dengan custom name
heroku create my-rpp-bot
```

### Step 3: Add Free Add-ons

```bash
# Redis
heroku addons:create heroku-redis:premium-0

# MongoDB (via mLab)
heroku addons:create mongolab:sandbox
```

Heroku akan auto-set environment variables.

### Step 4: Set Custom Environment Variables

```bash
heroku config:set TELEGRAM_BOT_TOKEN=your_token
heroku config:set CLAUDE_API_KEY=your_key
heroku config:set CANVA_API_KEY=your_key
heroku config:set NODE_ENV=production
```

### Step 5: Deploy

```bash
# Add Heroku remote
heroku git:remote -a your-app-name

# Deploy
git push heroku main

# View logs
heroku logs --tail
```

✅ **Bot live di Heroku!**

---

## 🐳 Advanced: Docker + VPS

### Step 1: Rent VPS

Options:
- **DigitalOcean** - $5/month
- **Linode** - $5/month
- **AWS Lightsail** - $3.50/month
- **Vultr** - $5/month

Rekomendasi specs:
- OS: Ubuntu 22.04
- RAM: 1GB (minimum)
- Storage: 20GB

### Step 2: SSH ke VPS

```bash
ssh root@your_vps_ip
# Atau jika pakai key:
ssh -i ~/.ssh/id_rsa root@your_vps_ip
```

### Step 3: Install Docker & Docker Compose

```bash
# Update system
apt-get update && apt-get upgrade -y

# Install Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh

# Install Docker Compose
curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose

# Verify
docker --version
docker-compose --version
```

### Step 4: Clone Repository

```bash
cd /opt
git clone https://github.com/yourname/rpp-pro-bot.git
cd rpp-pro-bot
```

### Step 5: Setup Environment

```bash
# Copy .env
cp .env.example .env

# Edit dengan API keys
nano .env
# Tambahkan:
# TELEGRAM_BOT_TOKEN=xxx
# CLAUDE_API_KEY=xxx
# CANVA_API_KEY=xxx
# MONGODB_URI=xxx
# REDIS_URL=xxx
```

### Step 6: Run Docker Compose

```bash
# Start services
docker-compose up -d

# View logs
docker-compose logs -f rpp-bot

# Stop services
docker-compose down
```

✅ **Bot berjalan di Docker!**

---

## 📊 Verify Deployment

Setelah deploy ke platform manapun, verify:

### 1. Test Health Check

```bash
curl https://your-app-url/health
# Should return: {"status": "ok"}
```

### 2. Test Bot di Telegram

1. Open Telegram
2. Find your bot (@botname)
3. Send `/start`
4. Should receive welcome message
5. Send `/rpp` to start RPP generation

### 3. Check Logs

```bash
# Railway
railway logs

# Heroku
heroku logs --tail

# Docker
docker-compose logs -f
```

### 4. Monitor Database

```bash
# MongoDB Atlas Dashboard
# - Check collections
# - View data

# Redis (if accessible)
redis-cli
> PING
# Should return: PONG
```

---

## 🔒 Production Checklist

Sebelum go-live:

- [ ] Set `NODE_ENV=production`
- [ ] Enable HTTPS/SSL
- [ ] Setup monitoring & alerts
- [ ] Configure backups (MongoDB)
- [ ] Setup CI/CD pipeline
- [ ] Add error logging (Sentry optional)
- [ ] Rate limiting enabled
- [ ] Secrets tidak hardcoded
- [ ] Database password kuat
- [ ] Regular security updates

---

## 📈 Scaling

Jika traffic tinggi:

**Railway:**
- Upgrade compute power di dashboard
- Auto-scaling available

**Heroku:**
- `heroku ps:type standard-1x`
- `heroku ps:scale worker=2`

**Docker VPS:**
- Upgrade instance size
- Setup load balancer (nginx)
- Database replicas

---

## 🐛 Troubleshooting Deployment

### Bot offline / tidak merespons

```bash
# Check health
curl https://your-url/health

# Check logs untuk error
heroku logs --tail  # atau railway logs

# Restart bot
heroku restart  # Heroku
railway redeploy  # Railway
docker-compose restart rpp-bot  # Docker
```

### Environment variable tidak ter-set

```bash
# Verify di Heroku
heroku config

# Verify di Railway
railway variables

# Docker
grep -i TELEGRAM_BOT_TOKEN .env
docker-compose exec rpp-bot env
```

### Database connection error

```bash
# Test MongoDB connection
mongo "your_mongodb_uri"

# Test Redis connection
redis-cli -u "your_redis_url" ping
# Should return: PONG

# Check firewall
# - Allow inbound dari IP bot
# - Check security groups
```

### Out of memory

```bash
# Check memory
heroku ps  # Heroku
docker stats  # Docker

# Fix:
# - Upgrade instance
# - Optimize code
# - Reduce cache size
```

---

## 🔄 Update Bot

### Deploy new version

**Railway:**
```bash
# Auto-deploy dari main branch
git push origin main
# Railway akan auto-redeploy
```

**Heroku:**
```bash
git push heroku main
```

**Docker:**
```bash
cd /opt/rpp-pro-bot
git pull origin main
docker-compose down
docker-compose up -d --build
```

---

## 📝 Maintenance

### Regular tasks

**Daily:**
- Monitor logs
- Check error alerts
- Verify bot responsiveness

**Weekly:**
- Check database size
- Verify backups
- Review API usage

**Monthly:**
- Update dependencies: `npm update`
- Security patches
- Performance optimization
- Analytics review

### Backup Strategy

**MongoDB:**
```bash
# Daily backup
mongodump --uri="your_uri" --out=./backups

# Restore
mongorestore --uri="your_uri" ./backups
```

**Environment variables:**
- Keep copy di secure location
- Rotate API keys quarterly

---

## 🎉 Success!

Bot kamu sekarang live dan ready untuk digunakan guru-guru Indonesia! 

**Next steps:**
1. Share bot link ke guru: `https://t.me/yourbot`
2. Monitor usage & feedback
3. Optimize berdasarkan user feedback
4. Plan feature improvements

---

## 📞 Need Help?

- **Documentation:** Check README.md
- **GitHub Issues:** Report bugs
- **Railway Docs:** https://docs.railway.app
- **Heroku Docs:** https://devcenter.heroku.com
- **Docker Docs:** https://docs.docker.com

**Happy deploying!** 🚀
