# Project 01: Autonomous Bug Diagnosis & Self-Healing CI/CD Pipeline
## Governed AI-SDLC & Disciplined Vibe Coding Execution Plan

**Author:** Vishwanath S | Lead Software Architect (`@vishwanath_tns`)  
**Repository:** [https://github.com/vishwanath-tns/autonomous-self-healing-cicd](https://github.com/vishwanath-tns/autonomous-self-healing-cicd)  
**Architecture Pattern:** Closed-Loop ReAct State Machine + Deterministic Tool Registry + Automated Test Oracle  

---

## 1. Executive Objective

Build a production-grade **Autonomous Bug Diagnosis & Self-Healing CI/CD Agent** following a strict, governed Software Development Life Cycle (SDLC). Rather than unguided "vibe coding" that produces monolithic spaghetti and untested happy paths, this project enforces **Architectural Governance** at every stage:

* **Zero-Dependency Simulation Mode + Live Gemini Mode:** Runs out-of-the-box with Python 3.11+ standard library and upgrades seamlessly to live Google Gemini tool-calling when `GEMINI_API_KEY` is provided.
* **Sub-3-Minute MTTR:** Automatically ingests failing CI stack traces, localizes the root cause in source code, synthesizes a minimal defensive patch, verifies the fix in a sandboxed test runner, records an immutable SQLite audit trail, and prepares a clean Git branch & Pull Request.

---

## 2. The 4 Invariant Rules of Disciplined Vibe Coding

Every file and commit in this repository obeys four non-negotiable architectural invariants:

| Rule | Invariant Principle | Architectural Enforcement in This Repo |
| :--- | :--- | :--- |
| **Rule 1** | **Schema as Law (Contracts Before Code)** | Define strict typed schemas (`StackFrame`, `ToolCall`, `PatchCandidate`, `HealResult`, `AuditRecord`, `WebhookPayload`) before writing business logic. Prevents context amnesia across prompts. |
| **Rule 2** | **Strict Modular Isolation (`< 200 LOC` / file)** | Every source file in `src/` has a single responsibility and is strictly capped at `< 200 lines of code` (verified automatically in CI). |
| **Rule 3** | **Invariant Tests Before Syntax (TDD for Agents)** | Write unit tests and explicit negative failure-mode assertions *before* implementing module code. The implementation iterates against deterministic tests. |
| **Rule 4** | **Explicit Public Domain Boundaries** | Strict decoupling between Webhook Ingestion, ReAct Reasoning Core, Sandboxed Tool Registry, L3 SQLite Audit Ledger, and Git/PR Publisher. |

---

## 3. Target Repository Architecture

```text
autonomous-self-healing-cicd/
├── README.md                        # Quickstart, Architecture Diagram & Verification Proof
├── ARCHITECTURE.md                  # System Specification, FSM Lifecycle & Tool Contracts
├── SDLC_PROJECT_PLAN.md             # This Governed AI-SDLC Execution Plan (Markdown)
├── SDLC_PROJECT_PLAN.pdf            # Executive Printable Project Plan (PDF)
├── SPECS.md                         # Phase 1: Gherkin Acceptance Criteria & 5 Negative Cases
├── src/
│   ├── __init__.py
│   ├── models.py                    # Rule 1: Typed Data Contracts & Enums (< 200 LOC)
│   ├── guardrails.py                # Blast-Radius Policy & Path Traversal Protection (< 200 LOC)
│   ├── trace_parser.py              # Stack Trace Regex & AST Localization (< 200 LOC)
│   ├── tools.py                     # Sandboxed Tool Registry: Read, Patch, Test Runner (< 200 LOC)
│   ├── audit_ledger.py              # L3 SQLite Audit Ledger for Compliance & Traceability (< 200 LOC)
│   ├── llm_client.py                # Dual-Mode Reasoning Engine: Sim + Live Gemini w/ L1 Cache (< 200 LOC)
│   ├── agent.py                     # Stochastic FSM & ReAct Controller w/ Circuit Breaker (< 200 LOC)
│   ├── git_publisher.py             # Automated Branch & Pull Request Emitter (< 200 LOC)
│   └── webhook_app.py               # HTTP / FastAPI CI Failure Webhook Gateway (< 200 LOC)
├── target_service/                  # Microservice Under Test
│   ├── __init__.py
│   ├── order_service.py             # Financial Order Processing Service
│   └── test_order_service.py        # Unit Test Suite
├── tests/                           # Rule 3: TDD Verification Harness
│   ├── test_contracts_and_ledger.py # Schema & SQLite L3 Ledger Tests
│   ├── test_tools_and_guardrails.py # Tool Execution & Negative Security Invariants
│   ├── test_agent_fsm.py            # End-to-End ReAct FSM & Circuit Breaker Tests
│   └── test_webhook_and_git.py      # Webhook Ingestion & PR Publisher Tests
├── diagrams/                        # Architecture Diagram Generators & Assets
│   ├── generate_diagram.py
│   ├── generate_diagram_mobile_white.py
│   └── Project_01_Architecture_Diagram.png
├── run_demo.py                      # 1-Click End-to-End Autonomous Repair Demo
└── .github/workflows/ci.yml         # Automated CI Pipeline + <200 LOC Architecture Gate
```

---

## 4. Step-by-Step 6-Phase SDLC Execution Roadmap

We execute one phase at a time with an explicit **Human Architect Approval Gate** between phases.

### Phase 1: Requirements & Spec Engineering (`SPECS.md`)
* **Deliverable:** Formal `SPECS.md` specification document.
* **Scope:**
  1. Define user stories and system state transitions (`INGESTION` $\rightarrow$ `LOCALIZATION` $\rightarrow$ `HYPOTHESIS` $\rightarrow$ `PATCHING` $\rightarrow$ `VERIFICATION` $\rightarrow$ `PR_EMISSION` / `CIRCUIT_BREAKER_ABORT`).
  2. Write strict **Gherkin (`Given-When-Then`)** acceptance criteria for every state transition.
  3. Specify **5 Mandatory Negative & Security Test Cases**:
     * **Negative Case 1 (Blast-Radius Violation):** Agent attempts to modify forbidden infrastructure files (`Dockerfile`, `.github/workflows/ci.yml`, `pyproject.toml`) $\rightarrow$ Blocked immediately by `GuardrailViolationError`.
     * **Negative Case 2 (Max-Retry Circuit Breaker):** Candidate patches fail verification 3 consecutive times $\rightarrow$ Loop aborts cleanly on iteration 3, restores original file state, and logs `CIRCUIT_BREAKER_ABORT`.
     * **Negative Case 3 (Subprocess Timeout Protection):** A bad patch introduces an infinite `while True` loop $\rightarrow$ Sandboxed test runner enforces a strict timeout and kills the child process safely.
     * **Negative Case 4 (Non-Exact / Hallucinated Diff Target):** Agent attempts to patch a code snippet that does not match the target file $\rightarrow$ Rejected atomically with zero file corruption.
     * **Negative Case 5 (Idempotency on Healthy Build):** Agent is triggered when all tests already pass $\rightarrow$ Exits immediately with status `ALREADY_PASSING` and `0` iterations.
* **Gate 1 Check:** Review and approve `SPECS.md` before Phase 2.

---

### Phase 2: Architecture & Contract Definition (`src/models.py` & `src/audit_ledger.py`)
* **Deliverable:** Strictly typed domain models (`src/models.py`) and the SQLite L3 Audit Ledger (`src/audit_ledger.py`), plus `ARCHITECTURE.md` alignment.
* **Scope:**
  1. Define immutable data structures for `StackFrame`, `TestRunResult`, `PatchProposal`, `TrajectoryStep`, `HealCycleReport`, and `CIWebhookEvent`.
  2. Define the SQLite L3 schema (`healing_audit_ledger` table storing `build_id`, `timestamp`, `exception_type`, `target_file`, `line_number`, `applied_diff`, `iterations`, `final_status`, `pr_branch`).
* **Gate 2 Check:** Review schemas and contracts before Phase 3.

---

### Phase 3: Test-Driven Harness First — TDD Red Phase (`tests/`)
* **Deliverable:** Complete unit and contract test suites written *before* the core engine modules.
* **Scope:**
  1. Write `tests/test_contracts_and_ledger.py` testing model invariants and SQLite audit CRUD.
  2. Write `tests/test_tools_and_guardrails.py` testing file range reading, atomic patching, rollback, blast-radius blocking, and subprocess timeouts.
  3. Write `tests/test_agent_fsm.py` and `tests/test_webhook_and_git.py` testing the ReAct state machine, circuit breaker, webhook payload ingestion, and PR metadata generation.
* **Gate 3 Check:** Verify test suite coverage and failure assertions before Phase 4.

---

### Phase 4: Context-Isolated Module Implementation (TDD Green Phase)
* **Deliverable:** Modular implementation in `src/` where every file is `< 200 LOC` and turns the Phase 3 tests green.
* **Sub-steps:**
  * **Step 4.1:** Implement `src/guardrails.py`, `src/trace_parser.py`, and `src/tools.py` (with automatic backup/rollback on failed patches).
  * **Step 4.2:** Implement `src/llm_client.py` (Dual-Mode: deterministic offline diagnosis + live Google Gemini REST API with frozen L1 System Prefix for KV-cache efficiency) and `src/agent.py` (ReAct FSM controller).
  * **Step 4.3:** Implement `src/git_publisher.py` and `src/webhook_app.py`.
  * **Step 4.4:** Organize `target_service/` and upgrade `run_demo.py` to showcase the full pipeline (including SQLite ledger inspection and PR output).
* **Gate 4 Check:** All unit tests and demo runs pass 100%.

---

### Phase 5: Automated CI/CD Guardrails (`.github/workflows/ci.yml`)
* **Deliverable:** Production GitHub Actions pipeline.
* **Scope:**
  1. Automated architectural linter verifying no Python file in `src/` exceeds 200 lines of code.
  2. Automated execution of the full `unittest` suite across positive and negative invariants.
  3. Automated execution of `python run_demo.py` to verify end-to-end self-healing in CI.
* **Gate 5 Check:** Verify CI workflow configuration before Phase 6.

---

### Phase 6: Senior Architect Review, Documentation Polish & Release
* **Deliverable:** Executive `README.md`, clean directory tree, and final Git push to `main`.
* **Scope:**
  1. Audit repository cleanliness (remove legacy duplicates, verify diagrams).
  2. Write comprehensive `README.md` with architecture visuals, quickstart commands, and sample terminal output.
  3. Commit and push release to `https://github.com/vishwanath-tns/autonomous-self-healing-cicd.git`.
