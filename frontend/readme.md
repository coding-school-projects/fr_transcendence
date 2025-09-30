# Frontend - Pong Game Platform

React SPA with Three.js 3D graphics, providing an immersive gaming experience with real-time multiplayer functionality.

## 📋 Overview

The frontend provides:
- **3D Ping Pong Game**: Three.js powered realistic table tennis
- **Key Clash Game**: Fast-paced typing competition
- **User Interface**: Lobby system, profile management, statistics
- **Real-time Updates**: Socket.io client for live gameplay
- **Authentication**: Complete auth flow with 2FA
- **Social Features**: Friend system with online status

## 🛠 Tech Stack

- **Framework**: React 18
- **Build Tool**: Vite
- **Language**: TypeScript
- **Routing**: React Router v6
- **Styling**: TailwindCSS
- **3D Graphics**: Three.js (r128)
- **Real-time**: Socket.io-client
- **HTTP Client**: Axios
- **Validation**: validator

## 📁 Project Structure

```
frontend/
├── src/
│   ├── main.tsx                    # App entry point
│   ├── app.tsx                     # Root component with routing
│   ├── styles.css                  # Global styles (Tailwind)
│   │
│   ├── pages/                      # Page components
│   │   ├── unauthorised/          # Public pages
│   │   │   ├── home.tsx           # Landing page
│   │   │   ├── login.tsx          # Login page
│   │   │   ├── register.tsx       # Registration
│   │   │   ├── verify-code.tsx    # 2FA verification
│   │   │   ├── reset-password.tsx # Password reset
│   │   │   └── changePassword.tsx # Password change
│   │   ├── authorised/            # Protected pages
│   │   │   ├── lobby.tsx          # Main lobby
│   │   │   ├── quickmatch.tsx     # 1v1 lobby
│   │   │   └── tournament.tsx     # Tournament lobby
│   │   ├── game/
│   │   │   └── playPage.tsx       # Game container
│   │   └── general/
│   │       └── avatar.tsx         # Avatar selection
│   │
│   ├── components/                 # Reusable components
│   │   ├── general/               # Shared components
│   │   │   ├── Alert.tsx
│   │   │   ├── ErrorBoundary.tsx
│   │   │   ├── Layout.tsx
│   │   │   ├── LoadingSpinner.tsx
│   │   │   └── Navbar.tsx
│   │   ├── lobby/                 # Lobby tab components
│   │   │   ├── OverviewTab.tsx
│   │   │   ├── MyLockerTab.tsx
│   │   │   ├── RallySquadTab.tsx
│   │   │   └── MatchHistoryTab.tsx
│   │   ├── quickmatch-lobby/
│   │   │   └── QuickmatchPlayerForm.tsx
│   │   └── tournament-lobby/
│   │       └── TournamentPlayerForm.tsx
│   │
│   ├── contexts/                   # React contexts
│   │   └── AuthContext.tsx        # Authentication state
│   │
│   ├── utils/                      # Utilities
│   │   ├── api.ts                 # Axios instance
│   │   ├── PingPongClient.ts      # Pong game client
│   │   └── keyClashClient.ts      # Key Clash client
│   │
│   └── env.d.ts                   # Environment types
│
├── public/                         # Static assets
│   ├── background/                # Background images
│   ├── avatars/                   # Avatar images
│   └── profile-pics/              # Default profile pics
│
├── index.html                     # HTML template
├── vite.config.ts                 # Vite configuration
├── tailwind.config.js             # TailwindCSS config
├── tsconfig.json                  # TypeScript config
├── Dockerfile
├── package.json
└── README.md                      # This file
```

## ⚙️ Installation

### Using Docker (Recommended)

From project root:
```bash
make run
```

### Local Development

```bash
cd frontend
npm install
npm run dev
```

## 🔧 Configuration

### Environment Variables (`.env`)

```env
# Backend API URL
VITE_API_URL=https://localhost:3000

# Google OAuth
VITE_GOOGLE_CLIENT_ID=your-google-client-id.apps.googleusercontent.com
```

### Vite Configuration

Key configurations in `vite.config.ts`:
- HTTPS with self-signed certificates
- Proxy for API requests
- Host: `0.0.0.0` for Docker

```typescript
export default defineConfig({
  server: {
    host: '0.0.0.0',
    port: 5173,
    https: {
      key: fs.readFileSync('./tls/key.pem'),
      cert: fs.readFileSync('./tls/cert.pem'),
    },
    proxy: {
      '/api': {
        target: 'https://localhost:3000',
        changeOrigin: true,
        secure: false,
        rewrite: (path) => path.replace(/^\/api/, ''),
      },
    },
  },
});
```

## 🎮 Game Clients

### PingPongClient.ts

**Purpose**: Manages 3D Pong game rendering and Socket.io communication

**Key Features**:
- Three.js scene management
- 60 FPS game loop
- Ball physics prediction
- Camera smoothing
- Paddle collision detection
- Score and timer display

**Usage**:
```typescript
const game = new PingPongClient(
  containerElement,
  gameId,
  mode,           // "local" | "remote"
  type,           // "1v1" | "tournament"
  navigate,
  playerNames,
  playerId
);

// Cleanup on unmount
game.dispose();
```

**Controls**:
- Local: W/S (left paddle), Arrow Up/Down (right paddle)
- Remote: W/S or Arrow Up/Down
- Pause: ESC (local only)
- Ready: SPACE

### keyClashClient.ts

**Purpose**: Manages Key Clash game logic and Socket.io communication

**Key Features**:
- Key press detection
- Prompt display
- Score tracking
- Timer countdown
- Visual feedback

**Usage**:
```typescript
const cleanup = KeyClashClient(
  containerElement,
  gameId,
  mode,
  type,
  navigate,
  playerNames,
  playerId
);

// Cleanup on unmount
cleanup();
```

**Controls**:
- Local: WASD (left), Arrow Keys (right)
- Remote: WASD or Arrow Keys
- Ready: SPACE

## 🔌 Socket.io Integration

### Connection Setup

```typescript
const socket = io("/namespace", {
  path: '/socket.io',
  transports: ['websocket'],
  secure: true
});
```

### Namespaces Used

- `/quickmatch` - 1v1 lobby
- `/tournament` - Tournament lobby
- `/pong` - Ping Pong game
- `/keyclash` - Key Clash game

### Event Handling Example

```typescript
// Lobby events
socket.on("lobby_update", (data) => {
  setPlayers(data.players);
  setPongGames(data.pongGames);
  setKeyClashGames(data.keyClashGames);
});

// Game events
socket.on("stateUpdate", (state) => {
  updateBallPosition(state.ball);
  updatePaddles(state.leftPaddle, state.rightPaddle);
  updateScore(state.scoreDisplay);
});
```

## 🎨 Styling

### TailwindCSS

Configuration in `tailwind.config.js`:
```javascript
module.exports = {
  content: ['./src/**/*.{js,jsx,ts,tsx}'],
  theme: {
    extend: {
      colors: {
        'game-bg': '#1b1443',
        'game-accent': '#7d2ae8',
      },
    },
  },
};
```

## 🔐 Authentication Context

### AuthContext.tsx

**Purpose**: Global authentication state management

**Features**:
- User state management
- Login/logout functions
- Session persistence
- BroadcastChannel for cross-tab sync

**Usage**:
```tsx
const { user, login, logout, isLoading } = useAuth();

if (isLoading) return <LoadingSpinner />;
if (!user) return <Navigate to="/login" />;
```

## 📱 Routing

### Route Structure

```typescript
<Routes>
  {/* Public routes */}
  <Route path="/" element={<Home />} />
  <Route path="/login" element={<PublicRoute><LoginPage /></PublicRoute>} />
  
  {/* Protected routes */}
  <Route path="/lobby" element={<ProtectedRoute><LobbyPage /></ProtectedRoute>} />
  
  {/* Game routes */}
  <Route path="/:game/:mode/:type/:gameId" element={<PlayPage />} />
</Routes>
```

## 🧪 Development

### Run Development Server
```bash
npm run dev
```

### Build for Production
```bash
npm run build
```

### Preview Production Build
```bash
npm run preview
```

### Type Checking
```bash
npm run type-check
```

## 🎯 Key Features Implementation

### Avatar Selection

**File**: `src/pages/general/avatar.tsx`

**Features**:
- 12 unique avatar options
- Shows "Already Taken" for selected avatars
- Returns to previous page with selected avatar

### Profile Customization

**File**: `src/components/lobby/MyLockerTab.tsx`

**Features**:
- Profile picture upload (base64)
- Avatar selection
- Personal information
- Validation and error handling
- Unsaved changes detection

### Friend System

**File**: `src/components/lobby/RallySquadTab.tsx`

**Features**:
- Real-time online status
- Friend requests (send/accept/decline)
- User search
- Friend removal
- Online/offline filtering

### Match History

**File**: `src/components/lobby/MatchHistoryTab.tsx`

**Features**:
- Detailed match information
- Win/loss statistics
- Global leaderboard
- Round-by-round details

## 🐛 Common Issues

### HTTPS Certificate Warning
Accept the self-signed certificate in browser settings.

### Socket.io Connection Failed
- Check backend is running
- Verify VITE_API_URL in `.env`
- Check browser console for CORS errors

### Three.js Performance Issues
- Reduce particles
- Lower shadow quality
- Check GPU acceleration

### Build Errors
```bash
rm -rf node_modules package-lock.json
npm install
npm run dev
```

## 🚀 Production Build

### Build Steps

```bash
npm install
npm run build
# Output in dist/ folder
```

### Optimization

The production build:
- Minifies JavaScript
- Optimizes CSS
- Code splitting
- Tree shaking
- Asset optimization

## 📊 Performance Optimization

### Best Practices

1. **Code Splitting**: Use lazy loading
```tsx
const LobbyPage = lazy(() => import('./pages/authorised/lobby'));
```

2. **Memoization**: Use React.memo
```tsx
export default React.memo(ExpensiveComponent);
```

3. **Three.js**: Dispose unused resources
```typescript
geometry.dispose();
material.dispose();
renderer.dispose();
```

## 📄 License

Part of 42 School curriculum.
