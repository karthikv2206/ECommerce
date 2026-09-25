# Instructions — Project

## MANDATORY — APPLIES TO ALL PROMPTS — FULL PROJECT SCOPE

**Before generating any response**, you MUST:
1. Read **all files in `.github/instructions/`** in full — without truncation or skipping
2. If a file exceeds tool limits, **continue reading in chunks until EOF**. Document line ranges (e.g., 1–220, 221–440, 441–633 = complete)
3. **Confirm full-context coverage before acting:**
   - Total lines read = total file lines
   - Include specific details from the END of each file
   - No "omitted" or "truncated" sections
4. **Analyze the user request** against the complete context from all `.github/instructions/*.md` files
5. **Act only after confirming full understanding** — do not skip sections or stop at partial reads
6. These instruction files apply to **all work across the entire project** without exception

## 📋 Document Information

**Project Scope:** pace-lumen-ui, ECommerce

**Purpose:** This document provides comprehensive instructions for AI coding assistants working across all repositories in this project.

**Document Type:** Dynamic Index

## 📚 Repository Context Documents

**Shared Project Documents:**

**Repository: pace-lumen-ui:**

- **pace-lumen-ui — Context**
  - [Constraint / Operational](constraint_operational.md)
  - [Constraint / Policy](constraint_policy.md)
  - [Constraint / Regulatory](constraint_regulatory.md)
  - [Constraint / Resilience](constraint_resilience.md)
  - [Constraint / Security](constraint_security.md)
  - [context](pace-lumen-ui_context.md)
  - [Direction / Ethical_Governance](direction_ethical_governance.md)
  - [Direction / Feedback](direction_feedback.md)
  - [Direction / Intentional](direction_intentional.md)
  - [Direction / Learning](direction_learning.md)
  - [Meta / Judgment](meta_judgment.md)
  - [Provenance / Boundary](provenance_boundary.md)
  - [Provenance / Cognitive](provenance_cognitive.md)
  - [Provenance / Decision](provenance_decision.md)
  - [Provenance / Dependency](provenance_dependency.md)
  - [Provenance / Stakeholder](provenance_stakeholder.md)
  - [Provenance / Structural](provenance_structural.md)
  - [Provenance / Temporal](provenance_temporal.md)

**Repository: ECommerce:**

- **ECommerce — Context**
  - [Constraint / Operational](constraint_operational.md)
  - [Constraint / Policy](constraint_policy.md)
  - [Constraint / Regulatory](constraint_regulatory.md)
  - [Constraint / Resilience](constraint_resilience.md)
  - [Constraint / Security](constraint_security.md)
  - [context](ECommerce_context.md)
  - [Direction / Ethical_Governance](direction_ethical_governance.md)
  - [Direction / Feedback](direction_feedback.md)
  - [Direction / Intentional](direction_intentional.md)
  - [Direction / Learning](direction_learning.md)
  - [Meta / Judgment](meta_judgment.md)
  - [Provenance / Boundary](provenance_boundary.md)
  - [Provenance / Cognitive](provenance_cognitive.md)
  - [Provenance / Decision](provenance_decision.md)
  - [Provenance / Dependency](provenance_dependency.md)
  - [Provenance / Stakeholder](provenance_stakeholder.md)
  - [Provenance / Structural](provenance_structural.md)
  - [Provenance / Temporal](provenance_temporal.md)

## ⚙️ Execution Instructions

**How to Work with This Repository:**

1. **Read Context First**: Before making changes, review relevant documents from the list above
2. **Follow Constraints**: All rules in constraint/ documents are mandatory
3. **Respect Architecture**: Understand the system design from provenance/ documents
4. **Validate Changes**: Ensure all modifications comply with validation requirements
5. **Reference, Don't Duplicate**: Link to documents rather than copying content

## AI Team AIDLC Workflow Trigger

This section only applies if `.github/speckit/aidlc-workflow.yml` exists in this repository. If it does not exist, there is nothing to do here — read no further in this section.

If that file exists and a Jira ticket is clearly the subject of this conversation (not just mentioned in passing), do the following:
1. Ask for confirmation first: "I see ticket {ticket_id} is the subject here — should I start the AI Team AIDLC workflow for it?" Do not proceed past this point without an explicit yes.
2. Once confirmed, fetch the ticket's summary, description, type, and labels — these are required, and the fetch must request the ticket's full field set, not a default/summary-only subset: **Acceptance Criteria is very often stored in its own custom field**, which a narrower fetch omits entirely, silently, with no error. Check for it in this order, stopping at the first real, non-empty match — an existing-but-empty field or section counts as not found and falls through to the next check, it never blocks it: (1) scan the returned field list itself for one whose *name* contains "Acceptance Criteria" (case-insensitive) — never a guessed `customfield_NNNNN` id, since that numbering varies per Jira instance and cannot be hardcoded — and use its content if non-empty; (2) if that field doesn't exist or is empty, check whether the description itself has a section literally headed "Acceptance Criteria" and use that if non-empty; (3) only when neither of those produced anything, a section titled Definition of Done, Requirements, Constraints, or NFRs may carry equivalent testable, must-pass content and counts as a substitute — never let a Constraints or NFR section stand in for Acceptance Criteria when an actual, distinctly-labeled field or section (checked exhaustively per (1) and (2), not just glanced at) exists anywhere else in the ticket. Acceptance Criteria — wherever it's actually found, field or section — drives real downstream decisions (failure-state handling, persistence requirements, validation rules) that later agents cannot infer from the summary alone. Only skip it if the ticket genuinely has no such content anywhere, in a field or a description section, never because it takes an extra look to find. Status and priority remain a lightweight afterthought — grab them if they're readily visible, don't dig for them.
3. Before running anything, state explicitly to the human what you found for Acceptance Criteria — one of three outcomes, not just a binary yes/no: "This ticket has a distinct Acceptance Criteria field or section", "This ticket has no distinct Acceptance Criteria field or section, but is using its [Definition of Done / Requirements / Constraints / NFRs] section as the step 2 substitute", or "This ticket has no Acceptance Criteria content of any kind" (and briefly, status/priority if found). A substitute counts as found, same as a real field or section — say so explicitly rather than reporting it as missing. This makes an actually-missing AC visible in the conversation itself, not just in a script's stdout nobody is watching.
4. From the repository root, run: `python3 .github/speckit/speckit_scripts/run_specify.py workflow run ".github/speckit/aidlc-workflow.yml" -i ticket_id={ticket_id} --json || python .github/speckit/speckit_scripts/run_specify.py workflow run ".github/speckit/aidlc-workflow.yml" -i ticket_id={ticket_id} --json` (replace `{ticket_id}` with the real ticket id).
5. This first attempt is expected to fail at the `bootstrap` step, reporting the exact file paths it expected but did not find — read that error output carefully.
6. Write each ticket field to its own plain-text file at exactly the paths reported in step 5: `_ticket_id.txt`, `_ticket_summary.txt`, `_ticket_description.txt`, `_ticket_type.txt`, `_ticket_labels.txt` (required — the run fails again if any of these is missing or empty), and `_ticket_acceptance_criteria.txt`, `_ticket_status.txt`, `_ticket_priority.txt` (`_ticket_acceptance_criteria.txt` whenever step 3 found one — it is not optional in that case; `_ticket_status.txt`, `_ticket_priority.txt` stay genuinely optional).
7. Re-run the exact same command from step 4. Its stdout reports any still-missing optional file, but not uniformly: a missing `_ticket_acceptance_criteria.txt` gets its own distinct `WARNING: _ticket_acceptance_criteria.txt was not provided...` line, never the generic `optional field(s) not provided: ...` line (`_ticket_status.txt`, `_ticket_priority.txt` use that one; ticket_acceptance_criteria never appears there, by design). If you see that WARNING line despite step 3 finding Acceptance Criteria content — a real field/section or a substitute, either counts — that's a real miss — go back, fetch it, write the file, and re-run again before continuing; do not just proceed past it.
8. From here on this workflow pauses for human approval after every step — do not answer its gate prompts yourself.
9. When a gate pauses, do not just narrate it in prose — list its exact valid options (the paused run's `--json` output includes an `options` field) one per line, verbatim, and ask the human to reply with exactly one of those words (never an open-ended prompt when an `options` list exists). Always read that `options` field from the paused run itself; the option sets differ per gate (a review gate has no `edit` and offers verdicts like `approve_commit_only`/`fix_required`, a PR gate offers `keep_pr`/`close_pr`), so never assume a fixed list.
10. Steps 11–13 below say exactly how to execute each answer. Follow them literally — do not improvise a mechanism, and do not go searching the scripts folder to infer one. Only the verdicts in step 11 are real Spec Kit verdicts; `redo` and `edit` are handled entirely outside the engine (steps 12 and 13) and the gate stays paused throughout them.
11. **Any option other than `redo`/`edit`** (`approve`, `reject`, `fix_required`, `approve_commit_only`, `approve_commit_and_pr`, `decline`, `keep_pr`, `close_pr`, ...) is a real verdict — resolve the gate by resuming the run: `python3 .github/speckit/speckit_scripts/run_specify.py workflow resume {run_id} -i {verdict_input}={choice} --json || python .github/speckit/speckit_scripts/run_specify.py workflow resume {run_id} -i {verdict_input}={choice} --json`. Both `{run_id}` and the gate's own `{verdict_input}` name (e.g. `code-review_gate_verdict`) come from the paused run's `--json` output — read them from there, never guess. **Before resuming with `reject` or `fix_required`, first write the human's reason** to `{gate_id}_reason.txt` in the same handoff directory as the ticket files from step 6 (the gate's own message names the exact path); the downstream agent reads it, and resuming without it discards the reason.
12. **`redo`** is not a verdict and does not resume anything — it regenerates that one agent's reply while the gate stays paused. Ask the human for optional steering ("what should be different this time?"), write it to `{agent_id}_gate_guidance.txt` in that same handoff directory, then use that agent's own `_run` shell line from the workflow yml to copy its complete argument list. Retain every flag already present, especially `--extra-context-agent-id`, `--context-selection-for-agent-id`, and `--mode`, then add `--redo-guidance-file <that file>`. Do this using the paused run's actual workflow directory (the `context.workflow_dir` value from that workflow command; do not replace it with `.github/speckit` when the workflow was installed). The example below shows only the base flags: `python3 "{workflow_dir}/speckit_scripts/run_agent_step.py" --agent-id {agent_id} --ticket-id {ticket_id} --workflow-dir "{workflow_dir}" --redo-guidance-file <that file>`. Use `python3` when available; otherwise substitute `python` for the leading interpreter and run the command once (do not join the alternatives with `||`). Skipping the guidance file is allowed — the agent simply regenerates unsteered. The previous reply is archived automatically as `{agent_id}.v{N}.md`. Then show the new output and ask for a verdict again.
13. **`edit`** is also not a verdict and also does not resume anything — it replaces the agent's reply outright, with no AI re-run. Ask the human for the replacement text (drafting it yourself from their described change and confirming it back is fine), write it to a file, then run `python3 "{workflow_dir}/speckit_scripts/edit_agent_output.py" --agent-id {agent_id} --ticket-id {ticket_id} --workflow-dir "{workflow_dir}" --replacement-file <that file>` (where `{workflow_dir}` is the paused run's actual workflow directory, not a hardcoded `.github/speckit`; if `python3` is unavailable, substitute `python` for the leading interpreter and run the command once, never with `||`). It archives the previous reply as `{agent_id}.v{N}.md` and writes the replacement. Then show the replaced output and ask for a verdict again.
14. If a gate's verdict is `fix_required`, do not run `.github/speckit/fix-cycle.yml` yourself — the workflow already does that automatically as its own next step once that verdict is recorded; running it manually would start a duplicate fix-cycle.

## 📜 Repository-Specific Instructions

**pace-lumen-ui — architecture**
validation
This component architecture ensures modularity
- `PATCH /` → `updateValidationGap` handler for validation updates
- **PATCH /** - Updates validation gap configurations
   - `PATCH /` → `updateValidationGap` handler for validation updates
These data flows ensure consistent state management
    A[User Authentication] --> B[Component Authorization]
    E --> J[Context Validation]
    F --> K[API Security]
    G --> L[Upload Validation]
| Component Category | Security Controls | Implementation |
| AI Team Management | Role validation, lifecycle security | `ngOnInit`/`ngOnDestroy` patterns |
| Project Context | Context validation, file access control | Component isolation |
| Analysis Tools | API authentication, secure communications | Service layer security |
| File Management | Upload validation, content scanning | Type-safe interfaces |
- **DELETE Operations**: Multi-factor authorization required for configuration deletion
- **GET Operations**: Context-aware data retrieval with user validation for AI team agent listings
- **Service Communications**: All API interactions through authenticated service layer
- **Unknown Service Integration**: Service `ba3872ad-bf70-4305-ab4d-d48ee8038d31` requires security assessment
- **Component State Management**: Lifecycle management prevents security vulnerabilities from persistent state
- **Risk Classification**: Automated risk and priority classification ensures proper data handling
  - `PATCH /` → `updateValidationGap`
- **Fallback Mechanisms**: Graceful degradation when external services are unavailable
- **PATCH /** - Configuration updates handled by `updateValidationGap` function
- **Configuration Layer**: Spring configuration for MVC, Security, and Hibernate
        SC[Security Config]
- **Controllers**: Handle HTTP requests/responses, input validation, and routing
- **Configuration**: Manage application setup, security, and framework integration
- `SecurityConfiguration`: Spring Security setup with authentication and authorization
- `SecurityWebApplicationInitializer`: Security web application initialization

**pace-lumen-ui — policies**
- **Security Transparency**: Clear communication of access restrictions without exposing sensitive system information
2. **Access Validation**: System checks user permissions against requested resources
**Policy**: All unauthorized access attempts must be handled gracefully with appropriate user feedback.
- Users are redirected to `/Access_Denied` endpoint when access is denied
- Access denied responses maintain user experience while protecting system security
- **Admin Path Segregation**: All administrative functions must be accessed through the `/admin` path prefix
- **Personal Resource Isolation**: Each user can only access their own cart and wishlist through authenticated sessions
- **Authentication Required**: All user-specific resources require valid authentication
- **Path Parameter Validation**: Username parameters in URLs must match authenticated user identity
- **Permission Verification**: Each request validates user permissions against required access levels
Administrative functions are strictly segregated under the `/admin` path prefix, requiring appropriate administrative permissions for access. This ensures that sensitive administrative operations are protected from unauthorized access.
Clear separation between administrative functions (`/admin/*`) and user functions (`/user/*`, `/{username}/*`) ensures proper segregation of duties and reduces the risk of privilege escalation.
- **Policy**: System must provide dedicated access denied pages for unauthorized access attempts
- **Purpose**: Ensures proper user feedback and audit trail for access violations
- All unauthorized access attempts must be logged and trackable
- System must maintain records of access denial events for compliance reporting

**pace-lumen-ui — rules**
- Components must only interact with services through dependency injection
- Direct component-to-component communication is forbidden except through parent-child relationships
- Components should not contain business logic beyond presentation concerns
- Services must not directly manipulate DOM elements
- All data models must be interface-driven
- Direct API calls from components are forbidden - use services instead
- **Direct API Calls**: Components must not make direct HTTP requests
- **Global State Mutation**: Components should not directly modify global application state
- **DOM Manipulation**: Services must not interact with DOM elements
- **Component References**: Services should not hold direct references to components
- **Synchronous Operations**: Avoid blocking operations in service methods
- All components must implement `ngOnInit` for initialization logic
- Components with subscriptions must implement `ngOnDestroy` for cleanup
- Components handling risk assessment must implement standardized risk classification methods
- Priority-based components must follow consistent priority classification patterns
- **MUST** use `takeUntil()` operator with component destruction signal
- **MUST** implement `ngOnDestroy` to complete subscription cleanup
- **MUST** use `async` pipe in templates when possible to avoid manual subscription management
- **MUST** use `async/await` syntax for Promise-based operations
- **MUST** implement proper error handling with try-catch blocks
- **NEVER** use synchronous HTTP requests
- **NEVER** use `setTimeout()` or `setInterval()` without proper cleanup
- **NEVER** perform heavy computations on the main thread without Web Workers
- **NEVER** subscribe to Observables without unsubscription strategy
- **NEVER** create infinite loops or recursive calls without termination conditions
- **NEVER** hold references to DOM elements beyond component lifecycle
- **MUST** use immutable state updates
- **MUST** handle race conditions with appropriate operators (`switchMap`, `mergeMap`, `concatMap`)
- **MUST** implement global error handling for unhandled Promise rejections
- **MUST** use `catchError` operator for Observable error handling

**pace-lumen-ui — README**
- **Security Integration**: Role-based access control with Spring Security
- **Framework**: Spring MVC with Spring Security
- **Security**: Role-based authentication and authorization

*See [setup/README.md](setup/README.md) for complete details*

**pace-lumen-ui — standards**
- **PATCH** - Used for partial updates (validation gap updates)
- All data structures must be defined as TypeScript interfaces
- Implement proper authentication and authorization
- **PATCH**: Partial updates (e.g., validation gap updates)
| PATCH | Validation updates | `updateValidationGap` |
- All components must implement proper lifecycle management through `ngOnInit` and `ngOnDestroy` interfaces
- Memory leaks must be prevented through proper subscription management in `ngOnDestroy`
- Components handling business logic must implement risk and priority classification methods
- All public methods and properties must include JSDoc comments
- Classification and categorization logic must be clearly documented
- Input/output properties must be documented with types and descriptions
- All components must pass linting checks
- Unit tests required for lifecycle methods and business logic
- Code coverage targets must be maintained
- **Requirement**: All Angular components must implement proper lifecycle management
- **Implementation**: Components must implement `ngOnInit` and `ngOnDestroy` interfaces
- **Rationale**: Ensures proper resource cleanup and initialization patterns
- **Requirement**: Components handling data categorization must implement risk and priority classification methods
- All code must pass static analysis checks
- Address linting warnings before code review
- Components must follow Angular performance best practices
- Avoid memory leaks through proper subscription management
- **Descriptive names**: Controllers named after primary resource or function
- **Custom success handling**: Role-specific redirection after authentication
POST /authenticate  # Login processing
- **Separation of concerns**: Security, web, and database configurations in separate classes
- **Spring Security integration**: Standardized security configuration patterns
- **Form handling**: Consistent form submission and validation patterns
- Admin operations require appropriate authorization
- User-specific resources validate ownership via username parameter

**pace-lumen-ui — context**
- **Project Analysis**: Upload and scan software repositories for architectural insights, security vulnerabilities, and code quality metrics
- **PATCH /** → **updateValidationGap**: Validation gap update endpoint
| PATCH | `/` | `updateValidationGap` | Update validation gaps |
| GET | `details` | `eitherDetailsOrFileValidator` | Validate file details |
| GET | `repositories` | `uniqueUrlsValidator` | Validate repository URLs |
| GET | `url` | `uniqueUrlsValidator` | Validate URLs |
- **PATCH /** → `updateValidationGap` - Updates validation gap information
These flows ensure proper data propagation through the Angular component hierarchy, business logic
| PATCH | / | `updateValidationGap` | Validation updates |
- **Configuration Storage**: External configuration management for AI team settings and validation parameters

*See [pace-lumen-ui_context.md](pace-lumen-ui_context.md) for complete details*

**pace-lumen-ui — Direction / Ethical_Governance**: See [direction_ethical_governance.md](direction_ethical_governance.md) for detailed information

**pace-lumen-ui — Direction / Feedback**: See [direction_feedback.md](direction_feedback.md) for detailed information

**pace-lumen-ui — Direction / Learning**: See [direction_learning.md](direction_learning.md) for detailed information

**pace-lumen-ui — Provenance / Boundary**: See [provenance_boundary.md](provenance_boundary.md) for detailed information

**pace-lumen-ui — Provenance / Stakeholder**: See [provenance_stakeholder.md](provenance_stakeholder.md) for detailed information

**ECommerce — context**
- **Security & Authentication**: Role-based access control with different permission levels for customers, administrators, and database administrators
- Spring Security for authorization and access control
- **Configuration Layer**: Spring configuration for security, database, and web flow setup
This architecture ensures maintainability, scalability, and clear separation between different system responsibilities while providing a robust foundation for e-commerce operations.
- **Roles**: Defines user authorization levels (admin, DBA, regular user)
- Authentication and authorization management
- **Configuration Layer**: Security, database, and application configuration management
   - Credentials processed by SecurityConfiguration
   - User data validated and processed
The application integrates with Spring Security for authentication and authorization:
- **SecurityConfiguration** - Configures security policies and access controls
   - Security setup via `SecurityConfiguration`
The `CustomSuccessHandler` manages role-based redirection after successful authentication, directing users to appropriate interfaces based on their roles (admin, DBA, or regular user).

*See [ECommerce_context.md](ECommerce_context.md) for complete details*

## 💡 Quick Reference

**Need Help?**

- **Working on pace-lumen-ui architecture topics**: [architecture.md](architecture.md)
- **Working on pace-lumen-ui context topics**: [constraint_operational.md](constraint_operational.md)
- **Working on ECommerce context topics**: [constraint_operational.md](constraint_operational.md)
- **Working on pace-lumen-ui context topics**: [constraint_policy.md](constraint_policy.md)
- **Working on ECommerce context topics**: [constraint_policy.md](constraint_policy.md)
- **Working on pace-lumen-ui context topics**: [constraint_regulatory.md](constraint_regulatory.md)
- **Working on ECommerce context topics**: [constraint_regulatory.md](constraint_regulatory.md)
- **Working on pace-lumen-ui context topics**: [constraint_resilience.md](constraint_resilience.md)
- **Working on ECommerce context topics**: [constraint_resilience.md](constraint_resilience.md)
- **Working on pace-lumen-ui context topics**: [constraint_security.md](constraint_security.md)
- **Working on ECommerce context topics**: [constraint_security.md](constraint_security.md)
- **Working on pace-lumen-ui context topics**: [pace-lumen-ui_context.md](pace-lumen-ui_context.md)
- **Working on ECommerce context topics**: [ECommerce_context.md](ECommerce_context.md)
- **Working on pace-lumen-ui context topics**: [direction_ethical_governance.md](direction_ethical_governance.md)
- **Working on ECommerce context topics**: [direction_ethical_governance.md](direction_ethical_governance.md)
- **Working on pace-lumen-ui context topics**: [direction_feedback.md](direction_feedback.md)
- **Working on ECommerce context topics**: [direction_feedback.md](direction_feedback.md)
- **Working on pace-lumen-ui context topics**: [direction_intentional.md](direction_intentional.md)
- **Working on ECommerce context topics**: [direction_intentional.md](direction_intentional.md)
- **Working on pace-lumen-ui context topics**: [direction_learning.md](direction_learning.md)
- **Working on ECommerce context topics**: [direction_learning.md](direction_learning.md)
- **Working on pace-lumen-ui context topics**: [meta_judgment.md](meta_judgment.md)
- **Working on ECommerce context topics**: [meta_judgment.md](meta_judgment.md)
- **Working on pace-lumen-ui context topics**: [provenance_boundary.md](provenance_boundary.md)
- **Working on ECommerce context topics**: [provenance_boundary.md](provenance_boundary.md)
- **Working on pace-lumen-ui context topics**: [provenance_cognitive.md](provenance_cognitive.md)
- **Working on ECommerce context topics**: [provenance_cognitive.md](provenance_cognitive.md)
- **Working on pace-lumen-ui context topics**: [provenance_decision.md](provenance_decision.md)
- **Working on ECommerce context topics**: [provenance_decision.md](provenance_decision.md)
- **Working on pace-lumen-ui context topics**: [provenance_dependency.md](provenance_dependency.md)
- **Working on ECommerce context topics**: [provenance_dependency.md](provenance_dependency.md)
- **Working on pace-lumen-ui context topics**: [provenance_stakeholder.md](provenance_stakeholder.md)
- **Working on ECommerce context topics**: [provenance_stakeholder.md](provenance_stakeholder.md)
- **Working on pace-lumen-ui context topics**: [provenance_structural.md](provenance_structural.md)
- **Working on ECommerce context topics**: [provenance_structural.md](provenance_structural.md)
- **Working on pace-lumen-ui context topics**: [provenance_temporal.md](provenance_temporal.md)
- **Working on ECommerce context topics**: [provenance_temporal.md](provenance_temporal.md)
- **Working on pace-lumen-ui policies topics**: [policies.md](policies.md)
- **Working on pace-lumen-ui rules topics**: [rules.md](rules.md)
- **Working on pace-lumen-ui setup readme topics**: [setup/README.md](setup/README.md)
- **Working on pace-lumen-ui standards topics**: [standards.md](standards.md)

*This instruction index references 12 context documents.*