# AI Agents, Claude Code, and Practical Workflows

This document organizes all the notes into a structured guide, in this order:

1. What agents are (concepts & mental models)  
2. What Claude Code is and the features/tools it provides  
3. How to organize Claude Code (memory, files, team practices)  
4. How to actually use Claude Code (workflows & patterns)  
5. Different AI models and what they’re good for  
6. A concrete workflow: Gemini for PRDs → Claude Code for planning & coding → docs and TODOs along the way  

---

## 1. What Are Agents?

### 1.1 Mental Model of AI Agents

Think of agents as:

- Eager, tireless junior developers:
  - Stubborn, well-read, and won’t admit when they don’t “know” something.
  - They try to be helpful even when they’re unsure.

- Great at:
  - Generating variants of tests and then fixing them.
  - Rubberducking: asking deep questions and explaining unfamiliar files or functions.
  - Doing research and summarizing documentation (replacing a lot of StackOverflow/Googling).

- Weak at:
  - Always using the correct (or even real) methods, classes, and APIs.
    - They hallucinate method names based on vibes (name + context) rather than actual definitions.
  - Following the best, up-to-date, existing practices.
    - Often use deprecated APIs or outdated patterns.
  - Writing tests from scratch that follow your standards (there are so many bad examples in the training data that tests can be mediocre by default).

Your job as an engineer is not to get “perfect one-shot code,” but to:

- Use agents as:
  - Explorers
  - Translators of intent into code
  - Accelerators of refactors, tests, and documentation
- And then:
  - Review their output
  - Guide them through multiple iterations
  - Encode what they learned into persistent memory (docs, CLAUDE.md, etc.)

### 1.2 Subagents, Skills, Tools, and MCP

Within the Claude ecosystem, “agents” interact with:

- **Subagents**:
  - Separate agent instances used for specific tasks (e.g. “research this library”, “verify tests”, “QA UI/UX”).
  - Each subagent has its own conversation history and context, keeping the main chat clean.
  - Best used for narrow, focused tasks—not as fully autonomous roles.

- **Skills** (Agent Skills):
  - Mini “capabilities” defined via SKILL.md files and supporting scripts/templates.
  - Automatically invoked by Claude when relevant (model-invoked, not slash-command-invoked).
  - Great for formalizing scripting-style workflows.

- **Tools**:
  - File tools (read/write), Bash commands, CLI utilities, test runners, etc.
  - Claude decides when to call them based on the context and your instructions.

- **MCP (Model Context Protocol)**:
  - Connects agents to external systems and environments.
  - Best thought of as a secure “data gateway”:
    - High-level tools like: download_raw_data, take_sensitive_gated_action, execute_code_in_environment_with_state.
    - Avoid mirroring REST APIs one function at a time; let the agent script against a few powerful primitives.

---

## 2. What Is Claude Code and What Can It Do?

Claude Code is an **AI IDE and agent framework** centered on:

- Working directly with your filesystem  
- Reading and writing code  
- Running commands and tests  
- Managing context and memory over time  

### 2.1 Major Features

- **Interactive CLI agent**:
  - Reads files, edits code, runs tests and build commands.
  - Works in multiple modes (Default, Auto, Plan).

- **Multiple chat modes**:
  - Default: suggests changes, waits for permission.
  - Auto: more autonomous, applies changes without asking for every step.
  - Plan: focuses on strategic planning and extended thinking before coding.

- **Memory system**:
  - Uses a hierarchy of CLAUDE.md and CLAUDE.local.md files across:
    - User-level (~/.claude/CLAUDE.md)
    - Project root (./CLAUDE.md)
    - Subdirectories (apps/foo/CLAUDE.md)
    - Local-only notes (CLAUDE.local.md)

- **Tools & Commands**:
  - File reads/writes, Bash commands, Git operations, test runners, etc.
  - Custom slash commands like /catchup or /pr.
  - Hooks that fire before/after tool use.

- **Skills (Agent Skills)**:
  - SKILL.md + supporting scripts/templates.
  - Model-invoked: Claude chooses when to use them.
  - Optional allowed-tools restrictions for security.

- **MCP**:
  - Connect Claude to external systems like Playwright, documentation sources, non-prod databases, APIs, etc.
  - Best used as a gateway, not as an overloaded API.

- **Git integration & Worktrees**:
  - Works well with Git worktrees for parallel branches and per-feature Claude contexts.

- **Claude Code SDK**:
  - Use the same engine programmatically.
  - Batch refactors, build internal chat tools, or prototype new agents.

- **GitHub Action (Claude Code GHA)**:
  - Run Claude Code inside GitHub Actions.
  - Create PRs from Slack/Jira/alerts.
  - Full logging for review and continuous improvement.

- **Config via settings.json**:
  - Networking (proxies).
  - Timeouts for tools.
  - API keys and permissions.
  - Permission behavior and security controls.

### 2.2 Chat Modes in More Detail

- **Default Mode**:
  - You ask Claude to do something.
  - It shows a plan or proposed edits.
  - You approve or reject steps.
  - Use /output-style to tell Claude how much to explain.

- **Auto Mode**:
  - “Vibe coder” mode.
  - Claude edits files and runs many steps without waiting for every confirmation.
  - Still asks for permission on sensitive commands unless:
    - You configure /permissions.
    - Or start with a “skip permissions” flag (risky without guardrails).
  - Hit Escape to stop if it goes off course.

- **Plan Mode**:
  - Designed for complex or new features.
  - Claude spends more time thinking and drafting a plan before touching code.
  - You can control thinking depth with hints like “think”, “think hard”, “think harder”, “ultrathink”.
  - Ideal for:
    - New features
    - Complex refactors
    - New projects from a PRD

---

## 3. Organizing Claude Code: Memory, Files, and Team Practices

### 3.1 Memory Hierarchy

Claude uses several layers of memory:

- **Global User Memory (~/.claude/CLAUDE.md)**:
  - Your cross-project preferences.
  - Example: tabs vs spaces, ESLint preferences, commit style.

- **Project Memory (./CLAUDE.md)**:
  - Shared, checked-in documentation:
    - Architecture decisions.
    - Common patterns.
    - Tooling conventions.
    - Testing strategy.
    - Logging and security guidelines.
  - Treated as part of your repo infrastructure.

- **Local Project Memory (./CLAUDE.local.md)**:
  - Gitignored, private notes:
    - Sandbox URLs.
    - Personal debug tricks.
    - Temporary focus tasks.

- **Subdirectory CLAUDE.md files**:
  - For component-specific rules:
    - e.g. apps/ui/CLAUDE.md, apps/api/CLAUDE.md, cdk/CLAUDE.md.
  - Only loaded when editing files in that subtree, limiting context bloat.

**Behavior:**  
When Claude runs in a directory, it recursively loads CLAUDE.md and CLAUDE.local.md from that directory upward to the root, plus the global user file.

### 3.2 The Cost of Forgetting

Without proper CLAUDE.md organization:

- Every new session re-pays:
  - Architecture explanation.
  - Coding conventions.
  - Project-specific patterns.
  - Clarifications of stack choices.

Over dozens of sessions, this wastes:

- Time (chatting to “re-teach” the agent)
- Tokens (context cost)
- Knowledge (insights lost when conversation ends)

### 3.3 Updating Memory Automatically

A useful pattern at the end of a good session:

- Explicitly ask Claude to:
  - Capture any new learnings or corrections.
  - Decide whether they belong in:
    - Shared CLAUDE.md (team knowledge).
    - Local CLAUDE.local.md (personal notes).
    - A specific component’s CLAUDE.md (apps/foo/CLAUDE.md, etc.).

Over time, this builds:

- A knowledge base the agent can rely on.
- Better first attempts (you start closer to attempt 2 or 3).

### 3.4 Philosophy for Writing CLAUDE.md

Treat CLAUDE.md as the agent’s **constitution**:

- **Start with guardrails, not manuals**:
  - Begin by documenting what Claude gets wrong and how to prevent it.

- **Avoid huge embedded docs**:
  - Don’t paste entire docs or @-pull huge files into CLAUDE.md.
  - Instead, mention:
    - Where to find docs.
    - When to read them.
    - Why they’re important.
    - Example: “For complex usage or FooBarError, read path/to/docs.md.”

- **Avoid pure “never” rules**:
  - Instead of “Never use flag X”, use “Avoid flag X; prefer Y in cases A/B/C.”

- **Forcing function for tool design**:
  - If you need paragraphs of docs to explain a CLI, the CLI is probably badly designed.
  - Create a simple wrapper with a clean API and document that instead.

- **Size discipline**:
  - For serious monorepos, keep CLAUDE.md curated and roughly sized (e.g., tens of KB rather than hundreds).
  - Think of documentation “ad space” that teams must earn with conciseness.

### 3.5 Tuning CLAUDE.md

- Remember: CLAUDE.md is part of the model’s prompt.
- You can:
  - Rewrite sections, reorder priorities, and emphasize important rules.
  - Use cues like “IMPORTANT” or “YOU MUST” for high-priority instructions.
  - Run CLAUDE.md through a prompt improver.
  - Use the # shortcut to quickly convert inline instructions into persistent CLAUDE.md entries.

### 3.6 Team Practices

Treat CLAUDE.md as real infrastructure:

- Commit CLAUDE.md files and review them in PRs.
- Use conventional commits for doc changes.
- Tag major updates if needed.
- Use a parallel AGENTS.md when you want cross-tool compatibility.
- Onboard new engineers by:
  - Having Claude read CLAUDE.md.
  - Asking it to explain architecture, patterns, and workflows.

---

## 4. How to Use Claude Code: Workflows and Patterns

### 4.1 General Mindset & Iterations

Forget one-shot perfection:

- First attempt:
  - Mostly wrong, but builds context and reveals challenges.
- Second attempt:
  - Better, but still requires heavy review.
- Third attempt:
  - Usually a solid base to refine.

Your workflow should be: plan → generate → review → iterate → document.

---

### 4.2 Context Management: /context, /clear, /compact

- **/context**:
  - Shows how your context window is being used (like disk usage).
  - In large monorepos, a fresh session might cost 20k tokens just in baseline context.

- **/compact**:
  - Tries to compress conversation history.
  - Often opaque and not easy to trust for critical work.
  - Good to be cautious with it.

- **/clear + /catchup**:
  - Recommended simple reboot:
    - Use /clear to wipe conversation history.
    - Use a custom /catchup slash command to have Claude read all changed files in your current branch.

- **“Document & Clear” for large tasks**:
  - Have Claude:
    - Write a plan + current status into a markdown file (plan.md, decisions.md, etc.).
    - Then /clear.
    - Start a new conversation telling it to read that doc and continue.

---

### 4.3 Example Workflows

#### Workflow A: Explore → Plan → Code → Commit

1. **Explore**
   - Ask Claude to:
     - Read specific files or areas (“read logging.py”, or “read files related to authentication”).
   - Tell it explicitly: “Don’t write code yet, just read and summarize.”
   - This is a good time to use subagents for:
     - Fact-checking.
     - Investigating uncertainties.
     - Keeping the main context light.

2. **Plan**
   - Ask Claude to “think hard” or “ultrathink” and propose a plan:
     - Architecture changes.
     - File-by-file edits.
     - Test strategy.
   - If the plan is decent:
     - Ask it to write the plan into plan.md or a GitHub issue so you can revert or refer.

3. **Code**
   - Once you agree on the plan:
     - Ask Claude to implement it step by step.
     - Encourage it to verify and reflect as it goes.
   - You can insert explicit checkpoints:
     - “Stop after creating the API schema and show me the diff.”

4. **Commit**
   - When satisfied:
     - Ask Claude to clean up, stage changes, and create a PR.
     - Ask it to update README, changelog, or architecture docs as appropriate.

---

#### Workflow B: Test-Driven Development (Write Tests First)

1. **Write tests**:
   - Tell Claude:
     - You’re doing test-driven development.
     - It should write tests only, based on expected I/O and behavior.
     - It must not implement the feature yet.

2. **Run tests and confirm failure**:
   - Ask Claude to run the test suite and verify the new tests fail as expected.

3. **Commit tests**:
   - Once you’re happy with test quality:
     - Ask Claude to commit just the tests.

4. **Implement to pass tests**:
   - Ask Claude to implement code that passes the tests.
   - Explicitly tell it not to modify the tests.
   - Let it iterate:
     - Implement → run tests → fix → run again.
   - Optionally:
     - Use subagents to ensure it’s not overfitting to tests.

5. **Commit implementation**:
   - When tests are green and code looks good:
     - Ask Claude to commit the implementation.

---

#### Workflow C: Code → Screenshot → Iterate (UI/UX)

1. **Provide a visual target**:
   - Give Claude:
     - Design mocks (images, screen captures) or file paths to them.

2. **Implement UI**:
   - A typical prompt might say:
     - Implement a specific screen.
     - Match colors, typography, spacing, shadows, border-radius, layout.
     - Use your shared UI packages (e.g., shadcn/ui).
     - Only modify a specific page or component.
     - Hard-code data from JSON, no real backend yet.

3. **Capture and iterate**:
   - Ask Claude to:
     - Run end-to-end or screenshot tools (via MCP, e.g. Puppeteer, Playwright, iOS simulator).
     - Compare the result visually to the mock.
     - Refine until the UI closely matches.

4. **Commit**:
   - Once satisfied, have Claude clean up and commit.

---

### 4.4 Slash Commands and Custom Commands

- Use slash commands sparingly as shortcuts:
  - Examples:
    - /catchup: read all changed files in the current branch.
    - /pr: clean up, stage changes, and prepare a PR.

- Avoid building a huge library of complex, mandatory slash commands:
  - If people need to memorize a big custom command catalog, you’ve failed at making the agent intuitive.
  - Instead, improve CLAUDE.md and tooling so natural language prompts are sufficient.

- “Anything that can be repeated can be a command”:
  - E.g. bootstrapping an API endpoint, setting up a standard test harness, generating a changelog.

---

### 4.5 Git Worktrees and Parallel Claude Sessions

Git worktrees let you:

- Check out multiple branches simultaneously into separate directories.
- Run individual Claude sessions in each directory with:
  - Their own local context.
  - Their own CLAUDE.local.md hints.

Example usage in practice:

- One worktree for feature A.
- Another for feature B.
- Another for main or bugfix branch.

Benefits:

- Parallel development without context switching.
- Each Claude instance stays focused on one feature.
- Any mistakes are isolated within that worktree.

---

### 4.6 Hooks

Hooks are user-defined shell commands attached to specific lifecycle events:

- PreToolUse (before any tool).
- PostToolUse (after a successful tool call).
- Notification.
- Stop.
- Sub-agent Stop.

Patterns:

- **Block-at-Submit Hooks**:
  - Example: wrap git commit.
  - Before committing:
    - Run tests and create a pass marker file if success.
    - If the marker doesn’t exist, block the commit.
  - Forces Claude into a test-and-fix loop until the build is green.

- **Hint Hooks**:
  - Non-blocking hints when Claude is doing something suboptimal.
  - Example: if it runs a slow command, remind it there is a better CLI.

Avoid:

- Blocking mid-edit (block-at-write).
  - Interrupting Claude mid-plan confuses it.
  - Better to let it finish, then validate at commit time.

---

### 4.7 Claude Code SDK and GitHub Actions

- Use the **SDK** for:
  - Massive parallel refactors and migrations using simple scripts.
  - Building internal chat tools that embed Claude for non-engineers.
  - Quickly prototyping agent workflows before you invest in full frameworks.

- Use the **GitHub Action (GHA)** for:
  - PR-from-anywhere flows:
    - Slack/Jira/alerts trigger a GHA run.
    - GHA runs Claude Code to fix the issue or implement a small feature.
    - Produces a fully-tested PR.
  - Audited, sandboxed AI usage:
    - Full logs reviewed regularly.
    - Use data from logs to:
      - Improve CLAUDE.md.
      - Improve CLIs.
      - Fix recurring mistakes.

---

## 5. Different Models and What They’re Good For

### Claude (Sonnet, etc.)

- Strengths:
  - Great writing (documentation, specs, long explanations).
  - Excellent coding and refactors.
  - Large context window:
    - Good for big repos and multi-file reasoning.
  - Strong for UI/UX:
    - Can reason from mocks.
    - Good at design-sensitive changes.

- Weakness:
  - Still hallucination-prone without good context and guardrails.
  - Needs explicit memory structures (CLAUDE.md, docs) to avoid re-learning.

---

### ChatGPT

- Strengths:
  - Very solid at analysis, step-by-step reasoning, and general Q&A.
  - Good for:
    - Everyday “how do I X?” questions.
    - Quick research-oriented prompts.
    - Conceptual design discussions and brainstorming.

- Use cases:
  - Analysis and explanation rather than heavy, multi-file repo edits.
  - “What’s the tradeoff between A and B?” style questions.

---

### Gemini

- Strengths:
  - Very strong for:
    - Product Requirements Documents (PRDs).
    - High-level planning and structuring content.
  - Good code assistance (especially in CLI mode).
  - Has access to Google Search, which is useful for:
    - Market and competitive research.
    - Documentation lookup.
    - Recent changes in libraries and tools.

- Use cases:
  - Brainstorming products.
  - Writing structured PRDs and specs.
  - Breaking down requirements into user stories and sprints.

---

### Perplexity

- Strength:
  - Focused on web search with citations.
  - Good for quickly aggregating recent information and sources.

- Use cases:
  - Up-to-date data or news.
  - Quick research with references.

---

## 6. Concrete Workflow: Gemini for PRDs → Claude Code for Build

Putting it all together, here’s the workflow you use and recommend:

### 6.1 Step 1: Brainstorm and PRD Creation with Gemini

- Use Gemini to:
  - Brainstorm product ideas.
  - Write the initial PRD:
    - Goals and non-goals.
    - User stories.
    - Functional and non-functional requirements.
    - Success metrics.
    - Open questions.

- Benefits:
  - Gemini’s strength at long-context, structured writing makes it a great PRD generator.
  - You get a clear shared artifact to hand off to Claude.

---

### 6.2 Step 2: Break Down the PRD into Sprints and Stories

- Still in Gemini (or with help from Claude):
  - Break the PRD into:
    - Sprints.
    - Epics.
    - User stories.
    - Acceptance criteria.
  - Optionally:
    - Generate an initial task list or backlog.

- Result:
  - A structured, incremental plan you can give Claude Code.

---

### 6.3 Step 3: Bring the PRD into Claude Code and Plan

- In Claude Code:
  - Put the PRD (or its distilled version) into:
    - A file in the repo (e.g. docs/prd-finance-tracker.md).
    - Or into a component-specific doc file.
  - Ensure CLAUDE.md:
    - Points to the PRD.
    - Explains when and why to read it.
  - Start a session in Plan Mode and:
    - Ask Claude to read the PRD.
    - Ask it to “think hard” or “ultrathink” and propose a detailed implementation plan.

- Plan output:
  - High-level architecture.
  - List of features and milestones.
  - Proposed file and folder structure.
  - Testing strategy.
  - Security/privacy considerations.

---

### 6.4 Step 4: Create a TODO List and Working Docs

Ask Claude to translate the plan into persistent artifacts such as:

- plan.md:
  - Master checklist of milestones and tasks.
- architecture.md:
  - System design, key patterns, data flow diagrams.
- todo.md:
  - Immediate next tasks (short-term focus).
- decisions.md:
  - Key architectural decisions and tradeoffs.

These docs:

- Act as durable memory between sessions.
- Let you /clear conversations without losing progress.
- Help other team members ramp up.

---

### 6.5 Step 5: Implement Features Incrementally with Claude Code

Follow the Explore → Plan → Code → Commit workflow:

1. For each feature:
   - Decide scope small enough to fit comfortably within the context window.
   - Ask Claude to:
     - Re-read relevant sections of the PRD.
     - Re-read the relevant part of plan.md and architecture.md.

2. Work in small, well-bounded chats:
   - Scope each chat to one feature or subfeature.
   - Use Auto Mode when you want speed and trust your guardrails.
   - Use Default or Plan Mode when stakes are higher or complexity is larger.

3. Use TDD or visual workflows where appropriate:
   - If feature is heavily UI, use screenshot iteration.
   - If feature is logic-heavy, favor test-first flow.

4. Track and document:
   - After finishing a feature:
     - Update plan.md (checked tasks).
     - Update decisions.md with new architectural choices.
     - Ask Claude to add any persistent learnings into CLAUDE.md and CLAUDE.local.md.

---

### 6.6 Step 6: Continuous Documentation & Memory Updates

At the end of productive sessions:

- Run an “update memory” pattern:
  - Ask Claude:
    - What did we learn about the project that wasn’t known before?
    - What mistakes did it make that we corrected?
    - What new constraints, patterns, or gotchas did we discover?

- Then:
  - Have Claude:
    - Add shared insights into CLAUDE.md.
    - Add personal tips into CLAUDE.local.md.
    - Update component-specific CLAUDE.md files if relevant (e.g., ui-only rules in apps/ui/CLAUDE.md).

This creates a flywheel:

- Better CLAUDE.md and tooling → better first attempts → fewer corrections → more time for deeper work.

---

### 6.7 Step 7: Multi-Model Loop

Throughout the project:

- Use Gemini:
  - To revise and expand PRDs.
  - To brainstorm new features and improvements.
- Use Claude Code:
  - To translate those PRDs into code, tests, and documentation.
  - To manage implementation details and refactors.
- Optionally:
  - Use ChatGPT or Perplexity for conceptual explanations or quick research.
  - Bring new insights back into:
    - PRDs.
    - CLAUDE.md.
    - plan.md / architecture.md / decisions.md.

---

## Final Takeaways

- Agents are powerful, but they are not magical senior engineers.
- Claude Code becomes effective when:
  - You give it structured memory (CLAUDE.md).
  - You use clear workflows (plan → code → iterate → document).
  - You combine it with the right models (Gemini for PRDs, Claude for coding).
- The biggest leverage is not raw token count, but:
  - Persistent knowledge.
  - Thoughtful prompts.
  - Good tooling and guardrails.

This document is your foundation: start here, evolve your CLAUDE.md and workflows as you learn, and let the system continuously improve itself.
