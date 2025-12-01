# WhatsApp Clone Pro - Quick Start Guide

## 🚀 Run Locally (5 minutes)

### Prerequisites
```bash
Node.js 18+
npm or yarn
PostgreSQL 12+
Redis (optional)
```

### 1. Clone & Setup
```bash
git clone https://github.com/arpancodez/whatsapp-clone-pro.git
cd whatsapp-clone-pro
npm install
```

### 2. Configure Environment
```bash
# Root directory
cp .env.example .env

# Backend
cd packages/backend
cp ../../.env.example .env
```

### 3. Update .env
```env
NODE_ENV=development
PORT=5000
DATABASE_URL=postgresql://user:password@localhost:5432/whatsapp_db
JWT_SECRET=your-super-secret-key
CORS_ORIGIN=http://localhost:5173,http://localhost:3000
```

### 4. Database Setup
```bash
# Create database
psql -U postgres -c "CREATE DATABASE whatsapp_db;"

# Run migrations (when Prisma schema added)
npx prisma migrate dev
```

### 5. Start Backend
```bash
cd packages/backend
npm run dev
# Server running on http://localhost:5000
```

### 6. Start Frontend (New Terminal)
```bash
cd apps/web
npm install
npm run dev
# Frontend running on http://localhost:5173
```

## 📚 API Endpoints Available

- `GET /health` - Health check
- `GET /api/v1/status` - API status

## 🔌 WebSocket Events

Connected to Socket.io on `http://localhost:5000`:
- `user:online` - User comes online
- `message:send` - Send message
- `message:typing` - Typing indicator
- `disconnect` - User disconnects

## 🐳 Docker Deployment

```bash
docker-compose up -d
# Access on http://localhost:5173
```

## 📦 Production Build

```bash
# Build backend
cd packages/backend
npm run build
npm run start

# Build frontend
cd apps/web
npm run build
npm run preview
```

## ✅ What's Included

✓ Express.js + Socket.io server
✓ TypeScript configuration
✓ CORS & security middleware
✓ Rate limiting
✓ Logging with Winston
✓ WebSocket event handlers
✓ Structured error handling

## 🔄 Next Steps

1. Add Prisma schema for database
2. Implement authentication service
3. Add message encryption
4. Build React frontend
5. Add contact sync
6. Deploy to Vercel + Railway

## 🆘 Troubleshooting

**Port 5000 already in use:**
```bash
kill -9 $(lsof -t -i:5000)
# or
PORT=3001 npm run dev
```

**Database connection error:**
- Ensure PostgreSQL is running
- Check DATABASE_URL in .env
- Run: `psql -U postgres -d whatsapp_db` to verify

**WebSocket not connecting:**
- Check CORS_ORIGIN includes your frontend URL
- Verify Socket.io version matches client

## 📝 Quick Commands

```bash
# Install all dependencies
npm run install-all

# Run all tests
npm run test

# Format code
npm run format

# Type check
npm run typecheck
```

## 🎯 Current Status

**Backend:** ✓ Running
**Frontend:** ⏳ Coming next
**Database:** ⏳ Prisma schema needed
**Auth:** ⏳ JWT implementation needed
**Real-time:** ✓ WebSocket ready

Enjoy building! 🚀
