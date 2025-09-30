## 2️⃣ BACKEND README (save as `pong-app/backend/README.md`)

```markdown
# 🎮 Backend Server - Game API & Socket.io Services

Node.js backend server providing REST API and real-time game services for the Pong Game Platform.

## 🏗️ Architecture

### Core Technologies

- **Runtime**: Node.js 22
- **Framework**: Fastify (REST API)
- **Real-time**: Socket.io (WebSocket communication)
- **Database**: SQLite with Prisma ORM
- **Authentication**: JWT + HTTP-only cookies
- **Email**: Nodemailer
- **OAuth**: Google OAuth 2.0
- **2FA**: Speakeasy

### Project Structure

```
backend/
├── src/
│   ├── routes/              # API endpoint handlers
│   │   ├── authRoutes.ts    # Authentication endpoints
│   │   ├── userRoutes.ts    # User profile management
│   │   ├── friendRoutes.ts  # Friend system
│   │   ├── gameRoutes.ts    # Game statistics
│   │   └── index.ts         # Route registration
│   │
│   ├── service/
│   │   └── emailService.ts  # Email sending logic
│   │
│   ├── types/
│   │   └── lobby.ts         # TypeScript interfaces
│   │
│   ├── index.ts             # Server entry point
│   ├── env.ts               # Environment validation
│   ├── gameData.ts          # Shared game state
│   ├── PingPongGame.ts      # Pong game engine
│   ├── KeyClashGame.ts      # KeyClash game engine
│   ├── PongServer.ts        # Pong Socket.io server
│   ├── quickmatch.ts        # Quick match lobby
│   └── tournament.ts        # Tournament lobby
│
├── prisma/
│   ├── schema.prisma        # Database schema
│   └── migrations/          # Database migrations
│
├── Dockerfile
├── package.json
└── tsconfig.json
```

## 🚀 Getting Started

### Installation

```bash
# Install dependencies
npm install

# Generate Prisma client
npx prisma generate

# Run database migrations
npx prisma migrate deploy

# Start development server
npm run dev
```

### Environment Variables

Create `.env` file:

```env
# Server Configuration
PORT=3000
DATABASE_URL="file:./dev.db"

# Security
JWT_SECRET=your-super-secret-jwt-key-change-this
COOKIE_SECRET=your-cookie-secret-key

# URLs
FRONTEND_URL=https://localhost:5173
FRONTEND_REMOTE_URL=https://your-ngrok-url.ngrok-free.app
CP_URL=https://localhost:5173

# Email Service (Gmail)
EMAIL_HOST=smtp.gmail.com
EMAIL_PORT=587
EMAIL_SECURE=false
EMAIL_SERVICE=gmail
EMAIL_USER=your-email@gmail.com
EMAIL_PASSWORD=your-app-password
EMAIL_FROM=your-email@gmail.com

# Branding
TEAM_NAME="Hivers5 Asteroids"

# Google OAuth
GOOGLE_CLIENT_ID=your-google-client-id.apps.googleusercontent.com
GOOGLE_CLIENT_SECRET=your-google-client-secret
GOOGLE_REDIRECT_URI=https://localhost:3000/auth/google/callback
```

### Development Commands

```bash
npm run dev          # Development with hot reload
npm run build        # Build TypeScript
npm start            # Start production server
npx prisma studio    # Open database GUI
```

## 📡 API Endpoints

### Base URL
```
https://localhost:3000
```

### Authentication

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| POST | `/auth/register` | Register new user | ❌ |
| POST | `/auth/login` | Login (sends 2FA code) | ❌ |
| POST | `/auth/verify-email` | Verify email with code | ❌ |
| POST | `/auth/verify-2fa` | Verify 2FA and login | ❌ |
| POST | `/auth/resend-verification-code` | Resend verification code | ❌ |
| POST | `/auth/reset-password` | Request password reset | ❌ |
| POST | `/auth/change-password` | Change password | ❌ |
| POST | `/auth/signin-with-google` | Google OAuth login | ❌ |
| POST | `/auth/logout` | Logout user | ✅ |
| GET | `/profile` | Get current user | ✅ |

### User Management

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/user/profile` | Get profile details | ✅ |
| PUT | `/user/profile` | Update profile | ✅ |
| GET | `/user/stats` | Get user statistics | ✅ |
| GET | `/user/avatars` | Get available avatars | ✅ |
| POST | `/user/avatar` | Upload avatar | ✅ |

### Friend System

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/friend/friends` | Get friends list | ✅ |
| GET | `/friend/online` | Get online friends | ✅ |
| GET | `/friend/users/search` | Search users | ✅ |
| GET | `/friend/requests` | Get friend requests | ✅ |
| POST | `/friend/request/:userId` | Send friend request | ✅ |
| POST | `/friend/accept/:requestId` | Accept request | ✅ |
| DELETE | `/friend/decline/:requestId` | Decline request | ✅ |
| DELETE | `/friend/remove/:friendshipId` | Remove friend | ✅ |

### Game Statistics

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/game/history` | Get match history | ✅ |
| GET | `/game/stats` | Get game statistics | ✅ |
| GET | `/game/leaderboard` | Get global leaderboard | ✅ |
| GET | `/game/recent` | Get recent matches | ✅ |
| POST | `/game` | Save game result | ✅ |

## 📝 Request/Response Examples

### Authentication Endpoints

#### Register User

**Request:**
```http
POST /auth/register
Content-Type: application/json

{
  "username": "john_doe",
  "email": "john@example.com",
  "password": "securePass123"
}
```

**Response (201 Created):**
```json
{
  "success": true,
  "message": "Verification email sent",
  "userId": 1,
  "requiresVerification": true
}
```

**Error Response (400 Bad Request):**
```json
{
  "error": "USERNAME_EXISTS",
  "message": "Username already exists"
}
```

---

#### Verify Email

**Request:**
```http
POST /auth/verify-email
Content-Type: application/json

{
  "userId": 1,
  "code": "123456"
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Email verified successfully. Please login to continue.",
  "user": {
    "id": 1,
    "email": "john@example.com",
    "username": "john_doe",
    "isVerified": true
  },
  "totp_url": "otpauth://totp/..."
}
```

**Error Response (400 Bad Request):**
```json
{
  "error": "INVALID_CODE",
  "message": "Invalid or expired verification code"
}
```

---

#### Login

**Request:**
```http
POST /auth/login
Content-Type: application/json

{
  "username": "john_doe",
  "password": "securePass123"
}
```

**Response (200 OK):**
```json
{
  "requires2FA": true,
  "userId": 1,
  "message": "2FA code sent to your email. Please verify to continue.",
  "url": "otpauth://totp/..." 
}
```

**Error Response (401 Unauthorized):**
```json
{
  "error": "INVALID_CREDENTIALS",
  "message": "Invalid username or password"
}
```

---

#### Verify 2FA

**Request:**
```http
POST /auth/verify-2fa
Content-Type: application/json

{
  "userId": 1,
  "code": "123456"
}
```

**Response (200 OK + HTTP-only Cookie):**
```json
{
  "user": {
    "id": 1,
    "email": "john@example.com",
    "username": "john_doe",
    "isVerified": true
  }
}
```

---

#### Google OAuth Sign-In

**Request:**
```http
POST /auth/signin-with-google
Content-Type: application/json

{
  "credential": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

**Response (200 OK + HTTP-only Cookie):**
```json
{
  "user": {
    "id": 2,
    "email": "user@gmail.com",
    "username": "googleUser123",
    "isVerified": true
  }
}
```

---

#### Reset Password Request

**Request:**
```http
POST /auth/reset-password
Content-Type: application/json

{
  "email": "john@example.com"
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Password reset instructions have been sent to your email."
}
```

---

#### Change Password

**Request:**
```http
POST /auth/change-password
Content-Type: application/json

{
  "token": "abc123def456...",
  "password": "newSecurePass456"
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Password successfully updated! You can now login with your new password."
}
```

---

#### Resend Verification Code

**Request:**
```http
POST /auth/resend-verification-code
Content-Type: application/json

{
  "userId": 1,
  "context": "email-verification"
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "message": "New verification code sent to your email"
}
```

---

#### Logout

**Request:**
```http
POST /auth/logout
Cookie: authToken=eyJhbGciOiJIUzI1NiIs...
```

**Response (200 OK + Clears Cookie):**
```json
{
  "success": true,
  "message": "Logged out successfully"
}
```

---

#### Get Profile

**Request:**
```http
GET /profile
Cookie: authToken=eyJhbGciOiJIUzI1NiIs...
```

**Response (200 OK):**
```json
{
  "id": 1,
  "username": "john_doe",
  "email": "john@example.com",
  "isVerified": true,
  "profilePic": "data:image/png;base64,...",
  "wins": 15,
  "losses": 8,
  "favAvatar": "AstroAce",
  "online_status": "online",
  "lastLogin": "2025-01-15T10:30:00.000Z"
}
```

---

### User Management Endpoints

#### Get User Profile

**Request:**
```http
GET /user/profile
Cookie: authToken=eyJhbGciOiJIUzI1NiIs...
```

**Response (200 OK):**
```json
{
  "id": 1,
  "username": "john_doe",
  "email": "john@example.com",
  "firstName": "John",
  "lastName": "Doe",
  "dateOfBirth": "1995-05-15",
  "gender": "male",
  "favAvatar": "AstroAce",
  "wins": 15,
  "losses": 8,
  "profilePic": "data:image/png;base64,...",
  "online_status": "online",
  "lastLogin": "2025-01-15T10:30:00.000Z",
  "isVerified": true,
  "createdAt": "2025-01-01T00:00:00.000Z"
}
```

---

#### Update Profile

**Request:**
```http
PUT /user/profile
Content-Type: application/json
Cookie: authToken=eyJhbGciOiJIUzI1NiIs...

{
  "firstName": "John",
  "lastName": "Doe",
  "dateOfBirth": "1995-05-15",
  "gender": "male",
  "favAvatar": "RoboRacer",
  "profilePic": "data:image/png;base64,iVBORw0KGgo..."
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Profile updated successfully",
  "user": {
    "id": 1,
    "username": "john_doe",
    "email": "john@example.com",
    "firstName": "John",
    "lastName": "Doe",
    "dateOfBirth": "1995-05-15",
    "gender": "male",
    "favAvatar": "RoboRacer",
    "wins": 15,
    "losses": 8,
    "profilePic": "data:image/png;base64,...",
    "isVerified": true
  }
}
```

---

#### Get Available Avatars

**Request:**
```http
GET /user/avatars
Cookie: authToken=eyJhbGciOiJIUzI1NiIs...
```

**Response (200 OK):**
```json
[
  {
    "id": "AstroAce",
    "name": "Astro Ace",
    "imageUrl": "/avatars/astro_ace.png",
    "description": "Space explorer, ready for any cosmic challenge."
  },
  {
    "id": "PixelPirate",
    "name": "Pixel Pirate",
    "imageUrl": "/avatars/pixel_pirate.png",
    "description": "A swashbuckler who sails the digital seas."
  },
  {
    "id": "RoboRacer",
    "name": "Robo Racer",
    "imageUrl": "/avatars/robo_racer.png",
    "description": "Futuristic racer, built for speed and style."
  }
]
```

---

### Friend System Endpoints

#### Get Friends List

**Request:**
```http
GET /friend/friends
Cookie: authToken=eyJhbGciOiJIUzI1NiIs...
```

**Response (200 OK):**
```json
[
  {
    "friendshipId": "1-2",
    "status": "Friend",
    "friend": {
      "id": 2,
      "name": "jane_smith",
      "email": "jane@example.com",
      "online_status": "online",
      "profilePic": null,
      "createdAt": "2025-01-02T00:00:00.000Z"
    }
  },
  {
    "friendshipId": "1-3",
    "status": "Friend",
    "friend": {
      "id": 3,
      "name": "mike_wilson",
      "email": "mike@example.com",
      "online_status": "offline",
      "profilePic": "data:image/png;base64,...",
      "createdAt": "2025-01-03T00:00:00.000Z"
    }
  }
]
```

---

#### Get Online Friends

**Request:**
```http
GET /friend/online
Cookie: authToken=eyJhbGciOiJIUzI1NiIs...
```

**Response (200 OK):**
```json
[
  {
    "id": 2,
    "name": "jane_smith",
    "status": "online"
  },
  {
    "id": 5,
    "name": "alex_johnson",
    "status": "online"
  }
]
```

---

#### Search Users

**Request:**
```http
GET /friend/users/search?q=john&online_only=false
Cookie: authToken=eyJhbGciOiJIUzI1NiIs...
```

**Response (200 OK):**
```json
[
  {
    "id": 4,
    "name": "johnny_bravo",
    "email": "johnny@example.com",
    "online_status": "online",
    "createdAt": "2025-01-04T00:00:00.000Z",
    "profilePic": null,
    "friendshipStatus": "NotFriend"
  },
  {
    "id": 6,
    "name": "john_williams",
    "email": "jwilliams@example.com",
    "online_status": "offline",
    "createdAt": "2025-01-05T00:00:00.000Z",
    "profilePic": "data:image/png;base64,...",
    "friendshipStatus": "NotFriend"
  }
]
```

---

#### Get Friend Requests

**Request:**
```http
GET /friend/requests
Cookie: authToken=eyJhbGciOiJIUzI1NiIs...
```

**Response (200 OK):**
```json
[
  {
    "requestId": "7-1",
    "type": "received",
    "user": {
      "id": 7,
      "name": "sarah_connor",
      "email": "sarah@example.com",
      "profilePic": null,
      "createdAt": "2025-01-06T00:00:00.000Z"
    }
  },
  {
    "requestId": "1-8",
    "type": "sent",
    "user": {
      "id": 8,
      "name": "kyle_reese",
      "email": "kyle@example.com",
      "profilePic": "data:image/png;base64,...",
      "createdAt": "2025-01-07T00:00:00.000Z"
    }
  }
]
```

---

#### Send Friend Request

**Request:**
```http
POST /friend/request/5
Cookie: authToken=eyJhbGciOiJIUzI1NiIs...
```

**Response (200 OK):**
```json
{
  "message": "Friend request sent successfully"
}
```

---

#### Accept Friend Request

**Request:**
```http
POST /friend/accept/7-1
Cookie: authToken=eyJhbGciOiJIUzI1NiIs...
```

**Response (200 OK):**
```json
{
  "message": "Friend request accepted"
}
```

---

#### Decline Friend Request

**Request:**
```http
DELETE /friend/decline/9-1
Cookie: authToken=eyJhbGciOiJIUzI1NiIs...
```

**Response (200 OK):**
```json
{
  "message": "Friend request declined"
}
```

---

#### Remove Friend

**Request:**
```http
DELETE /friend/remove/1-2
Cookie: authToken=eyJhbGciOiJIUzI1NiIs...
```

**Response (200 OK):**
```json
{
  "message": "Friendship removed"
}
```

---

### Game Statistics Endpoints

#### Get Match History

**Request:**
```http
GET /game/history?limit=10&offset=0
Cookie: authToken=eyJhbGciOiJIUzI1NiIs...
```

**Response (200 OK):**
```json
[
  {
    "id": "42",
    "date": "2025-01-15T14:30:00.000Z",
    "gameType": "pingpong",
    "opponent": "jane_smith",
    "result": "win",
    "score": "3-1",
    "duration": "1:45",
    "mode": "remote",
    "rounds": []
  },
  {
    "id": "41",
    "date": "2025-01-15T13:15:00.000Z",
    "gameType": "keyclash",
    "opponent": "mike_wilson",
    "result": "loss",
    "score": "45-52",
    "duration": "0:20",
    "mode": "local",
    "rounds": []
  }
]
```

---

#### Get Game Statistics

**Request:**
```http
GET /game/stats
Cookie: authToken=eyJhbGciOiJIUzI1NiIs...
```

**Response (200 OK):**
```json
{
  "wins": 15,
  "losses": 8,
  "totalMatches": 23,
  "winRate": 65.2,
  "currentWinStreak": 3,
  "longestWinStreak": 7,
  "monthlyWins": 5,
  "source": "game_api"
}
```

---

#### Get Leaderboard

**Request:**
```http
GET /game/leaderboard?limit=10
Cookie: authToken=eyJhbGciOiJIUzI1NiIs...
```

**Response (200 OK):**
```json
[
  {
    "rank": 1,
    "username": "pro_gamer_123",
    "wins": 150,
    "losses": 30,
    "totalGames": 180,
    "winRate": 83.3,
    "points": 420,
    "online_status": "online",
    "profilePic": null,
    "isCurrentUser": false
  },
  {
    "rank": 2,
    "username": "john_doe",
    "wins": 15,
    "losses": 8,
    "totalGames": 23,
    "winRate": 65.2,
    "points": 37,
    "online_status": "online",
    "profilePic": "data:image/png;base64,...",
    "isCurrentUser": true
  }
]
```

---

#### Get Recent Matches

**Request:**
```http
GET /game/recent?limit=5
Cookie: authToken=eyJhbGciOiJIUzI1NiIs...
```

**Response (200 OK):**
```json
[
  {
    "id": "42",
    "opponent": "jane_smith",
    "matchType": "pingpong",
    "mode": "remote",
    "result": "win",
    "score": "3-1",
    "date": "2025-01-15T14:30:00.000Z"
  },
  {
    "id": "41",
    "opponent": "mike_wilson",
    "matchType": "keyclash",
    "mode": "local",
    "result": "loss",
    "score": "45-52",
    "date": "2025-01-15T13:15:00.000Z"
  }
]
```

---

## 🔌 Socket.io Events

### Namespaces

- `/quickmatch` - Quick match lobby (1v1)
- `/tournament` - Tournament lobby (4-player)
- `/pong` - Pong game server
- `/keyclash` - Key Clash game server

### Quick Match Lobby (`/quickmatch`)

**Client → Server:**
```typescript
// Join lobby
socket.emit("name", name: string | null, playerId: number | null, callback)

// Create game
socket.emit("create_game", game: "pong" | "keyclash", mode: "local" | "remote")

// Join game
socket.emit("join_game", gameId: string, game: string, mode: string, callback)
```

**Server → Client:**
```typescript
// Lobby update
socket.on("lobby_update", (data: LobbyState) => {
  // data.players: Player[]
  // data.pongGames: GameRoom[]
  // data.keyClashGames: GameRoom[]
})

// Game created
socket.on("created_game", (gameId, game, mode) => {})

// Game joined
socket.on("joined_game", (gameId, game, mode) => {})
```

### Pong Game Server (`/pong`)

**Client → Server:**
```typescript
// Join game room
socket.emit("join_game_room", roomId, playerId, callback)

// Send names
socket.emit("names", { player1, player2, player3, player4 })

// Set ready
socket.emit("setReady")

// Move paddle
socket.emit("move", side: "left" | "right", positionZ: number)

// Pause
socket.emit("pause")

// Restart
socket.emit("restart")
```

**Server → Client:**
```typescript
// Request names
socket.on("get_names", () => {})

// Player side
socket.on("playerSide", (side: "left" | "right" | null) => {})

// State update (60fps)
socket.on("stateUpdate", (state: GameState) => {
  // state.ball, state.leftPaddle, state.rightPaddle
  // state.scoreDisplay, state.timerDisplay
  // state.status: "waiting" | "in-progress" | "finished" | "paused"
})

// Waiting
socket.on("waiting", (state) => {})

// Disconnection
socket.on("disconnection", () => {})
```

### Key Clash Game Server (`/keyclash`)

**Client → Server:**
```typescript
// Join room
socket.emit("join_game_room", roomId, mode, type, playerId, callback)

// Send names
socket.emit("names", { player1, player2, player3, player4 })

// Key press
socket.emit("keypress", { key: string })

// Set ready
socket.emit("setReady")
```

**Server → Client:**
```typescript
// Game start
socket.on("gameStart", (state) => {})

// Game state
socket.on("gameState", (state) => {
  // state.player1, state.player2
  // state.prompts, state.timeLeft
})

// Correct hit
socket.on("correctHit", ({ player: 1 | 2 }) => {})

// Game over
socket.on("gameOver", (state) => {})

// Waiting
socket.on("waiting", (state) => {})
```

## 🗄️ Database Schema

### User Model
```prisma
model User {
  id                   Int         @id @default(autoincrement())
  username             String      @unique
  password             String?
  email                String      @unique
  isVerified           Boolean     @default(false)
  twoFactorSecret      String?
  twoFactorURL         String?
  twoFactorRegistered  Boolean     @default(false)
  googleId             String?
  firstName            String?
  lastName             String?
  dateOfBirth          String?
  gender               Gender      @default(other)
  favAvatar            FavAvatar   @default(None)
  wins                 Int         @default(0)
  losses               Int         @default(0)
  profilePic           String?
  online_status        OnlineStatus @default(offline)
  lastLogin            DateTime?
  auth_provider        AuthProvider @default(email)
  createdAt            DateTime    @default(now())
}
```

### Friendship Model
```prisma
model Friendship {
  sender_id          Int
  receiver_id        Int
  sender_username    String
  receiver_username  String
  status             FriendshipStatus @default(NotFriend)
  
  @@id([sender_id, receiver_id])
}
```

### Game Model
```prisma
model Game {
  id_game     Int      @id @default(autoincrement())
  id_player1  Int?
  id_player2  Int?
  date        DateTime @default(now())
  rounds_json String
  game_name   GameName
}
```

## 🔐 Security Features

### Authentication Flow

1. Registration → Email verification (6-digit code)
2. Login → 2FA code sent via email
3. Session → JWT in HTTP-only cookie (1 hour expiry)
4. Auto cleanup → Inactive users marked offline after 65 minutes

### Security Measures

- Password hashing (bcrypt, 10 rounds)
- JWT with 1-hour expiry
- HTTP-only cookies (XSS protection)
- CORS protection
- Input validation (validator.js)
- SQL injection prevention (Prisma)
- Rate limiting on verification codes
- Session cleanup on restart

## 🎮 Game Logic

### Pong Game Physics

- Ball speed: 6.0 (X), 3.5 (Z) units/s
- Paddle speed: 12 units/s
- Ball acceleration: 5% per hit
- Table bounds: ±9.6 (X), ±5.6 (Z)
- Win: First to 3 points OR higher score after 2 min
- Update rate: 60fps

### Key Clash Mechanics

- Player 1: WASD keys
- Player 2: Arrow keys
- Duration: 20 seconds
- Correct: +1, Incorrect: -1
- Tie: Player 2 wins

## 🐛 Debugging

### Enable Debug Logging
```typescript
const server = fastify({
  logger: { level: 'debug' }
});
```

### Common Issues

**Database Lock:**
```bash
npx prisma studio  # Close if open
# Restart server
```

**Migration Errors:**
```bash
npx prisma migrate reset
npx prisma migrate deploy
```

## 🚀 Deployment

### Production Build

```bash
npm run build
NODE_ENV=production npm start
```

### Environment Checklist

- ✅ Secure JWT_SECRET
- ✅ Strong COOKIE_SECRET
- ✅ Production database URL
- ✅ Email service configured
- ✅ HTTPS enabled
- ✅ CORS for production domain
- ✅ Google OAuth credentials

---

**For more information, see the [main README](../README.md) and [frontend README](../frontend/README.md)**
```

