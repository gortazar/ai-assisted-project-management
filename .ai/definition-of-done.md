# Definition of Done (DoD) - Mandatory Quality Standards

This document establishes the minimum, non-negotiable requirements that ANY AI agent or human developer must fulfill before a task can be considered "Done" and submitted as a Pull Request (PR). Failure to meet any of these criteria will result in an automatic rejection of the changes.

---

## 1. Application Code & Quality Metrics
*   **Static Analysis:** Code must pass the project's official Linter and Formatter with zero errors or warnings.
*   **Cyclomatic Complexity:** No individual function or method may exceed a cyclomatic complexity score of **10**.
*   **Code Duplication:** New code must introduce **0% duplication** (zero tolerance for copy-pasting identical blocks). Reuse abstractions instead.
*   **Technical Debt Management (The Golden Rule):**
    *   If a task must introduce technical debt (e.g., a temporary workaround due to external API limitations), the agent **MUST** satisfy one of these three conditions:
        1. **Accompanying Refactoring:** Include a refactor within the same PR that pays down an equivalent or greater amount of pre-existing technical debt, maintaining a neutral or positive quality balance.
        2. **Exceptional Documentation:** Explicitly document the debt inline using a `// DEBT: [Reason & Strategy]` comment, state it clearly in the PR description, and outline a concrete plan to resolve it.
        3. **Rejection:** If the debt increase is neither justified nor offset by refactoring, the change will be **automatically rejected**.

## 2. Testing & Test Quality (Strict Gates)
*   **Minimum Coverage:** All new or modified lines of code must achieve a minimum of **80% unit test coverage**.
*   **Integration Tests:** If the functionality introduces a new API endpoint, data contract, or external system integration (databases, third-party APIs), it must include at least one robust integration test.
*   **Regression:** The complete existing test suite must be executed and achieve a **100% success rate**.
*   **Test Meaningfulness:** "Shallow tests" (tests that execute code paths to boost coverage numbers without asserting real behavior) are strictly prohibited. Assertions must be precise and explicitly targeted.
*   **State Isolation:** Tests must thoroughly clean up their own mocks, stubs, and database states (`afterEach`, `afterAll`) to guarantee zero cross-test contamination.

## 3. Security & Supply Chain
*   **Vulnerability Threshold:** No new dependencies containing Medium, High, or Critical known vulnerabilities (CVEs) may be introduced.
*   **Secret Prevention:** Hardcoding credentials, API keys, private tokens, or environment-specific configuration strings is strictly forbidden. All configuration must be driven by environment variables.
*   **Security Scanning (SAST):** The code changes must pass the repository's security pipelines with zero active flags.

## 4. Infrastructure as Code (IaC) & CI/CD
*   **Infrastructure Validation:** Any changes modifying infrastructure manifests (Terraform, Ansible, etc.) must pass their respective linting tools (e.g., `tflint`) and cloud architecture syntax checks.
*   **Pipeline Health:** The proposed code must compile perfectly and pass all local continuous integration scripts before a remote PR review is requested.

## 5. Live Documentation & Architecture
*   **Inline Documentation:** Complex logic or non-obvious business rules must be clearly documented inline using standardized codebase comment formats.
*   **Project Documentation:** If an API contract, component behavior, or setup step changes, the corresponding `.md` file inside the `docs/` folder must be updated in the same commit.
*   **Architecture Decision Records (ADR):** Any change modifying the software design, core database schemas, foundational API contracts, or introducing a core framework component **MUST include a new ADR entry** in `docs/architecture-decisions/` following the MADS template.
    *   The *Reviewer Agent* will automatically reject any structural change that lacks a corresponding ADR file update.

---

## Checklists for the Reviewer Agent
Before granting approval to any Pull Request, verify:
1. [ ] Does the implementation match the exact scope of the issue/ticket?
2. [ ] Are all static analysis, complexity (max 10), and duplication (0%) metrics fully satisfied?
3. [ ] If architecture was impacted, has an ADR been added to `docs/architecture-decisions/`?
4. [ ] Does the code hit the required 80% test coverage and satisfy the mutation test gate?
5. [ ] Is the codebase entirely free of new vulnerabilities or exposed secrets?

If any check is incomplete, deny the merge request, block the pipeline, and provide actionable feedback to the authoring agent.
