# Open-Source Developer Dashboard (OSDD)

> An open-source, developer-centric dashboard that helps contributors and maintainers visualize GitHub activity, manage contributions, and identify impactful open-source opportunities using the GitHub API.

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Next.js](https://img.shields.io/badge/Next.js-16-black)
![TypeScript](https://img.shields.io/badge/TypeScript-5-blue)

## Why This Project?

Open-source contribution is often fragmented across repositories, issues, and pull requests. New contributors struggle to find meaningful entry points, while maintainers lack simple tools to track contributor activity and repository health in one place. OSDD solves this by providing:

- **Unified Dashboard** - All your GitHub activity in one beautiful interface
- **Smart Discovery** - Find beginner-friendly projects matching your skills
- **Activity Insights** - Visualize your contribution patterns and streaks
- **Repository Health** - Monitor your repos with intelligent metrics

## Features

### 1. Developer Profile Overview
- Comprehensive GitHub identity display (avatar, bio, stats)
- Contribution streak tracking (last 30-90 days)
- Top programming languages aggregated from your repos
- Total stars and forks across all repositories
- Followers, following, and join date

### 2. Repository Insights Module
- List and sort repositories by stars, last updated, or open issues
- Repository health indicators based on activity
- Quick stats: stars, forks, open issues, primary language
- Direct links to repositories
- README preview support

### 3. Contribution Activity Tracker
- Visual timeline of commits, pull requests, and issues
- Filter by date range (7, 30, or 90 days)
- Event type breakdown with color-coded icons
- Activity statistics and patterns
- Real-time GitHub event streaming

### 4. Open-Source Discovery
- Recommend repositories based on your language preferences
- Filter for beginner-friendly issues ("good first issue")
- Repository quality indicators (stars, forks, activity)
- Direct links to contribution opportunities
- Smart language matching from your profile

## Tech Stack

- **Framework**: Next.js 16 (App Router)
- **Language**: TypeScript
- **Styling**: Tailwind CSS v4
- **UI Components**: shadcn/ui
- **API**: GitHub REST API v3
- **Authentication**: GitHub OAuth
- **State Management**: React Server Components + Client Components
- **Deployment**: Vercel (optimized)

## Architecture Decisions

### Why Next.js App Router?
- Server-side rendering for faster initial loads
- React Server Components for efficient data fetching
- Built-in API routes for GitHub OAuth flow
- Optimized for Vercel deployment

### Why No Database?
- Direct GitHub API integration keeps data always fresh
- No sync issues or stale data
- Reduced infrastructure complexity
- Privacy-first: no data storage beyond session cookies

### Why TypeScript?
- Type safety for GitHub API responses
- Better developer experience with IntelliSense
- Catches errors at compile time
- Self-documenting code

## Getting Started

### Prerequisites

- Node.js 18+ installed
- A GitHub account
- GitHub OAuth App credentials

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/open-source-dev-dashboard.git
   cd open-source-dev-dashboard
   ```

2. **Install dependencies**
   ```bash
   npm install
   # or
   yarn install
   # or
   pnpm install
   ```

3. **Set up GitHub OAuth App**
   
   Create a new OAuth App at [github.com/settings/developers](https://github.com/settings/developers):
   
   - **Application name**: Open-Source Developer Dashboard
   - **Homepage URL**: `http://localhost:3000`
   - **Authorization callback URL**: `http://localhost:3000/api/auth/callback`
   
   After creating, copy your Client ID and Client Secret.

4. **Configure environment variables**
   
   Copy `.env.example` to `.env.local`:
   ```bash
   cp .env.example .env.local
   ```
   
   Edit `.env.local` and add your credentials:
   ```env
   GITHUB_CLIENT_ID=your_github_client_id_here
   GITHUB_CLIENT_SECRET=your_github_client_secret_here
   NEXT_PUBLIC_DEV_GITHUB_REDIRECT_URL=http://localhost:3000/api/auth/callback
   ```

5. **Run the development server**
   ```bash
   npm run dev
   ```

6. **Open your browser**
   
   Navigate to [http://localhost:3000](http://localhost:3000)

## Deployment

### Deploy to Vercel (Recommended)

1. Push your code to GitHub
2. Import your repository on [vercel.com](https://vercel.com)
3. Add environment variables in Vercel dashboard:
   - `GITHUB_CLIENT_ID`
   - `GITHUB_CLIENT_SECRET`
4. Update your GitHub OAuth App callback URL to your Vercel domain
5. Deploy!

### Environment Variables for Production

```env
GITHUB_CLIENT_ID=your_production_client_id
GITHUB_CLIENT_SECRET=your_production_client_secret
```

The app automatically uses `NEXT_PUBLIC_VERCEL_URL` for production redirects.

## Project Structure

```
open-source-dev-dashboard/
├── app/
│   ├── api/
│   │   ├── auth/           # GitHub OAuth flow
│   │   └── discover/       # Repository discovery API
│   ├── dashboard/          # Main dashboard page
│   ├── layout.tsx          # Root layout with metadata
│   ├── page.tsx            # Landing page
│   └── globals.css         # Global styles and theme
├── components/
│   ├── dashboard/          # Dashboard-specific components
│   │   ├── profile-overview.tsx
│   │   ├── repo-insights.tsx
│   │   ├── activity-tracker.tsx
│   │   ├── discovery-section.tsx
│   │   └── dashboard-shell.tsx
│   ├── ui/                 # Reusable UI components (shadcn)
│   └── theme-provider.tsx  # Dark mode support
├── lib/
│   ├── github.ts           # GitHub API client
│   └── utils.ts            # Utility functions
├── README.md
├── CONTRIBUTING.md
├── LICENSE
├── .env.example
└── package.json
```

## API Rate Limits

The GitHub API has rate limits:
- **Authenticated requests**: 5,000 requests per hour
- **Unauthenticated requests**: 60 requests per hour

This app uses authenticated requests via OAuth, giving you plenty of room for normal usage. Rate limit information is handled gracefully with error messages.

## Security

- **HTTP-only cookies**: Tokens stored securely, not accessible via JavaScript
- **Minimal OAuth scopes**: Only requests `read:user`, `repo`, and `read:org` permissions
- **No data persistence**: No database means no data breach risk
- **Environment variables**: Sensitive credentials never committed to code
- **HTTPS in production**: Enforced by Vercel

## Contributing

We welcome contributions! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for details on:
- How to set up the development environment
- Code style guidelines
- How to submit pull requests
- Finding good first issues

## Roadmap

- [ ] Dark mode toggle (currently defaults to dark)
- [ ] GitHub GraphQL API integration for better performance
- [ ] Contribution calendar heatmap
- [ ] Repository health scoring algorithm
- [ ] Export dashboard as PDF/image
- [ ] Team/organization dashboard views
- [ ] CI/CD pipeline with GitHub Actions
- [ ] Unit and integration tests
- [ ] Accessibility improvements (WCAG 2.1 AA compliance)

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- MLH Fellowship application inspired
- Inspired by the open-source community
- UI components from [shadcn/ui](https://ui.shadcn.com/)
- Powered by the [GitHub REST API](https://docs.github.com/en/rest)

## Contact

- **GitHub**: [@yourusername](https://github.com/yourusername)
- **Issues**: [github.com/yourusername/open-source-dev-dashboard/issues](https://github.com/yourusername/open-source-dev-dashboard/issues)

---

Built with ❤️ for the open-source community