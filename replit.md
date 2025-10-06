# Cloudflare VibeSDK - Replit Environment

## Overview
This is the Cloudflare VibeSDK project - an open-source full-stack AI web app generator originally designed for Cloudflare's platform. This Replit environment runs the frontend development server for preview and development purposes.

**Project Type:** Full-stack AI application  
**Frontend:** React + Vite + TypeScript + Tailwind CSS  
**Backend:** Cloudflare Workers (not available in standard Replit environment)  
**Status:** Frontend running in development mode

## Recent Changes (October 6, 2025)
- Imported GitHub repository to Replit
- Fixed package.json dependency conflicts (removed conflicting overrides, updated @cloudflare/containers version)
- Configured Vite to use port 5000 and bind to 0.0.0.0 for Replit environment
- Disabled Cloudflare Vite plugin (requires Cloudflare infrastructure)
- Installed dependencies with `--legacy-peer-deps` flag
- Created .dev.vars file with JWT and webhook secrets
- Set up development workflow for frontend server
- Configured deployment settings

## Project Architecture

### Frontend
- **Framework:** React 19 with TypeScript
- **Build Tool:** Vite (using Rolldown)
- **Styling:** Tailwind CSS v4
- **UI Components:** Radix UI components
- **State Management:** React Context API
- **Routing:** React Router v7

### Backend (Cloudflare-specific - not available in Replit)
- **Runtime:** Cloudflare Workers
- **State Management:** Durable Objects
- **Database:** D1 (SQLite at the edge)
- **Storage:** R2 buckets for templates, KV for sessions
- **AI Services:** Multiple LLM providers via AI Gateway
- **Containers:** Sandboxed app previews

## Current Limitations in Replit

The application frontend loads and displays, but backend API calls will fail because:
1. **No Cloudflare Workers**: The backend requires Cloudflare Workers infrastructure
2. **No D1 Database**: Uses Cloudflare's edge SQLite database
3. **No Durable Objects**: Stateful serverless objects specific to Cloudflare
4. **No Containers**: Sandboxed execution requires Cloudflare Containers
5. **No AI Gateway**: LLM routing through Cloudflare's AI Gateway

## Running the Project

### Development Server
The frontend runs on port 5000 via the `Server` workflow:
```bash
npm run dev
```

### Environment Variables
Located in `.dev.vars` (not committed to git):
- `GOOGLE_AI_STUDIO_API_KEY`: Google Gemini API key (optional for frontend-only)
- `JWT_SECRET`: Secure random string for session management
- `WEBHOOK_SECRET`: Webhook authentication secret

## File Structure
```
├── src/                    # React frontend application
│   ├── components/         # UI components
│   ├── contexts/          # React contexts (auth, theme, etc.)
│   ├── hooks/             # Custom React hooks
│   ├── lib/               # Utility libraries
│   ├── routes/            # Route components
│   └── utils/             # Helper functions
├── worker/                # Cloudflare Workers backend
├── shared/                # Shared code between frontend/backend
├── migrations/            # Database migrations (D1)
├── public/                # Static assets
├── vite.config.ts         # Vite configuration
├── wrangler.jsonc         # Cloudflare Workers config
└── package.json           # Dependencies
```

## User Preferences
None recorded yet.

## Full Deployment
For full functionality, this application should be deployed to Cloudflare Workers:
1. Follow the README instructions for Cloudflare deployment
2. Use the "Deploy to Cloudflare" button or `bun run deploy`
3. Requires Cloudflare Workers Paid Plan and Workers for Platforms subscription

## Notes
- This is primarily a **Cloudflare-specific application**
- Replit environment is suitable for **frontend development and preview only**
- Full functionality requires **Cloudflare's edge infrastructure**
- The frontend successfully demonstrates the UI and user interface design
