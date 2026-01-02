# Contributing to Open-Source Developer Dashboard

Thank you for your interest in contributing to OSDD! This document provides guidelines and instructions for contributing.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Development Workflow](#development-workflow)
- [Code Style Guidelines](#code-style-guidelines)
- [Submitting Changes](#submitting-changes)
- [Finding Issues to Work On](#finding-issues-to-work-on)

## Code of Conduct

We are committed to providing a welcoming and inclusive environment. Please:

- Be respectful and considerate in all interactions
- Welcome newcomers and help them learn
- Focus on what is best for the community
- Show empathy towards other community members

## Getting Started

### Prerequisites

- Node.js 18 or higher
- npm, yarn, or pnpm
- Git
- A GitHub account
- GitHub OAuth credentials (see README.md)

### Setup Development Environment

1. **Fork the repository** on GitHub

2. **Clone your fork**
   ```bash
   git clone https://github.com/YOUR-USERNAME/open-source-dev-dashboard.git
   cd open-source-dev-dashboard
   ```

3. **Add upstream remote**
   ```bash
   git remote add upstream https://github.com/ORIGINAL-OWNER/open-source-dev-dashboard.git
   ```

4. **Install dependencies**
   ```bash
   npm install
   ```

5. **Set up environment variables**
   ```bash
   cp .env.example .env.local
   ```
   Add your GitHub OAuth credentials to `.env.local`

6. **Run the development server**
   ```bash
   npm run dev
   ```

7. **Open http://localhost:3000**

## Development Workflow

### Creating a Branch

Always create a new branch for your work:

```bash
git checkout -b feature/your-feature-name
# or
git checkout -b fix/bug-description
```

Branch naming conventions:
- `feature/` - New features
- `fix/` - Bug fixes
- `docs/` - Documentation changes
- `refactor/` - Code refactoring
- `test/` - Adding or updating tests

### Making Changes

1. Make your changes in your branch
2. Test your changes thoroughly
3. Commit your changes with clear messages
4. Push to your fork
5. Open a pull request

### Keeping Your Fork Updated

```bash
git checkout main
git fetch upstream
git merge upstream/main
git push origin main
```

## Code Style Guidelines

### TypeScript

- Use TypeScript for all new code
- Define proper types and interfaces
- Avoid `any` type unless absolutely necessary
- Use functional components with hooks

### React Components

- Use functional components over class components
- Prefer React Server Components when possible
- Client components should use `"use client"` directive
- Keep components small and focused

### File Structure

- Components: PascalCase (`ProfileOverview.tsx`)
- Utilities: kebab-case (`github-client.ts`)
- One component per file
- Co-locate related files

### Naming Conventions

```typescript
// Components
export function ProfileOverview() { }

// Hooks
function useGitHubData() { }

// Types/Interfaces
interface GitHubUser { }
type RepoStatus = "active" | "archived"

// Constants
const API_BASE_URL = "https://api.github.com"
```

### Code Formatting

We use Prettier for code formatting. Before committing:

```bash
npm run format
```

### Linting

We use ESLint for code quality:

```bash
npm run lint
```

## Submitting Changes

### Before Submitting

- [ ] Code follows the style guidelines
- [ ] Changes have been tested locally
- [ ] No console errors or warnings
- [ ] All existing tests pass
- [ ] New features include appropriate tests
- [ ] Documentation has been updated
- [ ] Commit messages are clear and descriptive

### Pull Request Process

1. **Update your branch** with the latest main
   ```bash
   git fetch upstream
   git rebase upstream/main
   ```

2. **Push your changes**
   ```bash
   git push origin your-branch-name
   ```

3. **Create a Pull Request** on GitHub

4. **Fill out the PR template** completely:
   - Describe what changes you made
   - Reference any related issues
   - Add screenshots for UI changes
   - Note any breaking changes

5. **Wait for review**
   - Address any feedback from reviewers
   - Make requested changes in new commits
   - Push updates to the same branch

6. **After approval**, a maintainer will merge your PR

### Commit Message Guidelines

Write clear, concise commit messages:

```
type(scope): brief description

Longer explanation if needed. Wrap at 72 characters.

Fixes #123
```

Types:
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting)
- `refactor`: Code refactoring
- `test`: Adding or updating tests
- `chore`: Maintenance tasks

Examples:
```
feat(dashboard): add contribution calendar view
fix(auth): resolve token expiration handling
docs(readme): update installation instructions
```

## Finding Issues to Work On

### Good First Issues

Look for issues labeled `good first issue` - these are great for newcomers:

[View Good First Issues](https://github.com/ORIGINAL-OWNER/open-source-dev-dashboard/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22)

### Help Wanted

Issues labeled `help wanted` are actively seeking contributors:

[View Help Wanted Issues](https://github.com/ORIGINAL-OWNER/open-source-dev-dashboard/issues?q=is%3Aissue+is%3Aopen+label%3A%22help+wanted%22)

### Issue Labels

- `bug` - Something isn't working
- `enhancement` - New feature or request
- `documentation` - Documentation improvements
- `good first issue` - Good for newcomers
- `help wanted` - Extra attention needed
- `question` - Further information requested

## Testing

### Running Tests

```bash
npm test
```

### Writing Tests

- Write unit tests for utility functions
- Write integration tests for API routes
- Write component tests for UI components
- Aim for meaningful test coverage

## Questions?

- Open an issue with the `question` label
- Reach out to maintainers
- Check existing issues and PRs for similar questions

## Recognition

Contributors will be:
- Listed in the project README
- Credited in release notes
- Appreciated by the community!

Thank you for contributing to OSDD! 🎉