# Specs Playbook

A modular AI-assisted project specification system for reusable engineering standards, architecture rules, and project-specific overrides.

## Why This Exists

AI coding agents work better when they receive focused, structured context.
Instead of one giant rule file, this repo stores small reusable spec files that can be selected per project.

## How It Works

1. Choose a project type.
2. Select relevant stack/architecture options.
3. Generate or manually create a `/specs` folder in your project.
4. Point your AI coding agent at the project context file.

## Core Mental Model

```
Global playbook = how I generally build software.
Project context = how this specific project should be built.
```

```
Knowledge files + Decision files + Manifests + Templates = Generated project specs
```

## Folder Structure

```
specs-playbook/
  core/           General AI coding rules, naming, and documentation standards
  frontend/       Frontend stack-specific rules (Vite, Next.js, MUI, etc.)
  backend/        Backend patterns (Express, API responses, errors, auth)
  database/       Database-specific guidance (Postgres, MySQL, MongoDB)
  architecture/   Architecture patterns (MVP, monorepo, microservices, plugins)
  project-types/  Project-type profiles (SaaS, portfolio, AI tool, marketplace)
  decisions/      How to choose between options
  manifests/      Machine-readable project type definitions
  schema/         JSON schema for manifest validation
  templates/      Reusable project context templates
  examples/       Example generated spec outputs
```

## Manual Usage

1. Open `decisions/` files to choose your stack.
2. Copy `templates/PROJECT_CONTEXT.md` into your project as `/specs/PROJECT_CONTEXT.md`.
3. Fill in your stack selections and list the playbook files that apply.
4. Add project-specific overrides in the overrides section.
5. At the start of each session, tell your AI agent to read the files listed in `PROJECT_CONTEXT.md`.

## Example

See `examples/saas-app-basic/specs/PROJECT_CONTEXT.md` for a complete filled-out example.

## V1 Status

This repo currently supports manual spec assembly.
Future versions may include a CLI or web app generator.

## Future Direction

1. CLI generator — select options via terminal, outputs a `/specs` folder
2. Web app questionnaire — guided UI that generates specs
