# Architecture Documentation

## System Overview

The Open-Source Developer Dashboard (OSDD) is a Next.js 16 application that leverages React Server Components (RSC) and the App Router to create a performant, scalable dashboard for visualizing GitHub activity.

## Architecture Principles

1. **Server-First Rendering** - Use RSC by default, client components only when necessary
2. **Direct API Integration** - No database layer, GitHub API as source of truth
3. **Type Safety** - TypeScript throughout for reliability
4. **Minimal Client JavaScript** - Ship less JavaScript to the browser
5. **Progressive Enhancement** - Core functionality works without JavaScript

## Technology Stack

### Frontend
- **Next.js 16** - React framework with App Router
- **React 19** - UI library with Server Components
- **TypeScript 5** - Type safety and developer experience
- **Tailwind CSS v4** - Utility-first styling
- **shadcn/ui** - Accessible component library

### API Integration
- **GitHub REST API v3** - Primary data source
- **OAuth 2.0** - Secure authentication flow
- **HTTP-only cookies** - Secure token storage

### Deployment
- **Vercel** - Optimized hosting platform
- **Edge Functions** - Low-latency API responses

## Directory Structure

```
app/
├── api/                      # API routes
│   ├── auth/                 # Authentication endpoints
│   │   ├── github/           # OAuth initiation
│   │   ├── callback/         # OAuth callback handler
│   │   └── logout/           # Session termination
│   └── discover/             # Repository discovery
├── dashboard/                # Protected dashboard pages
│   └── page.tsx              # Main dashboard (RSC)
├── layout.tsx                # Root layout with metadata
├── page.tsx                  # Landing page (RSC)
└── globals.css               # Global styles and theme tokens

components/
├── dashboard/                # Dashboard-specific components
│   ├── dashboard-shell.tsx   # Layout wrapper (Client)
│   ├── profile-overview.tsx  # User profile section (Client)
│   ├── repo-insights.tsx     # Repository analytics (Client)
│   ├── activity-tracker.tsx  # Contribution timeline (Client)
│   └── discovery-section.tsx # Project discovery (Client)
├── ui/                       # Reusable UI primitives
└── theme-provider.tsx        # Dark mode provider (Client)

lib/
├── github.ts                 # GitHub API client
└── utils.ts                  # Utility functions
```

## Component Architecture

### Server Components (RSC)
Used for:
- Initial page loads (`app/page.tsx`, `app/dashboard/page.tsx`)
- Data fetching from GitHub API
- SEO-critical content
- Static content

Benefits:
- Zero JavaScript bundle for these components
- Direct backend access (cookies, env vars)
- Faster initial page loads
- Better SEO

### Client Components
Used for:
- Interactive UI (sorting, filtering, tabs)
- State management (useState, useEffect)
- Event handlers (onClick, onChange)
- Browser APIs (localStorage, window)

Marked with `"use client"` directive.

## Data Flow

### Authentication Flow
```
1. User clicks "Continue with GitHub"
   ↓
2. Redirects to GitHub OAuth (/api/auth/github)
   ↓
3. User authorizes on GitHub
   ↓
4. GitHub redirects back (/api/auth/callback?code=...)
   ↓
5. Exchange code for access token
   ↓
6. Store token in HTTP-only cookie
   ↓
7. Redirect to dashboard
```

### Dashboard Data Flow
```
1. User visits /dashboard
   ↓
2. Server Component reads cookie
   ↓
3. Parallel fetch:
   - User profile
   - Repositories
   - Recent events
   ↓
4. Render initial HTML with data
   ↓
5. Client components hydrate
   ↓
6. Interactive features become active
```

## GitHub API Client

Location: `lib/github.ts`

### Design Decisions

**Class-based client**
- Encapsulates API logic
- Reusable across routes
- Centralized error handling
- Type-safe responses

**Error handling**
- Custom `GitHubAPIError` class
- Rate limit detection
- Graceful degradation
- User-friendly messages

**No caching layer**
- Always fresh data from GitHub
- Simpler architecture
- No sync issues
- Lower maintenance

### Rate Limiting

GitHub API limits:
- **5,000 requests/hour** (authenticated)
- Tracked via response headers
- Detected and surfaced to users

Mitigation strategies:
- Fetch only what's needed
- Parallel requests where possible
- Conditional rendering

## State Management

### Server State
- GitHub data fetched in RSC
- Passed as props to client components
- No client-side refetching

### Client State
- `useState` for UI state (filters, sorts)
- No global state library needed
- Keeps bundle size small

### URL State
- Sort/filter preferences could be in URL
- Future enhancement for shareable links

## Security

### Authentication
- **OAuth 2.0** standard flow
- **HTTP-only cookies** prevent XSS
- **Secure flag** in production
- **SameSite=Lax** prevents CSRF

### API Security
- Token never exposed to client
- All API calls from server
- Environment variables for secrets
- Minimal OAuth scopes

### Best Practices
- No secrets in code
- Input validation
- Error messages don't leak info
- HTTPS enforced in production

## Performance Optimizations

### Initial Load
- Server-side rendering
- Parallel data fetching
- Optimized images (Next.js Image)
- Font optimization (next/font)

### Bundle Size
- Server Components reduce JS
- Tree shaking unused code
- Code splitting by route
- Lazy loading components

### Caching
- Static assets cached by CDN
- API responses could use SWR
- No database to cache

## Scalability

### Current Architecture
- Stateless API routes
- No database = no bottleneck
- GitHub API handles load
- Vercel auto-scales

### Future Considerations
- Add caching layer (Redis) if needed
- Queue background jobs for analytics
- GraphQL for more efficient queries
- Pagination for large datasets

## Testing Strategy

### Unit Tests
- Utility functions (`lib/utils.ts`)
- API client methods (`lib/github.ts`)
- Pure component logic

### Integration Tests
- API routes
- OAuth flow
- Error handling

### E2E Tests
- User flows (login, view dashboard)
- Critical paths
- Cross-browser testing

### Testing Stack (Future)
- **Jest** - Unit tests
- **React Testing Library** - Component tests
- **Playwright** - E2E tests

## Deployment

### Vercel Configuration
- Automatic deployments from `main`
- Preview deployments for PRs
- Environment variables in dashboard
- Edge Functions for API routes

### Environment Variables
Required:
- `GITHUB_CLIENT_ID`
- `GITHUB_CLIENT_SECRET`

Optional:
- `NEXT_PUBLIC_DEV_GITHUB_REDIRECT_URL`

### Build Process
```
1. Install dependencies (npm install)
2. Type check (tsc)
3. Lint (eslint)
4. Build (next build)
5. Deploy to Vercel
```

## Monitoring

### Current
- Vercel Analytics (built-in)
- Error logs in Vercel dashboard
- GitHub API rate limit headers

### Future Enhancements
- Error tracking (Sentry)
- Performance monitoring (Web Vitals)
- User analytics
- API usage dashboards

## Accessibility

### Current Implementation
- Semantic HTML elements
- ARIA labels on icons
- Keyboard navigation support
- Color contrast compliance

### Future Improvements
- WCAG 2.1 AA full compliance
- Screen reader testing
- Focus management
- Reduced motion support

## Future Architecture Considerations

### GraphQL Migration
- GitHub GraphQL API more efficient
- Single query for complex data
- Reduces API calls
- Better performance

### Caching Layer
- Redis for frequently accessed data
- Reduce GitHub API calls
- Faster response times
- Better rate limit management

### Real-time Updates
- WebSocket connection to GitHub
- Live activity feed
- Notifications
- Collaborative features

### Database Integration
- Store user preferences
- Analytics tracking
- Custom dashboards
- Team features

## Contributing to Architecture

When proposing architectural changes:

1. **Document the problem** clearly
2. **Consider alternatives** (pros/cons)
3. **Measure impact** (performance, complexity)
4. **Create POC** if significant
5. **Update documentation** after implementation

## Questions?

For architecture questions or proposals:
- Open a GitHub Discussion
- Tag with `architecture` label
- Provide context and rationale