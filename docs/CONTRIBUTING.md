# Contributing

## Getting Started
1. Fork the repository.
2. Clone your fork locally.
3. Create a branch for your change.
4. Install dependencies for the backend and frontend.
5. Run the relevant tests before opening a pull request.

## Development Workflow
### Backend
```bash
cd backend
uv sync
uv run pytest
```

### Frontend
```bash
cd frontend
bun install
bun run build
```

## Code Style
- Backend: Ruff and mypy are used.
- Frontend: Biome and TypeScript are used.
- Prefer small, focused pull requests.

## Testing
- Backend tests live under `backend/tests`.
- Frontend end-to-end tests live under `frontend/tests`.
- Run Playwright locally when changing UI behavior.

## Pull Request Checklist
- Describe the change clearly.
- Include screenshots for UI changes when helpful.
- Mention any configuration or environment changes.
- Ensure tests pass and documentation stays current.

## Reporting Issues
Use GitHub Issues to report bugs, enhancements, or deployment problems. Include:
- steps to reproduce,
- expected behavior,
- actual behavior,
- and any relevant logs.
