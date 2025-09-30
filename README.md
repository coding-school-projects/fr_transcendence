<h1 align="center">
  <img src="https://github.com/senthilpoo10/badges/blob/main/badges/ft_transcendencem.png" width="200"/>
</h1>

<p align="center">
  <b><i>The Ultimate Pong Experience</i></b><br>
  <i>"It's time to shine!"</i>
</p>

<p align="center">
  <img alt="score" src="https://img.shields.io/badge/score-125%2F100-brightgreen" />
  <img alt="team" src="https://img.shields.io/badge/team-4%20members-yellow" />
  <img alt="time spent" src="https://img.shields.io/badge/time%20spent-300%20hours-blue" />
  <img alt="XP earned" src="https://img.shields.io/badge/XP%20earned-2016-orange" />
<p align="center">
  <img alt="GitHub code size in bytes" src="https://img.shields.io/github/languages/code-size/coding-school-projects/ft_transcendence?color=lightblue" />
  <img alt="GitHub top language" src="https://img.shields.io/github/languages/top/coding-school-projects/ft_transcendence?color=blue" />
  <img alt="GitHub last commit" src="https://img.shields.io/github/last-commit/coding-school-projects/ft_transcendence?color=green" />
</p>

# 🏓 Hivers5 Asteroids - Pong Game Platform

A full-stack, real-time multiplayer gaming platform featuring classic Ping Pong and Key Clash games with tournament modes, friend systems, and comprehensive player statistics.

## 📋 Overview

This project consists of two main components:
- **Backend**: Node.js/Fastify REST API + Socket.io game servers
- **Frontend**: React SPA with Three.js 3D graphics

### Key Features

🎮 **Gaming**
- 3D Ping Pong with realistic physics (Three.js)
- Key Clash typing competition game
- 1v1 Quick Match (Local & Remote)
- Tournament Mode (4-player bracket)
- Real-time multiplayer with Socket.io

👥 **Social Features**
- Friend system with online status
- Friend requests management
- Player search and discovery

📊 **Statistics**
- Match history tracking
- Win/loss statistics
- Global leaderboard
- Win streak tracking

🔐 **Security**
- JWT authentication
- Two-Factor Authentication (2FA)
- Google OAuth integration
- Email verification

## 🛠 Tech Stack

| Layer | Technology |
|-------|-----------|
| **Backend** | Node.js, Fastify, Socket.io, Prisma, SQLite |
| **Frontend** | React, Vite, Three.js, TailwindCSS |
| **Real-time** | Socket.io |
| **Deployment** | Docker, Docker Compose, ngrok |
| **Database** | SQLite with Prisma ORM |

## 📦 Prerequisites

- Docker (v20.10+) & Docker Compose (v2.0+)
- Make (for Makefile commands)
- Node.js 20+ (for local development)
- Git

## 🚀 Quick Start

### 1. Clone Repository

```bash
git clone <your-repo-url>
cd pong-app
```

### 2. Configure Environment

#### Backend (`backend/.env`)
```env
PORT=3000
DATABASE_URL="file:./dev.db"
JWT_SECRET=your-super-secret-jwt-key-change-in-production
COOKIE_SECRET=your-super-secret-cookie-key-change-in-production
FRONTEND_URL=https://localhost:5173
FRONTEND_REMOTE_URL=https://your-ngrok-url.ngrok.io
EMAIL_USER=your-email@gmail.com
EMAIL_PASSWORD=your-gmail-app-password
GOOGLE_CLIENT_ID=your-google-client-id
GOOGLE_CLIENT_SECRET=your-google-client-secret
```

#### Frontend (`frontend/.env`)
```env
VITE_API_URL=https://localhost:3000
VITE_GOOGLE_CLIENT_ID=your-google-client-id
```

#### Docker Compose (`docker-compose.yml`)
```yaml
ngrok:
  environment:
    - NGROK_AUTHTOKEN=your-ngrok-token  # Get from https://ngrok.com
```

### 3. Start Application

```bash
# View all commands
make help

# Build and start all services
make run

# View logs
make logs

# Stop services
make down

# Full cleanup
make fclean
```

### 4. Access Application

- **Frontend**: https://localhost:5173
- **Backend API**: https://localhost:3000
- **ngrok Dashboard**: http://localhost:4040

**Note**: Accept the self-signed certificate warning in your browser (Advanced → Proceed to localhost).

## 📁 Project Structure

```
pong-app/
├── backend/                 # Backend application
│   ├── src/                # Source code
│   ├── prisma/             # Database schema
│   ├── Dockerfile
│   └── README.md           # Backend documentation
│
├── frontend/               # Frontend application
│   ├── src/                # Source code
│   ├── public/             # Static assets
│   ├── Dockerfile
│   └── README.md           # Frontend documentation
│
├── ngrok/                  # ngrok tunnel setup
├── tls/                    # SSL certificates
├── docker-compose.yml      # Docker orchestration
├── Makefile               # Build commands
└── README.md              # This file
```

## 🎮 Game Modes

### Ping Pong (3D Table Tennis)
- **Controls**: W/S (left paddle) or Arrow Keys (right paddle)
- **Scoring**: First to 3 points wins
- **Duration**: 2 minutes maximum
- **Features**: Realistic physics, camera tracking, score display

### Key Clash (Typing Game)
- **Controls**: WASD keys (left player) or Arrow Keys (right player)
- **Scoring**: +1 point for correct key, -1 for wrong key
- **Duration**: 20 seconds per round
- **Features**: Real-time key prompts, visual feedback

### Tournament Mode
- 4-player single-elimination bracket
- 3 rounds total: Semi-final 1, Semi-final 2, Final
- Available for both Ping Pong and Key Clash
- Automatic matchmaking and progression

## 📚 Documentation

- **[Backend Documentation](./backend/README.md)** - API endpoints, database schema, Socket.io events
- **[Frontend Documentation](./frontend/README.md)** - Component structure, routing, game clients

## 🔧 Development

### Run Backend Locally
```bash
cd backend
npm install
npx prisma generate
npx prisma migrate dev
npm run dev
```

### Run Frontend Locally
```bash
cd frontend
npm install
npm run dev
```

See individual README files for detailed development instructions.

## 🐛 Troubleshooting

### SSL Certificate Warning
**Solution**: Accept the self-signed certificate in your browser (Click "Advanced" → "Proceed to localhost").

### Port Already in Use
```bash
make down
lsof -i :3000  # or :5173
kill -9 <PID>
```

### Database Issues
```bash
make fclean
make run
```

### ngrok Not Working
- Verify `NGROK_AUTHTOKEN` in docker-compose.yml
- Check ngrok dashboard: http://localhost:4040
- Restart ngrok: `docker compose restart ngrok`

### Email Not Sending
- Check Gmail App Password (not regular password)
- Verify 2FA is enabled on Gmail account
- Check spam folder
- View backend logs: `make logs`

## 📝 Available Make Commands

| Command | Description |
|---------|-------------|
| `make help` | Show all available commands |
| `make run` | Build and start all services |
| `make up` | Start containers (if already built) |
| `make down` | Stop and remove containers |
| `make logs` | View container logs (follow mode) |
| `make status` | Show container status |
| `make clean` | Stop and remove containers only |
| `make fclean` | Full cleanup (volumes, images, cache) |
| `make re` | Full rebuild (fclean + run) |

## 🚀 Deployment

### Production Checklist
- [ ] Replace self-signed SSL certificates with valid certificates
- [ ] Generate strong JWT_SECRET and COOKIE_SECRET (minimum 32 characters)
- [ ] Configure production database (PostgreSQL recommended over SQLite)
- [ ] Set up proper email service (SendGrid, AWS SES, etc.)
- [ ] Configure CORS for production domain
- [ ] Enable rate limiting on API endpoints
- [ ] Set up monitoring and logging (PM2, Winston, Sentry)
- [ ] Configure automated backup strategy
- [ ] Set `NODE_ENV=production` in all services
- [ ] Update all URLs to production domains

## 🤝 Contributing

1. Fork the repository
2. Create feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open Pull Request

## 📄 License

This project is part of the 42 School curriculum.

## 👥 Team

**Hivers5 Asteroids**

## 🙏 Acknowledgments

- Three.js for 3D graphics engine
- Socket.io for real-time communication
- 42 School for project guidelines
- Prisma for database ORM
- Fastify for efficient backend framework

---

**For detailed documentation, see:**
- [Backend Documentation](./backend/README.md)
- [Frontend Documentation](./frontend/README.md)

