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

## 📚 About The Project

ft_transcendence is 42 School's final web project that reimagines the classic Pong game with modern web technologies. This full-stack application combines real-time multiplayer gameplay with social features, tournament systems, and cutting-edge web development practices.

**Key Features:**
- Real-time Pong with WebSocket communication
- Tournament system with matchmaking
- User authentication (local + OAuth)
- Live chat with direct messaging
- AI opponent with adaptive difficulty
- Blockchain-based score tracking (bonus)
- 3D-rendered Pong (bonus)

## 🏁 Getting Started

### 🛠️ Installation & Setup

1. Clone the repository:
```bash
git clone https://github.com/coding-school-projects/ft_transcendence.git
cd ft_transcendence
```

2. Set up environment variables:
```bash
cp .env.example .env
# Fill in your credentials
```

3. Start with Docker:
```bash
docker-compose up --build
```

4. Access the application:
```
https://localhost:3000
```

### 🎮 Running the Project

**Basic Usage:**
1. Register or log in (supports Google OAuth)
2. Navigate to the game section
3. Play local Pong or join a tournament
4. Chat with other players in real-time

**Admin Commands:**
```bash
docker-compose exec backend npm run migrate
docker-compose exec backend npm run seed
```

## 🧠 Technical Implementation

### Core Architecture
| Component | Technology | Description |
|-----------|------------|-------------|
| Frontend | React/TypeScript | Single-page application with Tailwind CSS |
| Backend | NestJS (Fastify) | Microservice architecture |
| Database | PostgreSQL | Persistent data storage |
| Real-time | Socket.io | Game state and chat synchronization |
| Auth | JWT + OAuth2 | Secure authentication |
| Blockchain | Avalanche (testnet) | Tournament score storage |

### Implemented Modules
1. **Web Framework** (NestJS/Fastify)
2. **User Management** (Local + Google Auth)
3. **Remote Players** (WebSocket multiplayer)
4. **Live Chat**
5. **AI Opponent**
6. **Cybersecurity** (WAF + 2FA)
7. **3D Graphics** (Babylon.js)
8. **Blockchain Integration** (Avalanche)

## 🧪 Testing Protocol

1. Basic functionality test:
```bash
curl -k https://localhost:3000/api/health
```

2. Gameplay test:
- Open two browsers and play Pong
- Verify tournament progression

3. Security tests:
```bash
npm run test:e2e
```

4. Blockchain verification:
```bash
npm run test:blockchain
```

## 📝 Evaluation Criteria

1. **Functionality**: All mandatory features working
2. **Performance**: Smooth real-time gameplay
3. **Security**: No vulnerabilities detected
4. **Code Quality**: Clean, documented, norm-compliant
5. **Bonus**: Additional modules implemented

### 🧑‍💻 Peer Evaluations (3/3)

> **Peer 1**: " "

> **Peer 2**: " "

> **Peer 3**: " "


