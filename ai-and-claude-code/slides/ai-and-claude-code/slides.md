---
theme: seriph
background: https://images.unsplash.com/photo-1451187580459-43490279c0fa?w=1920
title: You're Absolutely Right!
info: |
    ## General Understanding of Agents and how to use them effectively in your project and
    development workflow
class: text-center
drawings:
    persist: false
transition: slide-left
mdc: true
---

# You're Absolutely Right!

Understanding AI Agents and How to Actually Use Them

<div class="abs-br m-6 text-sm opacity-50">
  Press Space to continue
</div>

---
layout: default
---

# What We'll Cover Today

<div class="grid grid-cols-3 gap-8 mt-8">

<div>
<div class="text-4xl mb-4">🎯</div>

### Setting Expectations

What agents are NOT, the "overly confident coding student," and why small steps win

</div>

<div>
<div class="text-4xl mb-4">🧰</div>

### The AI Toolbox

ChatGPT, Claude, Gemini, and Perplexity - what each one does best

</div>

<div>
<div class="text-4xl mb-4">💻</div>

### Claude Code Deep Dive

Features, modes, and the memory system (CLAUDE.md)

</div>

<div>
<div class="text-4xl mb-4">⚡</div>

### Skills & Agents

Built-in agents, custom skills, and subagents for tasks

</div>

<div>
<div class="text-4xl mb-4">🛠️</div>

### Real Workflows

Quick fixes, TDD, UI development, and refactoring patterns

</div>

<div>
<div class="text-4xl mb-4">🔥</div>

### The Ultimate Workflow

Gemini + Claude Code loop and continuous improvement

</div>

</div>

<div class="mt-12 text-center text-sm opacity-75">
~45 minutes • Questions at the end
</div>

---
layout: section
---

# Let's Be Honest About What AI Actually Is

---

# What AI Agents Are NOT

<div class="text-left space-y-6">

## ❌ NOT a Super Coder

AI can't architect complex systems, debug production issues, or make critical technical decisions

## ❌ NOT Production-Ready

It writes "draft zero" code that needs your expertise, review, and judgment

## ❌ NOT Magic

You can't say "Build me a CRM" and get working software. It doesn't work that way.

</div>

<div class="mt-10 p-4 bg-blue-500/10 border border-blue-500/50 rounded text-sm">
<strong>Real talk:</strong> Yes, AI will change how we work. But it's not replacing engineers—it's changing what engineering looks like. Think "calculator for math" not "robots taking jobs."
</div>

---

# So What ARE They?

<div class="text-center mb-8">

## Think of them as that overly confident coding student

</div>

You know the one:

- Read all of Stack Overflow over a weekend ✅
- Will confidently use APIs that don't exist ✅
- Insists they're right even when they're wrong ✅
- Tries to help even when they have no idea ✅
- Needs adult supervision ✅

<div class="mt-8 p-4 bg-orange-500/10 border border-orange-500/50 rounded">
<strong>But here's the thing:</strong> That student can read and write code 24/7, never gets tired, and can try 100 different approaches without getting frustrated
</div>

---

# The Golden Rule: Small Steps Win

<div class="grid grid-cols-2 gap-8">
<div>

## ❌ Don't Do This:

"Build me a complete authentication system with OAuth, 2FA, password reset, and email verification"

<div class="text-red-500 text-2xl mt-4">↓</div>
<div class="text-red-500">Disaster</div>

</div>
<div>

## ✅ Do This Instead:

1. "Create the user model"
2. "Add password hashing"
3. "Implement login endpoint"
4. "Add JWT generation"
5. "Test the login flow"

<div class="text-green-500 text-2xl mt-4">↓</div>
<div class="text-green-500">Success</div>

</div>
</div>

<div class="mt-8 text-center text-xl font-bold">
Iterate in small, reviewable chunks. Always.
</div>

---
layout: section
---

# The AI Model Lineup

Who's good at what?

---

# ChatGPT: Your Research Buddy

<div class="grid grid-cols-2 gap-8">
<div>

## Best For:

- Quick questions and explanations
- "How does X work?"
- Step-by-step reasoning
- Conceptual discussions
- Tradeoff analysis

## Not Great For:

- Multi-file code editing
- Long coding sessions
- Maintaining context over time

</div>
<div>

## Common Usage:

Most developers use ChatGPT's web interface for:

- Understanding concepts
- Getting unstuck
- Rubber ducking ideas
- Quick code snippets
- Syntax questions

<div class="mt-4 p-3 bg-orange-500/10 border border-orange-500/30 rounded text-sm">
<strong>The limitation:</strong> Copy-pasting between browser and editor doesn't scale for real development
</div>

</div>
</div>

---

# Claude/Anthropic: The Code Whisperer

<div class="grid grid-cols-2 gap-8">
<div>

## Strengths:

- Excellent at writing and editing code
- Great at refactoring
- Strong documentation writing
- Can reason across many files

## Weaknesses:

- Still hallucinates without guidance
- Needs good memory setup
- Requires iteration

</div>
<div>

## Use Cases:

- Complex refactors
- Documentation generation
- Multi-file edits
- Understanding large codebases
- Test generation

<div class="mt-8 p-4 bg-blue-500/10 border border-blue-500/50 rounded">
<strong>Why we're focusing on Claude Code:</strong> It's Claude + your actual codebase + powerful workflows
</div>

</div>
</div>

---

# Gemini: The PRD Machine

<div class="grid grid-cols-2 gap-8">
<div>

## Strengths:

- Excellent at structured documents
- Great PRD generation
- High-level planning
- Google Search integration
- Market research

## Use Cases:

- Writing Product Requirements
- Breaking down features
- Sprint planning
- Research and documentation
- Competitive analysis

</div>

</div>

---

# Perplexity: The Quick Researcher

<div class="grid grid-cols-2 gap-6">
<div>

## What It Does:

- Web search with citations
- Recent information
- Quick fact-checking
- Source aggregation

</div>
<div>

## When To Use:

- "What's the latest version of...?"
- "How did company X solve Y?"
- Quick research with sources
- Checking current best practices

</div>
</div>

---
layout: section
---

# Now, Let's Talk Claude Code

The main event

---

# What Is Claude Code?


<div class="mt-10 grid grid-cols-3 gap-4">
<div>

### Not a Chat Window

It's a CLI that lives in your project

</div>
<div>

### Not Copy-Paste

It directly reads and writes your files

</div>
<div>

### Not Stateless

It remembers your patterns and preferences

</div>
</div>

<div class="mt-12 text-center">

Think of it as: **Claude living inside your codebase**

</div>

---

# Claude Code Features: The Highlights

- 🧠 **Memory System** - CLAUDE.md files that persist knowledge across sessions
- 🎯 **Built-in Subagents** - Plan, Explore, and General agents for specific tasks
- 🛠️ **Tools** - File operations, Bash, Git, test runners, and more
- 🔌 **MCP (Model Context Protocol)** - Connect to Playwright, databases, APIs
- ⚡ **Skills** - Create custom reusable workflows
- 📝 **Slash Commands** - Build shortcuts for repeated tasks
- 🔒 **Hooks** - Lifecycle events for validation and automation

---
layout: section
---

# Memory: Teaching Claude Your Codebase

The secret sauce

---

# The Memory Hierarchy

<div class="grid grid-cols-2 gap-6 mt-4">

<div>

### 📁 Global User Memory

**`~/.claude/CLAUDE.md`**

Your personal preferences

- Coding style (tabs vs spaces)
- Commit message format
- Cross-project settings

</div>

<div>

### 📁 Project Memory

**`./CLAUDE.md`** ⭐

Team's shared knowledge (Git)

- Architecture & patterns
- Testing strategy
- Security rules

</div>

<div>

### 📁 Local Memory

**`./CLAUDE.local.md`**

Private notes (gitignored)

- Debug tricks

</div>

<div>

### 📁 Component Memory

**`apps/ui/CLAUDE.md`**

Component-specific rules

- Only loads in that directory
- Keeps context focused

</div>

</div>

---

# What Goes In CLAUDE.md?

<div class="grid grid-cols-2 gap-8">
<div>

## ✅ Do Include:

- What Claude keeps getting wrong
- "Use X instead of Y"
- Where to find documentation
- Testing requirements
- Security rules
- Common gotchas

</div>
<div>

## ❌ Don't Include:

- Entire documentation files
- Obvious things
- Pure "never" rules
- Massive embedded guides

</div>
</div>

<div class="mt-8 p-4 bg-blue-500/10 border border-blue-500/50 rounded">
<strong>Think of it as:</strong> Training notes for your overly confident coding student
</div>

<div class="mt-4 text-sm opacity-75 text-center">
Example: "When working with our API, always check docs/api-guide.md for rate limiting rules"
</div>

---

# Update Memory Automatically

<div class="text-center mb-6 text-lg opacity-90">
At the end of each session, create a memory update loop
</div>

<div class="grid grid-cols-3 gap-6">

<div>
<div class="text-3xl mb-3">💭</div>

### Ask Claude

- "What did you learn?"
- "What mistakes did we fix?"
- "What patterns emerged?"

</div>

<div>
<div class="text-3xl mb-3">💾</div>

### Have Claude Update

- Team → `CLAUDE.md`
- Personal → `CLAUDE.local.md`
- Component-specific as needed

</div>

<div>
<div class="text-3xl mb-3">🔄</div>

### The Result

- Claude improves over time
- You stop re-teaching
- Knowledge compounds

</div>

</div>

---
layout: section
---

# Skills, Agents, and Prompts

Building your AI toolkit

---

# Skills: Reusable Workflows

<div class="grid grid-cols-2 gap-12">
<div>

## What Are Skills?

Skills are like custom commands Claude can invoke automatically when relevant

**Use them for:**

- Repetitive workflows
- Best practices enforcement
- Process automation
- Team standards

</div>
<div>

## Example: Brainstorming Skill

```markdown
---
name: brainstorming
description: Use when refining ideas
---

When the user has a rough idea:

1. Ask clarifying questions
2. Explore alternatives
3. Identify tradeoffs
4. Document decisions
5. Create action items

Don't jump straight to code!
```

</div>
</div>

---

# Built-in Agents

<div class="text-center mb-8 text-lg opacity-90">
These are specialized subagents for different types of tasks
</div>

<div class="grid grid-cols-3 gap-8">

<div class="text-center">

### 🔍 Explore

<div class="text-left">

**When:** Need to understand code

**Does:**

- Fast codebase exploration
- Answers architecture questions
- Finds files and patterns

</div>

</div>

<div class="text-center">

### 📋 Plan

<div class="text-left">

**When:** Building new features

**Does:**

- Extended thinking
- Strategic planning
- Implementation plans

</div>

</div>

<div class="text-center">

### 🛠️ General

<div class="text-left">

**When:** Standard tasks

**Does:**

- Multi-step work
- Research + implementation
- General purpose coding

</div>

</div>

</div>

---

# Subagents: Tasks, Not Roles

<div class="grid grid-cols-2 gap-8">
<div>

## ❌ Don't Think: "Roles"

- "Backend developer agent"
- "Frontend developer agent"
- "DevOps agent"

This doesn't work well

</div>
<div>

## ✅ Think: "Tasks"

- "Research how we handle auth"
- "Test this specific feature"
- "Fix these 5 TypeScript errors"
- "Document this component"

Focused, specific, bounded

</div>
</div>

<div class="mt-8 p-4 bg-orange-500/10 border border-orange-500/50 rounded">
<strong>Key insight:</strong> Subagents keep your main chat clean and focused. Spawn them for specific research or tasks, then come back to your main work.
</div>

---
layout: section
---

# Workflows: How To Actually Use This

Real patterns you can use tomorrow


---

# Workflow A: Explore → Plan → Code → Commit

<div class="grid grid-cols-4 gap-4 text-sm">

<div>

### 1. Explore

- Ask Claude to read specific files
- "Don't write code yet, just read and summarize"
- Use subagents for fact-checking

</div>

<div>

### 2. Plan

- Ask Claude to "think hard" and propose a plan
- Architecture changes
- File-by-file edits
- Write plan to plan.md

</div>

<div>

### 3. Code

- Implement step by step
- Insert checkpoints
- "Stop after creating the API schema"

</div>

<div>

### 4. Commit

- Clean up and stage changes
- Create PR
- Update docs as needed

</div>

</div>

---

# Workflow B: Test-Driven Development

<div class="grid grid-cols-5 gap-4 text-sm">

<div>

### 1. Write Tests

- Tell Claude you're doing TDD
- Write tests only
- Based on expected behavior
- Don't implement yet

</div>

<div>

### 2. Run & Fail

- Run test suite
- Verify new tests fail as expected

</div>

<div>

### 3. Commit Tests

- Happy with test quality?
- Commit just the tests

</div>

<div>

### 4. Implement

- Code that passes tests
- Don't modify tests
- Iterate: code → test → fix

</div>

<div>

### 5. Commit Code

- Tests green?
- Code looks good?
- Commit implementation

</div>

</div>

---

# Workflow C: Code → Screenshot → Iterate

<div class="grid grid-cols-4 gap-4 text-sm">

<div>

### 1. Visual Target

- Give Claude design mocks
- Images or screen captures
- "Build this login form"

</div>

<div>

### 2. Implement UI

- Match colors, typography, spacing
- Use shared UI packages (shadcn/ui)
- Hard-code data from JSON

</div>

<div>

### 3. Screenshot

- Run tools via MCP (Playwright)
- Compare result to mock
- Refine until it matches

</div>

<div>

### 4. Commit

- Satisfied with result?
- Clean up code
- Commit changes

</div>

</div>

---
layout: section
---

# The Ultimate Workflow

Gemini + Claude Code = 🔥

---

# Step 1: Brainstorm in Gemini

<div class="text-center mb-4 text-lg opacity-90">
Start with Gemini for high-level planning
</div>

<div class="grid grid-cols-2 gap-8 text-sm">

<div>

### Write a PRD

- Goals and non-goals
- User stories
- Functional requirements
- Success metrics
- Open questions

</div>

<div>

### Break It Down

- Sprints
- Epics
- User stories with acceptance criteria

</div>

</div>

<div class="mt-8 p-4 bg-blue-500/10 border border-blue-500/50 rounded text-sm">
<strong>Why Gemini?</strong> Excellent at structured documents and has Google Search for research
</div>

---

# Step 2: Move to Claude Code

<div class="text-center mb-6 text-lg opacity-90">
Bring the PRD into your project
</div>

<div class="grid grid-cols-3 gap-6 text-sm">

<div>

### Setup

- Save PRD to `docs/prd-new-feature.md`
- Update `CLAUDE.md` to reference it
- "Read this PRD when working on feature X"

</div>

<div>

### Plan Mode

- Start Claude Code in Plan mode
- "Think hard about implementation"
- Claude creates:
  - `plan.md`
  - `architecture.md`
  - `decisions.md`

</div>

<div>

### Ready

- Clear roadmap
- Claude knows context
- Time to implement

</div>

</div>

---

# Step 3: Build Incrementally

<div class="text-center mb-6 text-lg opacity-90">
Small chunks, constant progress
</div>

<div class="grid grid-cols-2 gap-8 text-sm">

<div>

### Feature by Feature

- Pick one small feature from plan.md
- "Implement user registration (no OAuth yet)"

### Choose Your Mode

- Default for learning/high-stakes
- Auto when you trust the pattern
- Plan for complex pieces

</div>

<div>

### Use Right Workflow

- TDD for logic-heavy features
- Screenshot iteration for UI
- Refactor workflow for cleanup

### Update Docs

- Check off tasks in plan.md
- Update decisions.md with changes
- Add learnings to CLAUDE.md

</div>

</div>

---

# Step 4: Loop Back to Gemini

<div class="text-center mb-6 text-lg opacity-90">
After implementing, refine the plan
</div>

<div class="grid grid-cols-2 gap-8 text-sm">

<div>

### Back to Gemini

- Take what you learned
- Update PRD with new insights
- Plan the next sprint
- Adjust requirements based on reality

</div>

<div>

### Back to Claude Code

- Implement the next batch
- Update docs
- Commit progress
- Repeat the cycle

</div>

</div>

<div class="mt-8 p-4 bg-green-500/10 border border-green-500/50 rounded text-sm">
<strong>The magic:</strong> Gemini for strategy, Claude Code for execution, constant iteration
</div>

---

# Step 5: Continuous Improvement

<div class="text-center mb-6 text-lg opacity-90">
Make the system better over time
</div>

<div class="grid grid-cols-3 gap-6 text-sm">

<div>

### End of Session

- "What did we learn?"
- "What patterns should we remember?"
- "What did you get wrong that we fixed?"

</div>

<div>

### Update Memory

- Shared learnings → `CLAUDE.md`
- Personal tips → `CLAUDE.local.md`
- Component rules → `apps/X/CLAUDE.md`

</div>

<div>

### The Flywheel

- Better docs → Better first attempts
- Better attempts → Less rework
- Less rework → More time for features

</div>

</div>

---
layout: section
---

# Key Takeaways

---

# What We Learned

<div class="grid grid-cols-2 gap-8 text-sm">


### 1. AI agents are overly confident coding students

- They need guidance, review, and iteration
- Small steps win every time



### 2. Use the right tool for the job

- ChatGPT for quick questions
- Gemini for PRDs and planning
- Claude Code for building
- Perplexity for research



### 3. Memory is your superpower

- CLAUDE.md makes Claude better over time
- Stop re-teaching the same things
- Let your team benefit from collective knowledge



### 4. Workflows > Features

- Don't just use Claude Code randomly
- Follow patterns: TDD, UI iteration, refactoring
- Build your own workflows that work for your team


</div>

---

# The Real Secret

<div class="flex flex-col items-center justify-center h-90 space-y-12 mt-1">

<div class="text-4xl font-bold opacity-80">
It's not about the AI
</div>

<div class="text-5xl font-bold text-blue-400">
It's about you + AI
</div>

<div class="text-xl opacity-70 mt-8">
The best developers know when to iterate, when to stop, and when to just write it themselves
</div>

</div>

---

# Thanks!
