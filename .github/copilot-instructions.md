# Instructions — ECommerce

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

**Project Name:** ECommerce

**Purpose:** This document provides comprehensive instructions for AI coding assistants working on this codebase.

**Document Type:** Dynamic Index

## 📚 Repository Context Documents

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

**Setup Readme:**

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

**architecture**
- **Security & Authentication**: Role-based access control with separate user and admin interfaces
- **Spring Security**: Authentication and authorization management
- **SecurityConfiguration**: Spring Security setup with authentication and authorization
- **SecurityWebApplicationInitializer**: Security filter initialization
4. **Role-Based Access Control**: Security integrated at the architectural level
        SEC[Security Config]
- **Boundaries**: Serves public endpoints and authenticated user operations
- **Components**: `SecurityConfiguration`, `CustomSuccessHandler`, `SecurityWebApplicationInitializer`
- **Responsibilities**: Authentication, authorization, and role-based access control
- Route mapping and request validation
- Cross-cutting concerns like validation and security
- Relationship definitions and constraints
- **Security Integration**: All layers protected by Spring Security configuration
- `SecurityConfiguration`: Spring Security configuration with authentication setup
- `SecurityWebApplicationInitializer`: Security web application initializer
- `SecurityConfiguration` integrates with `CustomSuccessHandler` for role-based authentication
- `SecurityConfiguration` uses `CustomSuccessHandler` for authentication flow
Login Request → SecurityConfiguration → CustomSuccessHandler → Dashboard/Home
- Spring Security handles concurrent user sessions
- User context maintained through Spring Security authentication
- SecurityConfiguration integrates with CustomSuccessHandler for authentication flows
Security is handled through:
- `SecurityConfiguration` with custom success handlers
- `SecurityConfiguration` for authentication and authorization
    participant SecurityConfig
    Controller->>SecurityConfig: Authenticate
    SecurityConfig->>Database: Validate Credentials
    Database-->>SecurityConfig: User Details
    SecurityConfig->>CustomSuccessHandler: Success Callback
   - UserAccountController validates input

**Constraint / Operational**
Before setting up the ECommerce application, dependencies installed:
- **Spring Security** - Authentication and authorization framework

*See [constraint_operational.md](constraint_operational.md) for complete details*

**Constraint / Policy**
**Controller-Level Security**
- All controllers must implement proper service layer dependencies for user authentication
**Data Access Security**
- All domain entities (User, Product, Cart) implement Serializable interface to ensure secure data transmission
- Service layer acts as security boundary between controllers and data access
**HTTP Method Security**
- **SECURITY CONCERN**: Current implementation uses GET methods for delete operations, which violates REST security principles
**Layered Security Architecture**
1. **Presentation Layer**: Controllers validate user input and enforce view-level security
2. **Business Logic Layer**: Services implement business rules and authorization logic
3. **Data Access Layer**: DAOs provide controlled database access with proper query validation
**Required Security Implementations**
- Service layer must validate user permissions before executing business operations
- All user inputs must be validated at controller level before passing to services
- Session management and user authentication must be handled through dedicated service components
**Requirement**: All user-facing endpoints must implement authentication
- User authentication required for cart operations
- Admin authentication required for category and product management
- Session timeout after 30 minutes of inactivity
- Service layer must validate user roles before data operations
- Parameter validation in controller methods
- Business rule validation in service layer
- **Rule**: All administrative functions must be segregated under the `/admin` path prefix
- **Rationale**: Ensures clear separation between administrative and user-facing functionality
- **Rule**: System must enforce role-based access with three distinct privilege levels
- **Rule**: User-specific resources must be accessed via username-parameterized paths
- **Purpose**: Ensures users can only access their own resources
- **Rule**: Shopping cart operations must maintain data integrity for quantity and totals
  - System must track and calculate cart totals accurately
  - Cart state must persist across user sessions

*See [constraint_policy.md](constraint_policy.md) for complete details*

**Constraint / Regulatory**
- Use Spring Security for authentication and authorization
- Protect admin endpoints with appropriate security constraints
- Implement proper authorization checks
  - `SecurityConfiguration` for security settings
  - `SecurityWebApplicationInitializer` for security
- All unit tests must pass
- Code coverage must not decrease
- Controllers must inject required service dependencies:
- All domain entities must implement the `Serializable` interface
- Ensures proper data transfer and session management
- **Return Type Validation**: Ensure endpoint return types are appropriate - most controllers return String types for view names or redirect paths
- **Serialization Compliance**: All domain entities (Cart, Product, User) must implement the Serializable interface
- **Data Integrity**: Validate that entity relationships and constraints are properly defined
- **Dependency Injection**: Controllers must properly inject required service dependencies:
  - Validate all service dependencies are correctly wired
- Include parameter validation rules and response formats
- Service layer methods must include JavaDoc comments
- Controller endpoint purposes and expected behaviors must be documented
- [ ] Entities implement required interfaces (Serializable)
- All domain entities must pass serialization validation
- Controller-service wiring must be verified during code review
- **Serializable Entities**: All domain entities must implement the `Serializable` interface
  - Ensures proper serialization support for session management and caching
- **Service Dependencies**: Controllers must properly inject required service dependencies
  - Required services: `productService`, `userService`, `categoryService`
  - Ensure consistent return type patterns across controllers
- Verify all entities implement required interfaces
- Check service dependency injection in controllers
- Validate HTTP method usage patterns
- Ensure proper return type consistency

*See [constraint_regulatory.md](constraint_regulatory.md) for complete details*

**Constraint / Resilience**
- All controller methods must implement proper exception handling
- Use 400 for validation errors
- **FORBIDDEN**: Catching exceptions without proper handling or logging
- **FORBIDDEN**: Returning null or empty responses without error indication
- **FORBIDDEN**: Ignoring validation failures
- **FORBIDDEN**: Using generic Exception catch blocks without specific handling
- **FORBIDDEN**: Throwing generic RuntimeException without context
- **FORBIDDEN**: Using printStackTrace() instead of proper logging
- **FORBIDDEN**: Different error response formats across endpoints
- **FORBIDDEN**: Exposing internal system details in error messages
- **FORBIDDEN**: Inconsistent HTTP status codes for similar error conditions
- **FORBIDDEN**: Not properly closing resources in finally blocks or try-with-resources
- **FORBIDDEN**: Leaving database connections open on exceptions
- **FORBIDDEN**: Memory leaks due to improper exception handling
- **Controller Dependencies**: Controllers must properly inject service dependencies to enable async processing patterns
- **Serializable Entities**: All domain entities must implement `Serializable` interface to support async processing and caching mechanisms
- Ensure all entity classes implement `Serializable` for session storage, data access layers
- **Entity Requirements**: All domain entities must implement the `Serializable` interface for proper ORM functionality and session management
- **Service Layer Pattern**: Database access must be performed through the service layer, not directly from controllers
- **Controller Injection**: Controllers must inject required service dependencies:
- **Entity Serialization**: Ensure all new entities implement `Serializable` interface
- **Service Integration**: New database entities must have corresponding service classes
- **Controller Integration**: Controllers accessing new entities must inject appropriate service dependencies

*See [constraint_resilience.md](constraint_resilience.md) for complete details*

**Constraint / Security**
- **Controller Access Control**: All controllers must implement proper authentication checks before processing requests
- **Service Layer Security**: Service dependencies (productService, userService, categoryService) must validate user permissions before data operations
- **Serialization Security**: Entities implementing Serializable interface must not expose sensitive data during serialization
- **Sensitive Data Handling**: Never log or expose user credentials, payment information, or personal data
- **Database Security**: Use parameterized queries to prevent SQL injection
- **Configuration Security**: Store database credentials

*See [constraint_security.md](constraint_security.md) for complete details*

**context**
- **Secure Operations**: Role-based authentication and authorization with support for multiple user types (DBA, Admin, User)
- Shopping cart system for item management and checkout processes
- Spring Security for authentication and role-based access control
- Users must be authenticated to access cart and wishlist functionality
- Products must be associated with both a category and supplier
- **Security**: Admin endpoints require appropriate role authorization
   - SecurityConfiguration processes authentication
   - User data validation and account creation
- **Custom Authentication**: `SecurityConfiguration` uses `CustomSuccessHandler` for specialized authentication flows
- **Login**: Users authenticate through `/login` with role-based redirection handled by `CustomSuccessHandler`
1. **Login**: Administrators authenticate and are redirected to admin dashboard (`/admin/dashboard`)
2. **Security Setup**: Configure authentication and authorization through `SecurityConfiguration`
1. Browse products → Add to cart (`/addCart`) → View cart (`/{username}/cart`) → Remove items if needed (`/{username}/cart/remove-cart/{cart_id}`) → Proceed to checkout
- **Security Layer**: Spring Security for authentication and authorization
- Spring Security for authentication/authorization

*See [ECommerce_context.md](ECommerce_context.md) for complete details*

**Direction / Ethical_Governance**: See [direction_ethical_governance.md](direction_ethical_governance.md) for detailed information

**Direction / Feedback**: See [direction_feedback.md](direction_feedback.md) for detailed information

**Direction / Intentional**: See [direction_intentional.md](direction_intentional.md) for detailed information

**Direction / Learning**: See [direction_learning.md](direction_learning.md) for detailed information

**Provenance / Boundary**: See [provenance_boundary.md](provenance_boundary.md) for detailed information

**Provenance / Cognitive**
- All domain entities must implement `Serializable` interface
- Controllers must only handle HTTP request/response concerns
- Controllers must delegate business logic to the service layer
- Controllers must inject required service dependencies (productService, userService, categoryService)
- Services contain all business logic and validation
- Services must not directly handle HTTP concerns
- Services must use DAO layer for all data access operations
- DAOs must not contain business logic
- All domain entities must implement Serializable interface
- Models should not contain business logic methods
- Models may import: Standard Java libraries, validation annotations
- Models must not import Spring MVC or web-specific classes
- DAOs must not import controller or web layer classes
- Services must not import HTTP servlet classes directly
- **Rule**: DELETE operations must use HTTP DELETE method, not GET
- Controllers must not directly access database or persistence layer
- All data access must go through the DAO layer
- Controllers must not contain business rules or validation logic
- All business logic must reside in the service layer
- Lower layers (DAO, Model) must not depend on higher layers (Service, Controller)
- Circular dependencies between layers are forbidden

*See [provenance_cognitive.md](provenance_cognitive.md) for complete details*

**Provenance / Decision**: See [provenance_decision.md](provenance_decision.md) for detailed information

**Provenance / Dependency**: See [provenance_dependency.md](provenance_dependency.md) for detailed information

**Provenance / Stakeholder**: See [provenance_stakeholder.md](provenance_stakeholder.md) for detailed information

**Provenance / Structural**: See [provenance_structural.md](provenance_structural.md) for detailed information

**rules**
- **Integration Tests**: Required for all controller endpoints and database operations
- **End-to-End Tests**: Critical user flows must have automated E2E coverage
- **HTTP Method Validation**: Verify correct HTTP methods (note: system uses GET for delete operations)
- **Service Integration**: Mock service dependencies and verify interactions
- **Data Validation**: Verify input validation and sanitization
- **Serialization**: Verify Serializable implementation works correctly
- **Validation**: Test entity validation rules
**❌ NEVER create entities without Serializable**
**❌ NEVER create controllers without proper service injection**
**❌ NEVER return raw strings for REST APIs**
- **Never use GET requests for state-changing operations** (create, update

**README**
- **Security Integration**: Role-based access control separating customer and administrative functions
- Spring Security for authentication and authorization
- Spring Framework (MVC, Security, Web Flow)
- **Security Configuration**: Spring Security integration for authentication/authorization
Before setting up the ECommerce application, dependencies installed:
Before installing the ECommerce application
- **Spring Security** - Authentication, authorization framework
| Variable | Description | Required | Default |
- **Spring Security**: Authentication and authorization configuration
- **Spring Security** - Authentication and authorization framework
   - Configure Spring Security settings for local development
   - Leverage Spring Security for testing authentication scenarios
**Problem**: Authentication or authorization failures
- **Solution**: Review Spring Security configuration
- **Check**: Verify user roles

*See [setup/README.md](setup/README.md) for complete details*

## 💡 Quick Reference

**Need Help?**

- **Working on architecture topics**: [architecture.md](architecture.md)
- **Working on context topics**: [constraint_operational.md](constraint_operational.md)
- **Working on policies topics**: [policies.md](policies.md)
- **Working on rules topics**: [rules.md](rules.md)
- **Working on setup readme topics**: [setup/README.md](setup/README.md)
- **Working on standards topics**: [standards.md](standards.md)

*This instruction index references 19 context documents.*