# ai-assisted-project-management
This repository is a template with the necessary skills so that the development can be done with AI agents with different profiles that ensures best practices.

The enforced flow is as follows:

[Human Intent] 
      │
      ▼
👤 Project Owner Agent  <──(Iterative Refinement of Goals)──>  [Human Client]
      │
      ├─► Compares: Speed vs. Maintenance vs. Knowledge
      │
      ▼ (User Approves Option)
[Writes tech-stack.json]
      │
      ├─► 🧪 QA Guardian  ──► Generates Go/TS Testing Skills & Coverage Gates
      ├─► 🛡️ SecOps       ──► Generates Stack-Specific Dependabot & Security Audits
      │
      ▼
🖥️ Coder Agent ───────────► Reads Stack + Skills ──► Implements Idiomatic Code
      │
      ▼
🔎 Reviewer Agent ────────► Enforces Stack-Specific DoD ──► Merges to Main

