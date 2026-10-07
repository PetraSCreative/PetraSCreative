# PetraSCreative Architecture

## System Overview

```
┌─────────────────────────────────────────────────────────────┐
│                        Frontend (React)                      │
│                  (Vercel, SPA - JavaScript)                 │
└─────────────────────────────────────────────────────────────┘
                              |
                    (REST API / JSON)
                              |
┌─────────────────────────────────────────────────────────────┐
│                   Backend (Node.js/Express)                 │
│              (Render/Railway - Production)                  │
│                                                             │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐    │
│  │   Routes     │  │ Controllers  │  │   Services   │    │
│  └──────────────┘  └──────────────┘  └──────────────┘    │
│         |                 |                 |             │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐    │
│  │  Database    │  │    Redis     │  │  External    │    │
│  │ (PostgreSQL) │  │   (Cache)    │  │   APIs       │    │
│  └──────────────┘  └──────────────┘  └──────────────┘    │
└─────────────────────────────────────────────────────────────┘
                              |
        ┌─────────────────────┼─────────────────────┐
        |                     |                     |
   ┌─────────┐          ┌──────────┐          ┌──────────┐
   │Synthesia│          │Google    │          │  AWS S3  │
   │  API    │          │TTS API   │          │ (Videos) │
   │(Videos) │          │(Narration)           └──────────┘
   └─────────┘          └──────────┘
```

## Component Details

### Frontend (React)

**Key Modules:**
- **Pages:** Dashboard, Editor, VideoList, Settings
- **Components:** ScriptEditor, VideoPlayer, Timeline, VoiceSelector
- **Services:** API client, Authentication, Storage
- **State:** Redux (auth, videos, ui)
- **Styling:** Tailwind CSS

**Libraries:**
- `react-redux` - State management
- `axios` - HTTP client
- `react-router-dom` - Routing
- `framer-motion` - Animations
- `react-player` - Video playback

### Backend (Node.js)

**Architecture Layers:**

1. **Route Layer** (`/routes`)
   - Express route handlers
   - Request validation
   - Middleware application

2. **Controller Layer** (`/controllers`)
   - Business logic orchestration
   - Request/response handling
   - Error management

3. **Service Layer** (`/services`)
   - External API integration (Synthesia, Google TTS)
   - Video processing
   - Database operations
   - Business rules

4. **Model Layer** (`/models`)
   - Database schemas
   - Data validation
   - ORM (Sequelize/TypeORM)

5. **Middleware** (`/middleware`)
   - Authentication (JWT)
   - Authorization
   - Error handling
   - Request logging

**Database Schema:**

```sql
-- Users
CREATE TABLE users (
  id UUID PRIMARY KEY,
  email VARCHAR UNIQUE NOT NULL,
  password_hash VARCHAR NOT NULL,
  full_name VARCHAR,
  created_at TIMESTAMP,
  updated_at TIMESTAMP
);

-- Videos
CREATE TABLE videos (
  id UUID PRIMARY KEY,
  user_id UUID REFERENCES users(id),
  title VARCHAR NOT NULL,
  description TEXT,
  script_content TEXT,
  status VARCHAR (processing|completed|failed),
  video_url VARCHAR,
  thumbnail_url VARCHAR,
  duration INT,
  created_at TIMESTAMP,
  updated_at TIMESTAMP
);

-- Narrations
CREATE TABLE narrations (
  id UUID PRIMARY KEY,
  video_id UUID REFERENCES videos(id),
  voice_id VARCHAR,
  audio_url VARCHAR,
  duration INT,
  created_at TIMESTAMP
);

-- Video Segments
CREATE TABLE video_segments (
  id UUID PRIMARY KEY,
  video_id UUID REFERENCES videos(id),
  sequence INT,
  content TEXT,
  duration INT,
  transition_id VARCHAR,
  animation_id VARCHAR
);
```

### External Services

**Synthesia API:**
- Text-to-video generation
- Avatar selection
- Background customization
- Webhook for completion notifications

**Google Cloud Text-to-Speech:**
- Natural voiceover generation
- Multiple language support
- Voice customization (pitch, speed)

**AWS S3:**
- Video storage
- Thumbnail storage
- Public URL generation
- Lifecycle policies for cleanup

## Data Flow

### Video Generation Flow

```
1. User enters script → Frontend sends to /api/videos/generate
2. Backend validates script and creates video record (status: processing)
3. Backend calls Synthesia API with script + voice params
4. Synthesia processes asynchronously
5. Synthesia sends webhook when complete
6. Backend updates video record with video_url (status: completed)
7. Frontend polls GET /api/videos/:id to check status
8. Video appears in user's gallery when completed
```

### Video Customization Flow

```
1. User opens video in editor
2. User rearranges segments, adds transitions/animations
3. Frontend sends PUT /api/videos/:id with new configuration
4. Backend queues video recomposition job (FFmpeg)
5. Backend uses FFmpeg to rebuild video with new effects
6. Recomposed video saved to S3
7. Frontend updates video preview
8. User exports final video
```

## Scalability Considerations

1. **Job Queue:** Use Bull/BullMQ for async processing
2. **Caching:** Redis for frequently accessed data
3. **CDN:** CloudFront for video delivery
4. **Database:** Connection pooling, read replicas
5. **API Rate Limiting:** Prevent abuse
6. **Load Balancing:** Multiple backend instances

## Security

1. **Authentication:** JWT with refresh tokens
2. **Authorization:** Role-based access control (RBAC)
3. **Data Encryption:** TLS for transit, at-rest encryption for videos
4. **API Keys:** Secure storage for Synthesia/Google Cloud keys
5. **Input Validation:** Sanitize all user inputs
6. **CORS:** Restrict to trusted domains
