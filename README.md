# WhatsApp Clone Pro

A production-ready WhatsApp clone built with TypeScript, React, Node.js, and PostgreSQL. Features real-time messaging, contact sync, WebSocket support, WhatsApp Web accessibility, and end-to-end encryption.

## Features

✅ **Real-time Messaging** - WebSocket-powered instant messaging
✅ **Contact Sync** - Auto-sync contacts from device
✅ **WhatsApp Web** - Desktop web interface support
✅ **Authentication** - JWT-based phone/email verification
✅ **Media Sharing** - Images, videos, documents, voice notes
✅ **Group Chat** - Create and manage group conversations
✅ **End-to-End Encryption** - TweetNaCl.js for message encryption
✅ **Typing Indicators** - Real-time typing status
✅ **Online Status** - Live user availability
✅ **Message Status** - Sent, Delivered, Read receipts
✅ **Call Integration** - Voice/Video call support via WebRTC
✅ **Push Notifications** - FCM integration for alerts

## Project Structure

```
whatsapp-clone-pro/
├── apps/
│   ├── mobile/                 # React Native mobile app
│   │   ├── src/
│   │   │   ├── screens/
│   │   │   ├── components/
│   │   │   ├── services/
│   │   │   └── utils/
│   │   ├── package.json
│   │   └── app.json
│   │
│   ├── web/                    # React web application
│   │   ├── src/
│   │   │   ├── pages/
│   │   │   ├── components/
│   │   │   ├── contexts/
│   │   │   ├── hooks/
│   │   │   ├── services/
│   │   │   ├── stores/
│   │   │   ├── styles/
│   │   │   └── utils/
│   │   ├── public/
│   │   ├── package.json
│   │   └── tsconfig.json
│   │
│   └── desktop/                # WhatsApp Web (Electron)
│       ├── src/
│       ├── preload/
│       ├── public/
│       ├── package.json
│       └── tsconfig.json
│
├── packages/
│   ├── backend/                # Express.js backend server
│   │   ├── src/
│   │   │   ├── api/
│   │   │   │   ├── routes/
│   │   │   │   ├── controllers/
│   │   │   │   └── middleware/
│   │   │   ├── services/
│   │   │   │   ├── auth.service.ts
│   │   │   │   ├── message.service.ts
│   │   │   │   ├── user.service.ts
│   │   │   │   ├── group.service.ts
│   │   │   │   └── websocket.service.ts
│   │   │   ├── models/
│   │   │   │   ├── user.model.ts
│   │   │   │   ├── message.model.ts
│   │   │   │   ├── chat.model.ts
│   │   │   │   └── group.model.ts
│   │   │   ├── db/
│   │   │   │   ├── connection.ts
│   │   │   │   └── migrations/
│   │   │   ├── websocket/
│   │   │   │   └── socket.ts
│   │   │   ├── utils/
│   │   │   ├── config/
│   │   │   └── server.ts
│   │   ├── .env.example
│   │   ├── package.json
│   │   └── tsconfig.json
│   │
│   ├── shared/                 # Shared types and utilities
│   │   ├── src/
│   │   │   ├── types/
│   │   │   │   ├── user.ts
│   │   │   │   ├── message.ts
│   │   │   │   ├── chat.ts
│   │   │   │   └── socket.ts
│   │   │   ├── constants/
│   │   │   └── utils/
│   │   ├── package.json
│   │   └── tsconfig.json
│   │
│   └── crypto/                 # Encryption utilities
│       ├── src/
│       │   ├── encryption.ts
│       │   ├── decryption.ts
│       │   └── keys.ts
│       ├── package.json
│       └── tsconfig.json
│
├── docker/                     # Docker configuration
│   ├── Dockerfile.backend
│   ├── Dockerfile.frontend
│   └── docker-compose.yml
│
├── .github/
│   └── workflows/
│       ├── test.yml
│       ├── build.yml
│       └── deploy.yml
│
├── .env.example
├── package.json
├── tsconfig.base.json
├── nx.json                     # NX monorepo config
└── README.md
```

## Tech Stack

### Frontend
- **Web**: React 18 + TypeScript + Vite + Tailwind CSS
- **Mobile**: React Native + TypeScript
- **Desktop**: Electron + React
- **State Management**: Zustand / Jotai
- **HTTP Client**: Axios / TanStack Query
- **UI Components**: shadcn/ui, React Icons
- **Styling**: Tailwind CSS + Radix UI
- **WebSocket**: socket.io-client

### Backend
- **Runtime**: Node.js 18+
- **Framework**: Express.js
- **Language**: TypeScript
- **Database**: PostgreSQL with Prisma ORM
- **Cache**: Redis
- **WebSocket**: Socket.io
- **Authentication**: JWT + OAuth2
- **File Upload**: AWS S3 / Cloudinary
- **Encryption**: TweetNaCl.js (NaCl)

### DevOps
- **Container**: Docker & Docker Compose
- **Deployment**: Vercel (Frontend) + Railway/Heroku (Backend)
- **CI/CD**: GitHub Actions
- **Monorepo**: NX
- **Package Manager**: npm workspaces / pnpm

## Installation

### Prerequisites
```bash
Node.js 18+ or later
npm or pnpm
PostgreSQL 12+
Redis (optional)
```

### Clone Repository
```bash
git clone https://github.com/arpancodez/whatsapp-clone-pro.git
cd whatsapp-clone-pro
```

### Install Dependencies
```bash
npm install
# or
pnpm install
```

### Setup Environment Variables

**Backend** (.env)
```env
# Server
NODE_ENV=development
PORT=5000
SERVER_URL=http://localhost:5000

# Database
DATABASE_URL=postgresql://user:password@localhost:5432/whatsapp_db

# JWT
JWT_SECRET=your_jwt_secret_key
JWT_EXPIRE=7d

# Redis
REDIS_URL=redis://localhost:6379

# AWS S3
AWS_ACCESS_KEY_ID=your_key
AWS_SECRET_ACCESS_KEY=your_secret
AWS_S3_BUCKET=your_bucket

# OAuth (Google, Apple)
GOOGLE_CLIENT_ID=your_client_id
GOOGLE_CLIENT_SECRET=your_secret

# Encryption
ENCRYPTION_KEY=your_32_byte_encryption_key

# FCM (Push Notifications)
FCM_PROJECT_ID=your_project_id
FCM_PRIVATE_KEY=your_private_key
FCM_CLIENT_EMAIL=your_client_email
```

**Frontend** (.env.local)
```env
VITE_API_URL=http://localhost:5000/api
VITE_WS_URL=http://localhost:5000
VITE_GOOGLE_CLIENT_ID=your_client_id
```

### Database Setup

```bash
cd packages/backend
npx prisma migrate dev --name init
npx prisma db seed
```

### Running the Project

**Development Mode**
```bash
npm run dev
# Frontend runs on http://localhost:5173
# Backend runs on http://localhost:5000
```

**Production Mode**
```bash
npm run build
npm run start
```

## API Endpoints

### Authentication
- `POST /api/auth/register` - Register user
- `POST /api/auth/login` - Login user
- `POST /api/auth/verify-otp` - Verify OTP
- `POST /api/auth/refresh` - Refresh token
- `POST /api/auth/logout` - Logout user

### Users
- `GET /api/users/profile` - Get user profile
- `PUT /api/users/profile` - Update profile
- `GET /api/users/contacts` - Get user contacts
- `POST /api/users/contacts/sync` - Sync contacts
- `GET /api/users/search` - Search users
- `GET /api/users/:id/status` - Get user status

### Messages
- `GET /api/chats/:chatId/messages` - Get messages
- `POST /api/chats/:chatId/messages` - Send message
- `PUT /api/messages/:messageId` - Edit message
- `DELETE /api/messages/:messageId` - Delete message
- `POST /api/messages/:messageId/reactions` - Add reaction

### Chats
- `GET /api/chats` - Get all chats
- `POST /api/chats` - Create chat
- `PUT /api/chats/:id` - Update chat
- `DELETE /api/chats/:id` - Delete chat
- `POST /api/chats/:id/mute` - Mute chat
- `POST /api/chats/:id/archive` - Archive chat

### Groups
- `POST /api/groups` - Create group
- `GET /api/groups/:id` - Get group info
- `PUT /api/groups/:id` - Update group
- `POST /api/groups/:id/members` - Add member
- `DELETE /api/groups/:id/members/:userId` - Remove member
- `DELETE /api/groups/:id` - Delete group

### Media
- `POST /api/media/upload` - Upload file
- `GET /api/media/:id` - Get media
- `DELETE /api/media/:id` - Delete media

## WebSocket Events

### Client to Server
- `connect` - Establish connection
- `disconnect` - Close connection
- `message:send` - Send message
- `message:typing` - Show typing indicator
- `user:online` - Set user online
- `user:offline` - Set user offline
- `chat:read` - Mark messages as read
- `call:initiate` - Start call
- `call:answer` - Answer call
- `call:reject` - Reject call
- `call:end` - End call

### Server to Client
- `message:receive` - Receive message
- `message:delivered` - Message delivered
- `message:read` - Message read
- `user:typing` - User typing
- `user:online` - User came online
- `user:offline` - User went offline
- `call:incoming` - Incoming call
- `call:connected` - Call connected
- `call:ended` - Call ended

## Key Implementation Details

### 1. Authentication System
- Phone number/Email based registration
- OTP verification via SMS/Email
- JWT token-based sessions
- OAuth integration (Google, Apple)
- Refresh token rotation

### 2. Real-time Messaging
- Socket.io for bi-directional communication
- Message queuing with Redis
- Database persistence
- Delivery & read status tracking
- Message encryption at rest

### 3. Contact Sync
- Phone contact permission handling
- Batch contact upload
- Deduplication and filtering
- Contact change detection
- Privacy-preserving sync

### 4. Media Handling
- Image compression and resizing
- Video thumbnail generation
- Document scanning
- AWS S3 integration
- CDN caching

### 5. Security
- E2E encryption (TweetNaCl.js)
- Rate limiting
- CORS configuration
- SQL injection prevention
- XSS protection
- CSRF tokens

## Deployment

### Frontend (Vercel)
```bash
npm run build:web
vercel deploy
```

### Backend (Railway/Heroku)
```bash
git push heroku main
```

### Docker Deployment
```bash
docker-compose up -d
```

## Testing

```bash
# Unit tests
npm run test

# Integration tests
npm run test:integration

# E2E tests
npm run test:e2e

# Coverage
npm run test:coverage
```

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'Add amazing feature'`)
4. Push to branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

MIT License - see LICENSE file for details

## Support

For support, email support@whatsappclone.dev or create an issue on GitHub.

## Roadmap

- [ ] Video calling with WebRTC
- [ ] Voice calling with adaptive bitrate
- [ ] Status updates (stories)
- [ ] Channel support
- [ ] AI-powered chatbot integration
- [ ] Message scheduling
- [ ] Advanced search with filters
- [ ] Custom themes and dark mode
- [ ] Multi-device sync
- [ ] End-to-end backup
