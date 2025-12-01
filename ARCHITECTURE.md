# WhatsApp Clone Pro - Architecture & Implementation

## System Architecture Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                         CLIENT LAYER                             │
├──────────────────┬───────────────────────┬──────────────────────┤
│   React Web      │   React Native Mobile │   Electron Desktop   │
│   (Vite + TS)    │   (TypeScript)        │   (WhatsApp Web)     │
└──────────────────┴───────────────────────┴──────────────────────┘
         │                    │                      │
         └────────────────────┼──────────────────────┘
                              │
         ┌────────────────────▼───────────────────┐
         │     WebSocket/Socket.io Layer         │
         │  Real-time bidirectional communication│
         └────────────────────┬───────────────────┘
                              │
         ┌────────────────────▼───────────────────────┐
         │           API Gateway / Router             │
         │      (Express.js + TypeScript)            │
         │  - Authentication Middleware               │
         │  - Rate Limiting                           │
         │  - Request Validation                      │
         └────────────────────┬──────────────────────┘
                              │
         ┌────────────────────▼──────────────────────┐
         │        Business Logic Layer               │
         │  ┌──────────────────────────────────┐    │
         │  │ Services & Controllers           │    │
         │  │ - Auth Service                   │    │
         │  │ - Message Service                │    │
         │  │ - User Service                   │    │
         │  │ - Chat Service                   │    │
         │  │ - Contact Service                │    │
         │  │ - Media Service                  │    │
         │  └──────────────────────────────────┘    │
         └────────────────────┬──────────────────────┘
                              │
         ┌────────────────────▼──────────────────────┐
         │      Persistence Layer                    │
         │  ┌──────────────┬──────────────┐         │
         │  │ PostgreSQL   │    Redis     │         │
         │  │  (Primary DB)│ (Cache/Queue)│        │
         │  └──────────────┴──────────────┘         │
         └───────────────────────────────────────────┘
```

## Core Components

### 1. Backend API Server (Express.js + TypeScript)

#### Database Models
- User (phone_number, email, profile_pic, status)
- Message (content, chat_id, sender_id, recipient_id, status, encrypted)
- Chat (type, participants, last_message, created_at)
- Group (name, description, admin, members, pic)
- Contact (user_id, phone_number, name, saved_name)

#### API Endpoints Structure
```
/api/v1/
├── auth/
│   ├── POST /register
│   ├── POST /login
│   ├── POST /verify-otp
│   ├── POST /refresh-token
│   └── POST /logout
├── users/
│   ├── GET /profile
│   ├── PUT /profile
│   ├── GET /search
│   ├── POST /contacts/sync
│   └── GET /:id/status
├── chats/
│   ├── GET /
│   ├── POST /
│   ├── GET /:id
│   ├── PUT /:id
│   ├── DELETE /:id
│   ├── POST /:id/messages
│   ├── GET /:id/messages
│   └── DELETE /:id/messages/:messageId
├── groups/
│   ├── POST /
│   ├── GET /:id
│   ├── PUT /:id
│   ├── DELETE /:id
│   ├── POST /:id/members
│   └── DELETE /:id/members/:userId
└── media/
    ├── POST /upload
    ├── GET /:id
    └── DELETE /:id
```

### 2. WebSocket Events

#### Client → Server
- `message:send` - Send new message
- `message:typing` - User is typing
- `message:read` - Mark messages as read
- `user:online` - User came online
- `user:offline` - User went offline
- `call:initiate` - Start voice/video call
- `call:answer` - Answer incoming call
- `call:reject` - Reject call
- `call:end` - End call

#### Server → Client
- `message:receive` - New message received
- `message:delivered` - Message delivered
- `message:read` - Message read by recipient
- `user:typing` - Someone is typing
- `user:online` - User came online
- `user:offline` - User went offline
- `call:incoming` - Incoming call notification
- `call:connected` - Call connected
- `call:ended` - Call ended

### 3. Frontend Components (React)

```
src/
├── pages/
│   ├── Auth/
│   │   ├── Login.tsx
│   │   ├── Register.tsx
│   │   └── VerifyOTP.tsx
│   ├── ChatPage.tsx
│   ├── ContactsPage.tsx
│   ├── SettingsPage.tsx
│   └── ProfilePage.tsx
├── components/
│   ├── ChatWindow.tsx
│   ├── ChatList.tsx
│   ├── MessageItem.tsx
│   ├── ContactList.tsx
│   ├── UserProfile.tsx
│   └── InputBox.tsx
├── hooks/
│   ├── useChat.ts
│   ├── useSocket.ts
│   ├── useAuth.ts
│   ├── useUsers.ts
│   └── useMessage.ts
├── services/
│   ├── api.ts
│   ├── socket.ts
│   ├── auth.service.ts
│   ├── message.service.ts
│   └── encryption.ts
├── stores/
│   ├── authStore.ts
│   ├── chatStore.ts
│   └── messageStore.ts
└── utils/
    ├── constants.ts
    ├── validators.ts
    └── helpers.ts
```

## Key Features Implementation

### 1. Authentication Flow
```
1. User enters phone number
2. Backend sends OTP via SMS/Email
3. User verifies OTP
4. Backend creates session & generates JWT
5. Frontend stores JWT in localStorage
6. Subsequent requests include JWT in Authorization header
7. Backend validates JWT on every request
8. Token refresh mechanism for expired tokens
```

### 2. Real-time Messaging
```
1. Client sends message via WebSocket
2. Backend validates and encrypts message
3. Message saved to PostgreSQL
4. Server broadcasts to recipient via Socket.io
5. Recipient receives message in real-time
6. Delivery status updated
7. When recipient reads message, read receipt sent
```

### 3. Contact Sync
```
1. User permits contact access
2. App reads device contacts
3. Hashes phone numbers for privacy
4. Sends to backend in batches
5. Backend matches with existing users
6. Returns list of registered contacts
7. App displays suggestedcontacts
```

### 4. Encryption Strategy
```
- Use TweetNaCl.js for client-side encryption
- Public key exchange during chat initialization
- Messages encrypted before sending to server
- Server stores encrypted messages
- Decryption happens on client
- Key derivation using PBKDF2
```

### 5. File Upload & Media
```
1. User selects file
2. Frontend compresses if needed
3. Uploads to AWS S3 with signed URL
4. Server receives S3 URL
5. Creates media record in database
6. Sends media URL via message
7. Recipients download directly from CDN
```

## Database Schema

### Users Table
```sql
CREATE TABLE users (
  id UUID PRIMARY KEY,
  phone_number VARCHAR(20) UNIQUE,
  email VARCHAR(255) UNIQUE,
  password_hash VARCHAR(255),
  first_name VARCHAR(100),
  last_name VARCHAR(100),
  profile_pic_url TEXT,
  status_text VARCHAR(255),
  is_online BOOLEAN,
  last_seen_at TIMESTAMP,
  public_key TEXT,
  created_at TIMESTAMP,
  updated_at TIMESTAMP
);
```

### Messages Table
```sql
CREATE TABLE messages (
  id UUID PRIMARY KEY,
  chat_id UUID REFERENCES chats(id),
  sender_id UUID REFERENCES users(id),
  content TEXT ENCRYPTED,
  media_url TEXT,
  message_status ENUM('sent', 'delivered', 'read'),
  created_at TIMESTAMP,
  updated_at TIMESTAMP,
  deleted_at TIMESTAMP
);
```

### Chats Table
```sql
CREATE TABLE chats (
  id UUID PRIMARY KEY,
  type ENUM('direct', 'group'),
  group_id UUID,
  created_at TIMESTAMP,
  updated_at TIMESTAMP
);
```

### Chat Participants Table
```sql
CREATE TABLE chat_participants (
  id UUID PRIMARY KEY,
  chat_id UUID REFERENCES chats(id),
  user_id UUID REFERENCES users(id),
  joined_at TIMESTAMP,
  muted_until TIMESTAMP,
  archived BOOLEAN
);
```

## Security Considerations

1. **Authentication**: JWT with refresh tokens
2. **Encryption**: End-to-end encryption using TweetNaCl.js
3. **Rate Limiting**: 100 requests per 15 minutes per IP
4. **CORS**: Configured for whitelisted origins only
5. **HTTPS/WSS**: All communications encrypted in transit
6. **SQL Injection Prevention**: Parameterized queries with Prisma
7. **XSS Protection**: React's built-in XSS prevention
8. **CSRF Tokens**: Implemented for state-changing operations
9. **Password Hashing**: bcrypt with 12 salt rounds
10. **API Versioning**: Support for multiple API versions

## Deployment

### Frontend Deployment (Vercel)
```bash
npm run build:web
vercel deploy --prod
```

### Backend Deployment (Railway/Heroku)
```bash
git push railway main
```

### Docker Deployment
```bash
docker-compose up -d
```

## Performance Optimization

1. **Caching**: Redis for session and message cache
2. **Database Indexing**: On frequently queried columns
3. **Pagination**: For message and chat lists
4. **Lazy Loading**: Load messages on scroll
5. **Image Compression**: Client-side before upload
6. **Code Splitting**: React lazy loading for routes
7. **CDN**: CloudFront for static assets
8. **Database Connection Pooling**: PgBouncer

## Monitoring & Logging

1. **Error Tracking**: Sentry integration
2. **Performance Monitoring**: New Relic
3. **Logs**: Winston for structured logging
4. **Metrics**: Prometheus metrics
5. **Uptime Monitoring**: Uptimerobot

## Future Enhancements

1. Video/Voice calling with WebRTC
2. Status updates (Stories)
3. Channel support
4. Message scheduling
5. Advanced search with filters
6. Custom themes and dark mode
7. Bot integration
8. AI-powered suggestions
9. End-to-end backup
10. Multi-device sync
