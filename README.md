# Sheeko Voice Rooms - Standalone Package

A real-time voice chat application for live audio sessions, built with LiveKit SFU architecture.

## Features

- 🎙️ Live voice rooms with 70+ participant support
- 👥 Role-based permissions (Host, Co-host, Speaker, Listener)
- ✋ Hand raise to request speaking privileges
- 💬 Real-time chat with emoji reactions
- 📌 Pin important messages
- 🎬 Session recording support
- 📅 Scheduled sessions with countdown timers
- 🔔 RSVP notifications for upcoming sessions
- 🔒 Moderation tools (kick, ban, mute)
- 📱 PWA support for mobile installation

## Architecture

### Audio Infrastructure
- **LiveKit SFU**: Selective Forwarding Unit for scalable audio (vs. mesh topology)
- Supports 70+ concurrent participants
- Automatic permission management based on user roles

### Stack
- **Frontend**: React 18, TypeScript, Vite, TanStack Query
- **Backend**: Express.js, Node.js
- **Database**: PostgreSQL with Drizzle ORM
- **Real-time**: WebSocket for signaling, LiveKit for audio
- **UI**: Tailwind CSS, shadcn/ui, Framer Motion

## Setup

### 1. Environment Variables
Create a `.env` file with:

```env
# Database
DATABASE_URL=postgresql://user:password@host:5432/sheeko

# LiveKit (get from cloud.livekit.io)
LIVEKIT_URL=wss://your-project.livekit.cloud
LIVEKIT_API_KEY=your-api-key
LIVEKIT_API_SECRET=your-api-secret

# Session
SESSION_SECRET=your-secret-key
```

### 2. Database Setup
Run the migration to create voice room tables:

```sql
-- See shared/schema.ts for full schema
-- Main tables: voice_rooms, voice_participants, voice_room_messages, 
-- voice_recordings, voice_room_bans, voice_room_rsvps, host_follows
```

### 3. Install Dependencies

```bash
npm install
```

### 4. Start Development Server

```bash
npm run dev
```

## File Structure

```
sheeko-standalone/
├── client/
│   ├── src/
│   │   ├── components/
│   │   │   └── VoiceSpaces.tsx    # Main voice room UI component
│   │   ├── hooks/
│   │   │   ├── useLiveKitRoom.ts  # LiveKit connection hook
│   │   │   ├── useVoiceRoom.ts    # WebRTC fallback (optional)
│   │   │   ├── useVoiceRecording.ts # Recording functionality
│   │   │   └── useWebSocket.ts    # Real-time signaling
│   │   └── contexts/
│   │       └── AuthContext.tsx    # User authentication context
├── server/
│   ├── routes.ts                  # API endpoints
│   ├── storage.ts                 # Database operations
│   └── websocket/
│       └── presence.ts            # WebSocket handler
└── shared/
    └── schema.ts                  # Database schema (Drizzle)
```

## API Endpoints

### Voice Rooms
- `GET /api/voice-rooms` - List all rooms
- `POST /api/voice-rooms` - Create new room
- `GET /api/voice-rooms/:id` - Get room details
- `POST /api/voice-rooms/:id/join` - Join room
- `POST /api/voice-rooms/:id/leave` - Leave room
- `POST /api/voice-rooms/:id/start` - Start room (host only)
- `POST /api/voice-rooms/:id/end` - End room (host only)

### LiveKit
- `POST /api/livekit/token` - Get LiveKit access token

### Participants
- `PATCH /api/voice-rooms/:id/participants/:participantId/role` - Change role
- `POST /api/voice-rooms/:id/participants/:participantId/kick` - Kick user
- `POST /api/voice-rooms/:id/participants/:participantId/ban` - Ban user

### Chat
- `GET /api/voice-rooms/:id/messages` - Get room messages
- `POST /api/voice-rooms/:id/messages` - Send message
- `DELETE /api/voice-rooms/:id/messages/:messageId` - Delete message
- `POST /api/voice-rooms/:id/pin/:messageId` - Pin message

### RSVP
- `POST /api/voice-rooms/:id/rsvp` - RSVP for scheduled room
- `DELETE /api/voice-rooms/:id/rsvp` - Cancel RSVP

### Follow
- `POST /api/hosts/:id/follow` - Follow host
- `DELETE /api/hosts/:id/follow` - Unfollow host
- `GET /api/hosts/:id/following` - Check follow status

## LiveKit Setup

1. Go to [cloud.livekit.io](https://cloud.livekit.io)
2. Create a new project
3. Go to Settings → Keys
4. Copy the URL, API Key, and API Secret
5. Add them to your environment variables

### Free Tier Limits
- 5,000 participant-minutes per month
- Supports: 40 participants × 2 hours × 2 sessions = 4,800 minutes

## Customization

### Adding Custom Authentication
Edit `client/src/contexts/AuthContext.tsx` to integrate your own auth system.

### Changing UI Theme
Update Tailwind CSS configuration in `tailwind.config.js`.

### Adding Features
- Voice recording to cloud storage
- Reactions and emoji animations
- AI moderation for chat messages

## License

MIT
