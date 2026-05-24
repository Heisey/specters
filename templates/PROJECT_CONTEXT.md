# Project Context

## Project Summary
{{project_summary}}

## Project Type
{{project_type}}

## Selected Architecture
{{architecture}}

## Selected Stack
- Frontend: {{frontend}}
- UI: {{ui}}
- Backend: {{backend}}
- Database: {{database}}
- Infrastructure: {{infrastructure}}

## Included Spec Files
- {{file_list}}

## Project Rules
{{project_rules}}

## Project Overrides
{{overrides}}

## Folder Structure
{{folder_structure}}

## Agent Instructions
- Read every file listed in **Included Spec Files** before generating or modifying code.
- Treat this file as the project-specific source of truth.
- Follow all rules from included spec files unless explicitly overridden here.
- Project overrides take priority over general playbook rules.
- More specific rules override more general rules.
- Do not change the folder structure unless an override explicitly allows it.
- Do not bypass `packages/` for shared logic.
- Do not introduce new architecture patterns unless defined in `specs/`.
- Store important architectural decisions in `specs/`, not only in chat history.
- When rules conflict and no override resolves the conflict, ask for clarification before proceeding.

## Open Questions
{{open_questions}}