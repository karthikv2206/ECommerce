\# AIDLC Guardrails

## Purpose

This steering file instructs Kiro to consult the AI Development Lifecycle (AIDLC) configuration and its linked knowledge sources before responding to queries about this project.

## AIDLC Configuration

Before answering questions or making changes, read the AIDLC manifest at:

#[[file:.kiro/aidlc/guardrails.yaml]]

This YAML defines the project's knowledge sources, including repository links and file inclusion patterns.

## Workflow

1. **Read the AIDLC YAML** located at `.kiro/aidlc/guardrails.yaml` to identify configured knowledge sources, their repository locations, branches, and file inclusion patterns.

2. **Follow the repository link** specified in `knowledgeSources[].location` (currently https://github.com/UST-PACE/pace-lumen-api-v2.git, branch: `main`) and fetch the markdown files matching the `include` glob patterns (`**/*.md`).

3. **Understand the markdown guardrails** found in that repository. These markdown files define project conventions, architectural decisions, coding standards, and operational guidelines that must be respected.

4. **Respond based on that understanding.** When answering questions, generating code, or making recommendations, ensure your output aligns with the guardrails and standards documented in those markdown files.

## Key Behaviors

- When the user asks about project standards, conventions, or architecture, reference the markdown guardrails from the linked repository.
- When generating or modifying code, ensure compliance with the documented guidelines.
- If a guardrail conflicts with a user request, surface the conflict and ask for clarification before proceeding.
- Treat the AIDLC-linked markdown files as authoritative project documentation alongside the existing steering files.

## Context File Integration

This steering works alongside the existing context files (repo context files, project context files, and user-uploaded context files). The AIDLC knowledge sources provide an additional layer of authoritative project guidance that should be consulted together with those context files.

