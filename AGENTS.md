# Repository Guidelines

## Project Structure & Module Organization
The FastAPI backend lives in `backend/app`, with helper scripts in `backend/scripts` and tests in `backend/tests`. Frontend React work happens in `frontend/src`, while Playwright specs land in `frontend/tests`. Shared automation sits in `scripts/`, and root `docker-compose*.yml` files define the local stack. Keep secrets out of git; store developer values in `.env` and production values in your secrets manager.

## Build, Test, and Development Commands
Launch the full stack with `docker compose watch`; rebuild containers when dependencies change using `docker compose build`. For backend-only work, run `uv sync` once, then `uv run fastapi dev app/main.py`. Frontend developers should `npm install` inside `frontend/`, use `npm run dev` for hot reloads, and `npm run build` before shipping. Mirror CI locally with `uv run pre-commit run --all-files`.

## Coding Style & Naming Conventions
Python code uses 4-space indentation, type hints, and snake_case for modules and functions. Ruff and `ruff-format` enforce linting and formatting, while `mypy` runs in strict mode—treat warnings as failures. Frontend TypeScript relies on Biome; keep components PascalCase, hooks camelCase, and co-locate JSX with its feature directory. Regenerate API clients with `npm run generate-client` when backend schemas shift.

## Testing Guidelines
Backend coverage is executed through `backend/scripts/test.sh`, which calls `coverage run -m pytest` and renders `htmlcov/index.html`. Use `uv run pytest -k pattern` for targeted runs, and avoid commits without a clean coverage report. Frontend UI flows are validated with `npx playwright test`; keep fixtures in `frontend/tests` and refresh snapshots locally before review. Within containers, `docker compose exec backend bash scripts/tests-start.sh` reuses the running stack.

## Commit & Pull Request Guidelines
History favors concise, emoji-prefixed subjects (e.g., `🛠 Refine auth service`) with imperative verbs. Reference issues using `(#123)` where applicable and keep bodies focused on rationale and follow-up actions. Pull requests should outline scope, testing commands or UI screenshots, and any configuration steps reviewers must repeat. Request review once CI and pre-commit pass, and avoid bundling unrelated refactors with feature or bugfix changes.

## Configuration & Security Tips
Review `SECURITY.md` before handling vulnerabilities. Rotate secrets stored in `.env` alongside infrastructure updates and prefer Docker or CI environment variables over committed files. Copy `.env.example` when bootstrapping and keep sensitive values out of version control.
