# CLAUDE.md — TinyPay Codebase Guide

This file provides context for AI assistants working in this repository.

## Project Overview

**TinyPay** is a Ruby on Rails 5.0 web application designed as a lightweight paywall/payment processor. The goal is a simple, web-based payment flow supporting PayPal, Apple Pay, and Android Pay, with a 2-click authentication concept and user tracking via cookies/IP/user agent.

The application is in early scaffold phase — no custom models, controllers, or routes are implemented yet. It's ready for active feature development.

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Language | Ruby (Rails 5.0.2) |
| Database | SQLite3 (all environments) |
| Web server | Puma 3.x |
| Frontend | ERB templates, jQuery, CoffeeScript, SCSS, Turbolinks 5 |
| Real-time | Action Cable (async in dev/test, Redis in production) |
| Background jobs | Active Job (inline adapter in development) |
| Testing | Rails TestUnit / Minitest |

## Directory Structure

```
tinypay/
├── app/
│   ├── assets/           # JS (CoffeeScript), CSS (SCSS), images
│   ├── channels/         # Action Cable WebSocket channels
│   ├── controllers/      # HTTP request handlers (inherit ApplicationController)
│   ├── helpers/          # View helper modules
│   ├── jobs/             # Background jobs (inherit ApplicationJob)
│   ├── mailers/          # Email classes (inherit ApplicationMailer)
│   ├── models/           # ActiveRecord models (inherit ApplicationRecord)
│   └── views/            # ERB templates + layouts
├── config/
│   ├── environments/     # Per-environment overrides (development/test/production)
│   ├── initializers/     # Boot-time setup scripts
│   ├── database.yml      # SQLite3 config for all environments
│   ├── routes.rb         # URL routing (currently empty)
│   └── secrets.yml       # Secret key config
├── db/
│   ├── migrate/          # Database migrations (none yet)
│   └── seeds.rb          # Seed data script
├── design/
│   └── tinypay.sketch    # UI design file
├── meeting_notes/        # Project planning notes
├── test/
│   ├── controllers/      # Controller tests
│   ├── fixtures/         # YAML fixture files
│   ├── integration/      # Integration tests
│   ├── models/           # Model unit tests
│   └── test_helper.rb    # Global test configuration
├── Gemfile               # Gem dependencies
└── README.md             # (placeholder — not filled in)
```

## Development Setup

```bash
bin/setup                 # Install gems, create and migrate database
bin/rails server          # Start dev server at http://localhost:3000
bin/rails console         # Interactive Rails console
```

## Common Commands

```bash
# Testing
bin/rails test                   # Run all tests
bin/rails test test/models/      # Run model tests only

# Database
bin/rails db:migrate             # Run pending migrations
bin/rails db:rollback            # Revert last migration
bin/rails db:seed                # Load db/seeds.rb
bin/rails db:reset               # Drop, create, migrate, seed

# Code generation
bin/rails generate model User name:string email:string
bin/rails generate controller Users index show
bin/rails generate migration AddEmailToUsers email:string
bin/rails destroy model User     # Remove generated files

# Maintenance
bin/rails log:clear              # Clear log files
bin/rails tmp:clear              # Clear tmp directory
bin/rails restart                # Touch restart.txt
```

## Rails Conventions to Follow

- **Controllers**: Plural snake_case filenames (e.g., `payments_controller.rb`), inherit from `ApplicationController`
- **Models**: Singular CamelCase class names (e.g., `Payment`), inherit from `ApplicationRecord`
- **Views**: Organized under `app/views/<plural_resource>/`, use `.html.erb` extension
- **Routes**: RESTful resources preferred; define in `config/routes.rb`
- **Migrations**: Use `rails generate migration` — never edit existing migrations
- **Tests**: Mirror the `app/` structure under `test/`; use fixtures for test data
- **Assets**: JavaScript in `app/assets/javascripts/` (CoffeeScript), stylesheets in `app/assets/stylesheets/` (SCSS)

## Security Defaults (Do Not Disable)

- CSRF protection is enabled globally via `protect_from_forgery with: :exception`
- Per-form CSRF tokens enabled
- Parameters are filtered in logs (`config/initializers/filter_parameter_logging.rb`)
- Production uses `ENV["SECRET_KEY_BASE"]` — never hardcode secrets

## Environment Variables

| Variable | Purpose | Default |
|----------|---------|---------|
| `RAILS_ENV` | Application environment | `development` |
| `PORT` | Web server port | `3000` |
| `RAILS_MAX_THREADS` | Puma thread count | `5` |
| `SECRET_KEY_BASE` | Encryption key (production only) | — |
| `RAILS_SERVE_STATIC_FILES` | Serve static assets from Rails | — |

Sensitive values go in `.env` (gitignored). No dotenv gem is configured yet — add `dotenv-rails` if needed.

## Action Cable

- Development/Test: async adapter (no external dependency)
- Production: Redis at `redis://localhost:6379/1` — requires a running Redis instance

## Testing Guidelines

- All tests live under `test/`
- Run the full suite before committing: `bin/rails test`
- Use fixtures (not factories) for test data: `test/fixtures/`
- Each model/controller should have a corresponding `_test.rb` file
- No CI is configured yet — run tests locally

## What's Not Yet Implemented

The application is a fresh scaffold. These features are planned (from meeting notes):
- Payment flows (PayPal, Apple Pay, Android Pay)
- 2-click authentication
- User tracking (cookies, IP address, user agent)
- Paywall / content gating
- Custom models, controllers, and routes

## Git Workflow

Development branch: `claude/add-claude-documentation-iDNOo`

```bash
git push -u origin claude/add-claude-documentation-iDNOo
```
