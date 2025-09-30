# 🏓 Hivers5 Asteroids - Pong Game Platform

A full-stack, real-time multiplayer gaming platform featuring classic Ping Pong and Key Clash games with tournament modes, friend systems, and comprehensive player statistics.

## 📋 Table of Contents

- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Prerequisites](#-prerequisites)
- [Installation](#-installation)
- [Configuration](#-configuration)
- [Running the Application](#-running-the-application)
- [Project Structure](#-project-structure)
- [Game Modes](#-game-modes)
- [API Documentation](#-api-documentation)
- [Development](#-development)
- [Troubleshooting](#-troubleshooting)

## ✨ Features

### 🎮 Gaming
- **Two Game Types**:
  - **Ping Pong**: 3D table tennis with realistic physics (Three.js)
  - **Key Clash**: Fast-paced typing competition game
- **Multiple Game Modes**:
  - 1v1 Quick Match (Local & Remote)
  - Tournament Mode (4-player bracket)
- **Real-time Multiplayer**: Socket.io powered gameplay

### 👥 Social Features
- Friend system with online status tracking
- Friend requests (send/accept/decline)
- Online friends display
- Player search functionality

### 📊 Statistics & Progress
- Match history with detailed game data
- Win/loss statistics
- Win streak tracking
- Global leaderboard
- Monthly statistics

### 🔐 Authentication & Security
- Email/Password registration
- Google OAuth sign-in
- Two-Factor Authentication (2FA)
- Email verification
- Password reset functionality
- Secure JWT-based sessions

### 🎨 Customization
- Avatar selection system (12 unique avatars)
- Profile customization (name, birthday, gender)
- Profile picture upload

## 🛠 Tech Stack

### Backend
- **Runtime**: Node.js 22
- **Framework**: Fastify
- **Real-time**: Socket.io
- **Database**: SQLite + Prisma ORM
- **Authentication**: JWT, Google OAuth 2.0
- **Email**: Nodemailer
- **Language**: TypeScript

### Frontend
- **Framework**: React 18
- **Build Tool**: Vite
- **Routing**: React Router v6
- **Real-time**: Socket.io-client
- **3D Graphics**: Three.js (r128)
- **Styling**: TailwindCSS
- **Language**: TypeScript

### DevOps
- **Containerization**: Docker & Docker Compose
- **Tunneling**: ngrok (for external access)
- **SSL**: Self-signed certificates for local HTTPS

## 📦 Prerequisites

- **Docker** (v20.10+) & **Docker Compose** (v2.0+)
- **Make** (for Makefile commands)
- **Node.js** 20+ (for local development)
- **Git**

## 🚀 Installation

### 1. Clone the Repository

```bash
git clone <your-repo-url>
cd pong-app
```

### 2. SSL Certificates Setup

The application uses self-signed SSL certificates located in `pong-app/tls/`. These are already included in the repository:
- `cert.pem` - Server certificate
- `key.pem` - Private key
- `ca-cert.pem` - Certificate Authority certificate

**Important**: For production, replace these with proper SSL certificates.

### 3. Environment Configuration

#### Backend Environment (`pong-app/backend/.env`)

```env
# Server Configuration
PORT=3000
NODE_ENV=development

# Database
DATABASE_URL="file:./dev.db"

# JWT & Cookies
JWT_SECRET=your-super-secret-jwt-key-change-this-in-production
COOKIE_SECRET=your-super-secret-cookie-key-change-this

# Frontend URLs
FRONTEND_URL=https://localhost:5173
FRONTEND_REMOTE_URL=https://your-ngrok-url.ngrok.io
CP_URL=https://localhost:5173

# Email Configuration (Gmail example)
EMAIL_SERVICE=gmail
EMAIL_HOST=smtp.gmail.com
EMAIL_PORT=587
EMAIL_SECURE=false
EMAIL_USER=your-email@gmail.com
EMAIL_PASSWORD=your-app-specific-password
EMAIL_FROM=your-email@gmail.com
TEAM_NAME="Hivers5 Asteroids"

# Google OAuth
GOOGLE_CLIENT_ID=your-google-client-id
GOOGLE_CLIENT_SECRET=your-google-client-secret
GOOGLE_REDIRECT_URI=https://localhost:3000/auth/google/callback
```

#### Frontend Environment (`pong-app/frontend/.env`)

```env
# API Configuration
VITE_API_URL=https://localhost:3000

# Google OAuth
VITE_GOOGLE_CLIENT_ID=your-google-client-id
```

#### Docker Compose ngrok Configuration (`pong-app/docker-compose.yml`)

```yaml
ngrok:
  environment:
    - NGROK_AUTHTOKEN=your-ngrok-authtoken  # Get from https://ngrok.com
```

### 4. Google OAuth Setup (Optional)

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project
3. Enable Google+ API
4. Create OAuth 2.0 credentials
5. Add authorized redirect URIs:
   - `https://localhost:3000/auth/google/callback`
   - `https://your-ngrok-url.ngrok.io/auth/google/callback`
6. Copy Client ID and Secret to `.env` files

### 5. Email Setup (Gmail Example)

1. Enable 2-Step Verification in Gmail
2. Generate App Password: Google Account → Security → App Passwords
3. Use the generated password in `EMAIL_PASSWORD`

## 🎬 Running the Application

### Using Make Commands (Recommended)

```bash
# View all available commands
make help

# Full build and start (clean build)
make run

# Start containers (if already built)
make up

# Stop containers
make down

# View logs
make logs

# Full cleanup and rebuild
make fclean
make run

# Check container status
make status
```

### Using Docker Compose Directly

```bash
# Build and start
docker compose -f ./pong-app/docker-compose.yml up -d --build

# Stop
docker compose -f ./pong-app/docker-compose.yml down

# View logs
docker compose -f ./pong-app/docker-compose.yml logs -f
```

### Accessing the Application

- **Frontend**: `https://localhost:5173`
- **Backend API**: `https://localhost:3000`
- **ngrok Dashboard**: `http://localhost:4040`

**Note**: Your browser will warn about the self-signed certificate. Click "Advanced" → "Proceed to localhost" to continue.

### Getting ngrok URL

After starting with `make up`, the ngrok tunnel URL will be displayed in the terminal. You can also access it at:
```bash
curl http://localhost:4040/api/tunnels | grep public_url
```

## 📁 Project Structure

```
pong-app/
├── backend/                 # Backend Node.js application
│   ├── src/
│   │   ├── index.ts        # Main server entry point
│   │   ├── env.ts          # Environment validation
│   │   ├── routes/         # API route handlers
│   │   │   ├── authRoutes.ts      # Authentication endpoints
│   │   │   ├── userRoutes.ts      # User profile endpoints
│   │   │   ├── friendRoutes.ts    # Friend system endpoints
│   │   │   └── gameRoutes.ts      # Game statistics endpoints
│   │   ├── service/        # Business logic
│   │   │   └── emailService.ts    # Email sending
│   │   ├── PingPongGame.ts # Pong game logic
│   │   ├── KeyClashGame.ts # Key Clash game logic
│   │   ├── PongServer.ts   # Pong Socket.io handler
│   │   ├── quickmatch.ts   # 1v1 lobby handler
│   │   ├── tournament.ts   # Tournament lobby handler
│   │   └── gameData.ts     # Game state management
│   ├── prisma/
│   │   └── schema.prisma   # Database schema
│   ├── Dockerfile
│   └── package.json
│
├── frontend/               # Frontend React application
│   ├── src/
│   │   ├── main.tsx       # App entry point
│   │   ├── app.tsx        # Root component with routing
│   │   ├── pages/         # Page components
│   │   │   ├── unauthorised/   # Public pages (login, register, etc.)
│   │   │   ├── authorised/     # Protected pages (lobby, etc.)
│   │   │   ├── game/           # Game page
│   │   │   └── general/        # Shared pages (avatar selection)
│   │   ├── components/    # Reusable components
│   │   │   ├── general/        # Shared components
│   │   │   ├── lobby/          # Lobby tab components
│   │   │   ├── quickmatch-lobby/
│   │   │   └── tournament-lobby/
│   │   ├── contexts/      # React contexts
│   │   │   └── AuthContext.tsx
│   │   ├── utils/         # Utility functions
│   │   │   ├── api.ts          # Axios instance
│   │   │   ├── PingPongClient.ts  # Pong game client
│   │   │   └── keyClashClient.ts  # Key Clash game client
│   │   └── styles.css     # Global styles
│   ├── Dockerfile
│   └── package.json
│
├── ngrok/                 # ngrok tunnel configuration
│   └── Dockerfile
│
├── tls/                   # SSL certificates
│   ├── cert.pem
│   ├── key.pem
│   └── ca-cert.pem
│
├── docker-compose.yml     # Docker orchestration
└── Makefile              # Build automation
```

## 🎯 Game Modes

### Ping Pong (3D Table Tennis)

**Controls**:
- **Local Mode**:
  - Player 1 (Left): `W` (up) / `S` (down)
  - Player 2 (Right): `Arrow Up` / `Arrow Down`
  - Pause: `ESC`
- **Remote Mode**:
  - Both players: `W/S` or `Arrow Up/Down`
  - Pause: `ESC` (local mode only)

**Scoring**:
- First to 3 points wins
- Maximum game time: 2 minutes
- Ball speeds up with each paddle hit

### Key Clash (Typing Game)

**Controls**:
- **Local Mode**:
  - Player 1 (Left): `W`, `A`, `S`, `D` keys
  - Player 2 (Right): Arrow keys
- **Remote Mode**:
  - Both players: `W/A/S/D` or Arrow keys
- Start/Ready: `SPACE`

**Scoring**:
- Match displayed keys as fast as possible
- Correct key: +1 point
- Wrong key: -1 point
- Game duration: 20 seconds

### Tournament Mode

- 4-player single-elimination bracket
- 3 rounds total:
  - Round 1: Player 1 vs Player 2
  - Round 2: Player 3 vs Player 4
  - Round 3 (Final): Winner 1 vs Winner 2
- Available for both Ping Pong and Key Clash

## 📡 API Documentation

### Authentication Endpoints

```
POST   /api/auth/register              # Register new user
POST   /api/auth/login                 # Login user (initiates 2FA)
POST   /api/auth/verify-email          # Verify email with code
POST   /api/auth/verify-2fa            # Verify 2FA code and login
POST   /api/auth/resend-verification-code  # Resend verification code
POST   /api/auth/reset-password        # Request password reset
POST   /api/auth/change-password       # Change password with token
POST   /api/auth/signin-with-google    # Google OAuth sign-in
POST   /api/auth/logout                # Logout user
GET    /api/profile                    # Get current user profile
```

### User Endpoints

```
GET    /api/user/profile               # Get user profile details
PUT    /api/user/profile               # Update user profile
GET    /api/user/stats                 # Get user statistics
GET    /api/user/avatars               # Get available avatars
POST   /api/user/avatar                # Upload profile picture
```

### Friend Endpoints

```
GET    /api/friend/friends             # Get user's friends list
GET    /api/friend/online              # Get online friends
GET    /api/friend/users/search        # Search users
GET    /api/friend/requests            # Get friend requests
POST   /api/friend/request/:userId     # Send friend request
POST   /api/friend/accept/:requestId   # Accept friend request
DELETE /api/friend/decline/:requestId  # Decline friend request
DELETE /api/friend/remove/:friendshipId # Remove friend
```

### Game Endpoints

```
GET    /api/game/history               # Get match history
GET    /api/game/stats                 # Get game statistics
GET    /api/game/leaderboard           # Get global leaderboard
GET    /api/game/recent                # Get recent matches
POST   /api/game                       # Save game result (manual)
```

### Socket.io Namespaces

```
/quickmatch    # 1v1 lobby
/tournament    # Tournament lobby
/pong          # Ping Pong game
/keyclash      # Key Clash game
```

## 💻 Development

### Local Development (Without Docker)

#### Backend

```bash
cd pong-app/backend
npm install
npx prisma generate
npx prisma migrate dev
npm run dev
```

#### Frontend

```bash
cd pong-app/frontend
npm install
npm run dev
```

### Database Management

```bash
# Generate Prisma client
npx prisma generate

# Create migration
npx prisma migrate dev --name migration_name

# View database
npx prisma studio

# Reset database
npx prisma migrate reset
```

### Code Quality

```bash
# Backend linting
cd pong-app/backend
npm run lint

# Frontend linting
cd pong-app/frontend
npm run lint

# Type checking
npm run type-check
```

## 🐛 Troubleshooting

### Common Issues

#### 1. SSL Certificate Warnings
**Problem**: Browser shows security warning  
**Solution**: This is normal for self-signed certificates. Click "Advanced" → "Proceed to localhost (unsafe)"

#### 2. Port Already in Use
**Problem**: `Error: Port 3000/5173 is already in use`  
**Solution**:
```bash
# Stop containers
make down

# Check for running processes
lsof -i :3000
lsof -i :5173

# Kill process if needed
kill -9 <PID>
```

#### 3. Database Connection Issues
**Problem**: Prisma connection errors  
**Solution**:
```bash
# Rebuild database
docker compose down -v
make fclean
make run
```

#### 4. ngrok Tunnel Not Working
**Problem**: ngrok URL not accessible  
**Solution**:
- Verify `NGROK_AUTHTOKEN` in `docker-compose.yml`
- Check ngrok dashboard: `http://localhost:4040`
- Restart ngrok container:
```bash
docker compose restart ngrok
```

#### 5. Google OAuth Fails
**Problem**: "redirect_uri_mismatch" error  
**Solution**:
- Add correct redirect URIs in Google Console
- Ensure `GOOGLE_CLIENT_ID` matches in both backend and frontend `.env`
- Update `FRONTEND_REMOTE_URL` with your ngrok URL

#### 6. Email Not Sending
**Problem**: Verification emails not received  
**Solution**:
- Check spam folder
- Verify Gmail App Password (not regular password)
- Ensure 2FA is enabled on Gmail account
- Check backend logs: `make logs`

### Viewing Logs

```bash
# All services
make logs

# Specific service
docker compose -f ./pong-app/docker-compose.yml logs -f backend
docker compose -f ./pong-app/docker-compose.yml logs -f frontend
```

### Clean Restart

```bash
# Full cleanup
make fclean

# Rebuild from scratch
make run
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📝 License

This project is part of the 42 School curriculum.

## 👥 Authors

**Hivers5 Asteroids Team**

## 🙏 Acknowledgments

- Three.js for 3D graphics
- Socket.io for real-time communication
- 42 School for the project guidelines

---

**Note**: This application uses self-signed SSL certificates for local development. For production deployment, replace with proper SSL certificates from a trusted Certificate Authority.
