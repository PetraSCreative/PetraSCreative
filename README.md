# PetraSCreative

**AI-Powered Text-to-Video Generator for E-Learning**

PetraSCreative is an innovative platform that transforms text into engaging, animated videos with AI-generated voiceover narration. Designed for e-learning creators, content creators, and corporate training teams.

## Features

✨ **Core MVP Features:**
- **Text-to-Video Generation**: Convert written scripts into professional videos
- **AI Voiceover**: Automatic narration with customizable voices and languages
- **Animations & Transitions**: Pre-built templates and effects library
- **Video Editor**: Customize, rearrange, and fine-tune generated videos
- **Template Library**: Professional templates for different course types

## Tech Stack

**Frontend:**
- React 18+
- TypeScript
- Tailwind CSS
- Redux for state management
- Vite for build tooling

**Backend:**
- Node.js + Express
- PostgreSQL
- Redis (caching)
- Synthesia API (video generation)
- Google Cloud Text-to-Speech (narration)
- FFmpeg (video processing)

**Hosting:**
- Vercel (Frontend)
- Render/Railway (Backend)
- AWS S3 (Video storage)

## Project Structure

```
PetraSCreative/
├── frontend/                 # React app
│   ├── src/
│   │   ├── components/       # Reusable React components
│   │   ├── pages/            # Page components
│   │   ├── services/         # API calls
│   │   ├── store/            # Redux state
│   │   └── styles/           # Tailwind CSS
│   └── package.json
├── backend/                  # Node.js/Express API
│   ├── src/
│   │   ├── routes/           # API endpoints
│   │   ├── controllers/      # Business logic
│   │   ├── models/           # Database models
│   │   ├── services/         # External APIs
│   │   ├── middleware/       # Auth, validation
│   │   └── config/           # Configuration
│   └── package.json
├── docs/                     # Documentation
├── .github/workflows/        # CI/CD pipelines
└── package.json              # Monorepo root
```

## Getting Started

### Prerequisites
- Node.js 18+
- npm or yarn
- PostgreSQL 12+
- Redis

### Installation

```bash
# Clone the repository
git clone https://github.com/PetraSCreative/PetraSCreative.git
cd PetraSCreative

# Install dependencies
npm install

# Setup environment variables
cp .env.example .env.local

# Run development servers
npm run dev
```

## MVP Roadmap

See [ROADMAP.md](./ROADMAP.md) for detailed milestones and timelines.

## API Documentation

See [API.md](./docs/API.md) for backend endpoints.

## Contributing

Contributions are welcome! Please read [CONTRIBUTING.md](./CONTRIBUTING.md) for guidelines.

## License

MIT

## Support

For issues and feature requests, please open a GitHub issue.
