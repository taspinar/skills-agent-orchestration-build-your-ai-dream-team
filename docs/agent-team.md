# Agent team for Mona's Project Pulse dashboard

We will use a four-agent custom team to plan, design, coordinate, and implement Mona's Project Pulse dashboard, all orchestrated through GitHub Copilot CLI in a Codespace.

- Planner — model: Claude Opus 4.7 (copilot)
  - Responsibility: researches the repo, reads the relevant files and docs, identifies edge cases, and produces a concrete implementation plan for the dashboard work.
  - Definition: `.github/agents/planner.agent.md`

- Orchestrator — model: Claude Opus 4.7 (copilot)
  - Responsibility: breaks the plan into execution phases, delegates work to the specialist agents, manages dependencies and sequencing, and verifies that the integrated result makes sense.
  - Definition: `.github/agents/orchestrator.agent.md`

- Designer — model: Gemini 3.1 Pro (copilot)
  - Responsibility: focuses on the user experience for Project Pulse, including layout, accessibility, information hierarchy, responsive behavior, and polished dashboard styling.
  - Definition: `.github/agents/designer.agent.md`

- Coder — model: GPT-5.5 (copilot)
  - Responsibility: implements the actual application logic and code changes within the file scope assigned by the Orchestrator, while keeping validation and behavior deterministic.
  - Definition: `.github/agents/coder.agent.md`

This team lives under the repository's custom agent folder and is coordinated from GitHub Copilot CLI inside a Codespace, with the Orchestrator acting as the central coordinator for the Project Pulse build.
