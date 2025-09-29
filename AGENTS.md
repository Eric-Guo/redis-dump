# Repository Guidelines

## Project Structure & Module Organization
- `lib/redis/dump.rb` houses the dump/load engine; keep new logic modular there.
- CLI wrappers in `exe/` (`redis-dump`, `redis-load`, `redis-report`) should delegate and stay flag-parsing only.
- Developer scripts in `bin/` (`bin/setup`, `bin/console`) bootstrap environments; type stubs sit under `sig/`.
- Acceptance tryouts and fixtures live in `try/`; CI in `.github/workflows/main.yml` mirrors this layout.

## Build, Test, and Development Commands
- `bin/setup` installs dependencies via Bundler after cloning or touching the gemspec.
- `bundle exec rake` runs the default RuboCop task; resolve offenses before raising a PR.
- `redis-server try/redis.conf` starts the local redis instance expected by the tryouts and CI matrix.
- `bundle exec try -v try/*_try.rb` executes the full tryout suite; narrow the glob while iterating locally.
- `gem build redis-dump.gemspec` produces a release artifact in `pkg/` for smoke-testing the executables.

## Coding Style & Naming Conventions
- Use Ruby two-space indentation, snake_case for methods, and CamelCase modules under the `Redis` namespace.
- Keep CLI entrypoints dash-named and route new flags through explicit methods inside `Redis::Dump`.
- Let RuboCop drive formatting (`bundle exec rubocop`); include cops suppressions sparingly and comment the rationale.
- Store sample data as deterministic JSON in `try/`; never commit sensitive dumps from real systems.

## Testing Guidelines
- Name tryouts `NN_description_try.rb` to preserve order and keep scenarios readable.
- Start redis with the repo config before testing, and flush targeted DBs in teardown to prevent coupling.
- Check in any new fixtures alongside their tryout and document expectations inline for future editors.
- Note redis version requirements in comments when covering edge behaviour introduced by specific releases.

## Commit & Pull Request Guidelines
- Follow the history’s short, imperative subjects (e.g., `[#39] Fix executable path`) and mention linked issues.
- In PR descriptions, summarise scope, list lint and tryout results, and flag configuration or CI updates.
- Attach CLI transcripts or reproduction steps for behavioural fixes so reviewers can replay quickly.
- For releases, bump `Redis::Dump::VERSION`, refresh `CHANGES.txt`, and mention the built gem artifact.
