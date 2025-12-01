# Deployment Guide - WhatsApp Clone Pro

## 🚀 Deploy Backend to Railway/Heroku

### 1. Railway Deployment

```bash
# Install Railway CLI
npm i -g @railway/cli

# Login
railway login

# Create project
railway init

# Configure environment
railway variables set NODE_ENV=production
railway variables set JWT_SECRET=your-secret
railway variables set DATABASE_URL=postgresql://...

# Deploy
git push
```

### 2. Heroku Deployment

```bash
# Login
heroku login

# Create app
heroku create whatsapp-clone-pro

# Set environment
heroku config:set NODE_ENV=production
heroku config:set JWT_SECRET=your-secret

# Deploy
git push heroku main

# Check logs
heroku logs --tail
```

## 🌐 Deploy Frontend to Vercel

### 1. Vercel CLI

```bash
cd apps/web
npm i -g vercel
vercel
# Follow prompts
```

### 2. Vercel Dashboard

- Go to vercel.com
- Import whatsapp-clone-pro repo
- Set build command: `npm run build`
- Set output dir: `dist`
- Deploy!

### 3. Configure Environment Variables

In Vercel dashboard:

```
VITE_API_URL=https://whatsapp-clone-pro.railway.app/api
VITE_WS_URL=https://whatsapp-clone-pro.railway.app
VITE_GOOGLE_CLIENT_ID=your-client-id
```

## 🐳 Docker Deployment

### 1. Build Docker Images

```bash
# Backend
docker build -t whatsapp-clone-backend:latest -f docker/Dockerfile.backend .

# Frontend
docker build -t whatsapp-clone-web:latest -f docker/Dockerfile.frontend .
```

### 2. Docker Compose

```bash
docker-compose -f docker/docker-compose.yml up -d
```

## 📊 Database Setup

### 1. Create PostgreSQL Database

```bash
# Local
psql -U postgres -c "CREATE DATABASE whatsapp_db;"

# Railway/Cloud
# Use provider dashboard to create PostgreSQL service
```

### 2. Run Migrations

```bash
npx prisma migrate deploy
npx prisma db seed
```

## ✅ Post-Deployment Checklist

- [ ] Backend API responding on production URL
- [ ] WebSocket connecting successfully
- [ ] Database migrations completed
- [ ] Environment variables configured
- [ ] Frontend loading from production API
- [ ] CORS properly configured
- [ ] SSL/HTTPS enabled
- [ ] Monitor backend logs for errors
- [ ] Test authentication flow
- [ ] Test real-time messaging

## 🔍 Monitoring & Logs

```bash
# Railway
railway logs

# Heroku
heroku logs --tail

# Docker
docker logs -f whatsapp-clone-backend
```

## 🔐 Security Checklist

- [ ] JWT_SECRET is strong & unique
- [ ] CORS_ORIGIN restricted
- [ ] HTTPS enabled
- [ ] API Rate limiting active
- [ ] Database backups configured
- [ ] Error messages don't expose internals
- [ ] Sensitive data not in logs

## 🆘 Troubleshooting

**WebSocket connection failing:**
- Check CORS_ORIGIN includes frontend domain
- Verify WebSocket protocol (ws:// or wss://)
- Check firewall rules

**Database connection timeout:**
- Verify DATABASE_URL is correct
- Check database is running
- Confirm network access

**Build failing on Vercel:**
- Check node version matches
- Verify all env vars set
- Check build logs

## 📈 Scaling Considerations

- Use Redis for session caching
- Implement horizontal scaling
- Set up load balancer
- Configure CDN for static assets
- Monitor memory & CPU usage
- Implement auto-scaling policies

Deployment complete! 🎉
