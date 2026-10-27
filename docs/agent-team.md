# Project Pulse Agent Team

This team will build Mona's Project Pulse dashboard through a coordinated, phased workflow. The Orchestrator will first ask the Planner to research the repository and produce an implementation plan with file ownership, dependencies, edge cases, and validation expectations. The Orchestrator will then assign non-overlapping tasks to the specialists, run independent work in parallel where appropriate, sequence dependent work, and verify the integrated result.

## Orchestrator

- **Model:** Claude Opus 5.5 (copilot)
- **Definition:** `.github/agents/orchestrator.agent.md`
- **Responsibility:** Coordinate the Planner, Designer, and Coder; turn the plan into phases with explicit file assignments; manage dependencies and parallel work; integrate and verify the result; report outcomes and blockers. The Orchestrator coordinates but does not implement the dashboard.

## Planner

- **Model:** Claude Opus 5.5 (copilot)
- **Definition:** `.github/agents/planner.agent.md`
- **Responsibility:** Research the repository, documentation, dependencies, and edge cases, then provide an ordered implementation plan. The plan identifies file ownership, dependencies, work that can run in parallel or must be sequential, risks, validation expectations, and open questions. The Planner does not write code.

## Designer

- **Model:** Gemini 3.7 Flash (copilot)
- **Definition:** `.github/agents/designer.agent.md`
- **Responsibility:** Define the dashboard's user experience, information hierarchy, accessibility, interaction flow, responsive behavior, and visual direction. For Project Pulse, the design should use visible project cards, status badges, clear priority treatment, readable spacing, and deterministic styling hooks such as `.dashboard` and `.project-card`.

## Coder

- **Model:** GPT-5.5 (copilot)
- **Definition:** `.github/agents/coder.agent.md`
- **Responsibility:** Implement the assigned application files and support configuration, following the agreed design and existing repository patterns. The Coder keeps behavior explicit and testable, validates the implementation, and reports files changed and remaining risks.

## How the team will work together

1. The Orchestrator delegates repository research and planning to the Planner before implementation.
2. The Planner proposes ordered phases, ownership, dependencies, and validation expectations.
3. The Orchestrator assigns the design direction to the Designer and implementation to the Coder, keeping file scopes separate.
4. The Designer's guidance informs the Coder's dashboard implementation. Independent tasks may run in parallel only when their file scopes do not overlap and there are no dependencies; dependent tasks run sequentially.
5. The Orchestrator reviews the integrated dashboard against the plan and validation expectations, then reports the outcome and any blockers.

The agents do not stage, commit, or push changes; those Git operations remain under the learner's control.
