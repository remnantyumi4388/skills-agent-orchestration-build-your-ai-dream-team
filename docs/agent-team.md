# Agent team

The Mona's Project Pulse dashboard will be built by a coordinated team of custom agents. Each agent has a focused responsibility and an explicit file scope so planning, design, implementation, and integration remain clear.

| Agent | Target model | Responsibility | Definition |
| --- | --- | --- | --- |
| **Orchestrator** | Claude Opus 4.7 (copilot) | Coordinates the team, turns the plan into phases, delegates work with non-overlapping file scopes, manages dependencies, and verifies the integrated result. | [`.github/agents/orchestrator.agent.md`](../.github/agents/orchestrator.agent.md) |
| **Planner** | Claude Opus 4.7 (copilot) | Researches the repository and relevant documentation, identifies requirements, dependencies, edge cases, and risks, then produces an actionable implementation plan. | [`.github/agents/planner.agent.md`](../.github/agents/planner.agent.md) |
| **Designer** | Gemini 3.1 Pro (copilot) | Defines the dashboard's UI/UX direction, information hierarchy, accessibility, responsive behavior, visual styling, project cards, status badges, and priority treatment. | [`.github/agents/designer.agent.md`](../.github/agents/designer.agent.md) |
| **Coder** | GPT-5.5 (copilot) | Implements the assigned dashboard logic and application code, follows repository patterns, creates required runnable-app configuration, and validates deterministic, testable behavior. | [`.github/agents/coder.agent.md`](../.github/agents/coder.agent.md) |

I am using **GitHub Copilot CLI in a Codespace** to orchestrate this team: the Orchestrator delegates planning, design, and coding tasks to the specialist agents, then coordinates their integration into Mona's Project Pulse dashboard.
