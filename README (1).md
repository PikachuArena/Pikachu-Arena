# Pikachu Arena

A real-time multiplayer Pokemon battle game featuring turn-based combat, 3D graphics, and blockchain-based wagering on the Solana network.

## Overview

Pikachu Arena is a competitive Pokemon battle platform where players can select from 6+ unique Pokemon, engage in 1v1 turn-based battles with real-time matchmaking, and optionally stake SOL tokens on match outcomes. The platform combines classic Pokemon battle mechanics with modern Web3 technology.

## Features

### Core Gameplay
- **6+ Playable Pokemon**: Pikachu, Bulbasaur, Charizard, Gengar, Mewtwo, and Shiny Lugia (Legendary)
- **Turn-Based Combat**: Strategic battles with attack, defend, and special ability options
- **Active Dodge Mechanics**: WASD/Arrow key movement during battles
- **Mobile Support**: Virtual joystick for touch-based devices
- **Real-Time Matchmaking**: WebSocket-based instant opponent matching

### Visual & Audio
- **3D Pokemon Models**: Fully rendered 3D models using React Three Fiber
- **Attack Visual Effects**: Dynamic particle effects and animations
- **Background Music & Sound Effects**: Immersive audio experience
- **Pokemon-Inspired Theme**: Bright yellow Pikachu-themed UI

### Competitive Features
- **Ranking System**: Bronze, Silver, Gold, Diamond, and Champion tiers
- **Win Streak Tracking**: Consecutive victory bonuses
- **Leaderboard**: Global rankings with top player display
- **Daily Prize Pool**: Community competition incentives

### Wager System (Optional)
- **SOL Token Wagering**: Stake native SOL on match outcomes
- **Wallet Integration**: Phantom and Solflare wallet support
- **Automated Escrow**: Server-side escrow wallet for secure deposits
- **Instant Payouts**: Automated winner payouts upon battle completion
- **Free Play Mode**: Non-wager matches available for all users

## Wager Match System

### Overview

Pikachu Arena features a trustless wagering system built on the Solana blockchain. Players can stake real SOL tokens on battle outcomes, with winners receiving the full prize pool automatically upon victory.

### How It Works

#### 1. Creating a Wager Room

```
Player A → Connects Wallet → Selects Stake Amount → Signs Transaction → Room Created
```

- Connect your Phantom or Solflare wallet
- Choose your Pokemon for the battle
- Select stake amount (0.05, 0.1, 0.25, 0.5, or 1.0 SOL)
- Confirm the transaction to deposit SOL to the escrow wallet
- Your room appears in the open wager rooms lobby

#### 2. Joining a Wager Room

```
Player B → Views Open Rooms → Selects Room → Matches Stake → Signs Transaction → Battle Starts
```

- Browse available wager rooms in the lobby
- Each room displays the creator's Pokemon and stake amount
- Click "Join Game" to match the stake
- Confirm the transaction to deposit your SOL
- Both players are immediately transported to the battle arena

#### 3. Battle & Payout

```
Battle Concludes → Winner Determined → Server Sends Payout → SOL Transferred to Winner
```

- Standard turn-based battle mechanics apply
- The winner receives the entire prize pool (2x the stake)
- Payouts are processed automatically by the server
- Transaction is recorded on-chain for transparency

### Stake Options

| Stake Amount | Prize Pool | Network Fee (Est.) |
|--------------|------------|-------------------|
| 0.05 SOL     | 0.10 SOL   | ~0.000005 SOL     |
| 0.10 SOL     | 0.20 SOL   | ~0.000005 SOL     |
| 0.25 SOL     | 0.50 SOL   | ~0.000005 SOL     |
| 0.50 SOL     | 1.00 SOL   | ~0.000005 SOL     |
| 1.00 SOL     | 2.00 SOL   | ~0.000005 SOL     |

### Escrow System

The platform uses a secure server-side escrow system:

1. **Deposit Phase**: Both players send SOL to the escrow wallet before the battle begins
2. **Verification**: Server verifies on-chain transaction confirmations
3. **Battle Phase**: Funds are held securely during the match
4. **Settlement**: Upon battle completion, the server automatically transfers the full pot to the winner's wallet

### Supported Wallets

| Wallet    | Status    | Features                          |
|-----------|-----------|-----------------------------------|
| Phantom   | Supported | Full transaction signing support  |
| Solflare  | Supported | Full transaction signing support  |

### Wager Room States

| State      | Description                                      |
|------------|--------------------------------------------------|
| `open`     | Room created, waiting for opponent              |
| `matched`  | Both players deposited, battle in progress      |
| `settled`  | Battle complete, winner paid out                |
| `cancelled`| Room cancelled (refund if applicable)           |

### Security Features

- **On-Chain Verification**: All deposits are verified on the Solana blockchain
- **Server-Side Escrow Key**: Private key never exposed to clients
- **Transaction Confirmation**: Deposits confirmed before battle creation
- **Automated Settlement**: No manual intervention required for payouts
- **Audit Trail**: All wager rooms and transactions logged in database

### Technical Flow

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Player A  │     │   Server    │     │   Player B  │
└──────┬──────┘     └──────┬──────┘     └──────┬──────┘
       │                   │                   │
       │ Create Room       │                   │
       │ (Deposit SOL)     │                   │
       │──────────────────>│                   │
       │                   │                   │
       │   Room Created    │                   │
       │<──────────────────│                   │
       │                   │                   │
       │                   │    Join Room      │
       │                   │   (Deposit SOL)   │
       │                   │<──────────────────│
       │                   │                   │
       │  Battle Start     │   Battle Start    │
       │<──────────────────│──────────────────>│
       │                   │                   │
       │        ... Battle in Progress ...     │
       │                   │                   │
       │   Battle End      │   Battle End      │
       │<──────────────────│──────────────────>│
       │                   │                   │
       │                   │ Payout to Winner  │
       │                   │──────────────────>│
       │                   │                   │
```

### Error Handling

| Error                    | Cause                              | Resolution                        |
|--------------------------|------------------------------------|-----------------------------------|
| Insufficient Balance     | Not enough SOL in wallet           | Add more SOL to your wallet       |
| Transaction Rejected     | User cancelled in wallet           | Try again and confirm transaction |
| Wallet Not Connected     | Wallet disconnected                | Reconnect your wallet             |
| Room Not Found           | Room was cancelled or filled       | Refresh and select another room   |

## Technology Stack

### Frontend
- React 18 with TypeScript
- React Three Fiber for 3D rendering
- Tailwind CSS for styling
- Zustand for state management
- Framer Motion for animations
- Socket.IO Client for real-time communication

### Backend
- Node.js with Express
- Socket.IO for WebSocket communication
- PostgreSQL database with Drizzle ORM
- Session-based authentication with bcrypt

### Blockchain
- Solana Web3.js
- Solana Wallet Adapter (Phantom, Solflare)
- Native SOL token transactions

## Installation

### Prerequisites
- Node.js 18+
- PostgreSQL database
- Solana wallet (for wager features)

### Setup

1. Clone the repository:
```bash
git clone https://github.com/yourusername/pikachu-arena.git
cd pikachu-arena
```

2. Install dependencies:
```bash
npm install
```

3. Configure environment variables:
```bash
cp .env.example .env
```

Required environment variables:
```
DATABASE_URL=postgresql://user:password@host:port/database
SESSION_SECRET=your-session-secret
SOLANA_RPC_URL=https://api.mainnet-beta.solana.com
ESCROW_PRIVATE_KEY=your-escrow-wallet-private-key
```

4. Initialize the database:
```bash
npm run db:push
```

5. Start the development server:
```bash
npm run dev
```

The application will be available at `http://localhost:5000`.

## Project Structure

```
pikachu-arena/
├── client/                 # Frontend React application
│   ├── public/            # Static assets (3D models, textures, audio)
│   └── src/
│       ├── components/    # React components
│       ├── lib/           # Utilities and state stores
│       └── hooks/         # Custom React hooks
├── server/                # Backend Express server
│   ├── routes.ts         # API endpoints
│   ├── storage.ts        # Database operations
│   └── index.ts          # Server entry point
├── shared/               # Shared types and schemas
│   └── schema.ts        # Drizzle ORM schema
└── package.json
```

## API Endpoints

### Authentication
- `POST /api/auth/register` - Create new account
- `POST /api/auth/login` - User login
- `POST /api/auth/logout` - User logout
- `GET /api/auth/me` - Get current user

### Game
- `GET /api/leaderboard` - Fetch top players
- `POST /api/battles` - Record battle results

### Wager System
- `GET /api/wager-rooms` - List open wager rooms
- `POST /api/wager-rooms` - Create wager room
- `POST /api/wager-rooms/:id/join` - Join wager room
- `POST /api/wager-rooms/:id/settle` - Settle completed battle

## WebSocket Events

### Client Events
- `joinMatchmaking` - Enter matchmaking queue
- `leaveMatchmaking` - Exit matchmaking queue
- `attack` - Send attack action
- `registerSocket` - Register user socket for wager matches

### Server Events
- `matchFound` - Opponent found notification
- `wagerMatchFound` - Wager battle ready
- `battleStart` - Battle initialization
- `turnChange` - Turn transition
- `battleEnd` - Battle conclusion with results

## Security Considerations

- All passwords are hashed using bcrypt
- Session-based authentication with secure cookies
- Escrow wallet private key stored server-side only
- Transaction verification before battle creation
- Input validation using Zod schemas

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/new-feature`)
3. Commit changes (`git commit -am 'Add new feature'`)
4. Push to branch (`git push origin feature/new-feature`)
5. Open a Pull Request

## License

MIT License - see [LICENSE](LICENSE) for details.

## Disclaimer

This project involves real cryptocurrency transactions. Users participate at their own risk. Always verify transaction details before confirming wallet operations. The developers are not responsible for any financial losses incurred through the use of this platform.
