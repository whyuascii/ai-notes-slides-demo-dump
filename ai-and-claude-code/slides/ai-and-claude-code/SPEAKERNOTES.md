# Speaker Notes: You're Absolutely Right!

## Slide 1: Title Slide
**You're Absolutely Right! - Understanding AI Agents and How to Actually Use Them**

**Opening:**
- Welcome everyone! The title is a little tongue-in-cheek
- It's a callback to how AI agents often respond - they're always agreeing with you, even when they're about to give you completely wrong code
- Today we're going to talk about AI agents, what they actually are (spoiler: not magic), and how to use them without losing your mind

---

## Slide 2: What We'll Cover Today

**Notes:**
Here's our roadmap for the next 45 minutes - I've broken it into 6 parts:

**Part 1: Setting Expectations**
- We'll bust some myths about AI agents
- I'll give you a better analogy than "AI assistant"
- Show you why small iteration beats big bang approaches

**Part 2: The AI Toolbox**
- Quick tour of ChatGPT, Claude, Gemini, and Perplexity
- What each one is actually good at
- When to use which tool

**Part 3: Claude Code**
- This is where we go deep
- What it is, why it matters
- Features, modes, and the memory system

**Part 4: Skills & Agents**
- Built-in agents and how to use them
- Creating custom skills
- Understanding subagents

**Part 5: Real Workflows**
- Four practical workflows you can use tomorrow
- Quick fixes, TDD, UI development, and refactoring
- Real examples, not theory

**Part 6: Putting It Together**
- The ultimate workflow combining Gemini and Claude Code
- How to create a continuous improvement loop
- Key takeaways to remember

We'll have questions at the end, and this should take about 45 minutes total.

---

## Slide 3: Let's Be Honest About What AI Actually Is

**Notes:**
- I know the elephant in the room: everyone's worried about AI and job security
- I'm new here, teaching you about AI, so let me be straight with you
- I'm not going to tell you AI won't change things - it will
- But I also need to set realistic expectations about what AI actually can and can't do
- Let's cut through the hype and talk honestly

---

## Slide 4: What AI Agents Are NOT

**Notes:**
Let me be very clear about what AI is NOT:

**NOT a Super Coder:**
- AI can't architect a complex distributed system
- It can't debug that weird production issue where the symptoms don't match any known pattern
- It can't make critical technical decisions like "should we migrate to microservices?"
- It doesn't have the experience or judgment that you have
- Think about the hardest technical problem you've solved - AI couldn't do that

**NOT Production-Ready:**
- What AI writes is "draft zero" code
- It's a starting point, not the finish line
- It needs YOUR expertise to review
- It needs YOUR judgment to validate
- It needs YOUR understanding to maintain
- You wouldn't ship it without review any more than you'd ship unreviewed code from a junior dev

**NOT Magic:**
- You can't say "Build me a CRM system" and get working software
- It doesn't work that way
- It needs clear, small, specific instructions
- Just like working with any developer

**Real talk (the honest part):**
Yes, AI will change how we work. That's not a maybe, that's happening now.

But here's what it actually means:
- AI is like a calculator for math - it didn't replace mathematicians, it changed what they focus on
- You'll spend less time on boilerplate and more time on architecture
- You'll spend less time Googling syntax and more time solving actual problems
- Engineers who learn to work WITH AI will be more productive than those who don't

But "replacing engineers"? No. Because:
- AI doesn't understand your business context
- AI doesn't know your users
- AI can't navigate organizational politics
- AI can't make judgment calls
- AI can't look at a bug report and intuitively know what's actually broken

You're not being replaced. Your job is evolving. And this talk is about how to evolve with it.

---

## Slide 5: So What ARE They?

**Notes:**
Okay, so if they're not magic replacements for developers, what ARE they?

**Here's my favorite analogy:**

Think of them as that overly confident coding student. You know the one I'm talking about:

- **Read all of Stack Overflow over a weekend**: They've seen a LOT of code, just like AI has
- **Will confidently use APIs that don't exist**: This is the hallucination problem. They'll make up method names that sound right but don't exist
- **Insists they're right even when they're wrong**: They don't have self-doubt. They'll confidently give you broken code.
- **Tries to help even when they have no idea**: They want to be helpful, even when they shouldn't
- **Needs adult supervision**: This is you. You're the adult.

**But here's the thing:**
That student has one huge advantage - they can work 24/7, never get tired, never get frustrated, and can try 100 different approaches without complaining.

That's actually really valuable when you harness it correctly.

---

## Slide 6: The Golden Rule - Small Steps Win

**Notes:**
This is probably the single most important slide in the whole presentation.

**Don't do this (left side):**
"Build me a complete authentication system with OAuth, 2FA, password reset, and email verification"

What happens? Disaster. The AI will:
- Hallucinate APIs
- Mix deprecated and current patterns
- Create code that almost works but is broken in subtle ways
- Generate something you can't debug because it's too much at once

**Do this instead (right side):**
Break it into tiny, reviewable pieces:
1. Create the user model
2. Add password hashing
3. Implement login endpoint
4. Add JWT generation
5. Test the login flow

Each step is:
- Small enough to review
- Testable
- Fixable if wrong

**The mantra: Iterate in small, reviewable chunks. Always.**

This is how you avoid spending 3 hours debugging AI-generated code.

---

## Slide 7: The AI Model Lineup

**Notes:**
- Okay, now that we've set expectations, let's talk about the actual tools
- Different AI models are good at different things
- Using the right tool for the job is crucial
- We'll go through ChatGPT, Claude, Gemini, and Perplexity

---

## Slide 8: ChatGPT - Your Research Buddy

**Notes:**

**Best for:**
- This is probably what most of you are using right now
- Great for quick questions: "How does X work?"
- Excellent at explaining concepts
- Good for rubber ducking - just talking through a problem
- Tradeoff analysis

**Not great for:**
- Multi-file editing
- Long coding sessions
- Maintaining context over hours of work

**Common usage:**
Most developers are already using ChatGPT's web interface. And that's perfectly fine for certain tasks:
- Understanding new concepts
- Getting unstuck on a problem
- Rubber ducking ideas
- Getting quick code snippets
- Asking syntax questions

**The limitation:**
The problem is the copy-paste workflow. You ask ChatGPT a question in the browser, it gives you code, you copy it, paste it into your editor, test it, realize it's not quite right, go back to the browser, explain the problem, copy the new code...

This works for small, isolated questions. But it doesn't scale for real development work where you're working across multiple files, need context from your actual codebase, and want to iterate quickly.

That's where Claude Code comes in - and that's where we're heading next.

---

## Slide 9: Claude/Anthropic - The Code Whisperer

**Notes:**

**Strengths:**
- Claude is really good at writing and editing code
- Has a huge context window - can see a LOT of your codebase at once
- Great at refactoring
- Writes excellent documentation
- Can reason across many files

**Weaknesses:**
- Still hallucinates without good guidance
- Needs a good memory system (we'll talk about CLAUDE.md later)
- Requires iteration - not one-shot perfect

**Use cases:**
- Complex refactors
- Documentation
- Multi-file edits
- Understanding large codebases
- Test generation

**Why we're focusing on Claude Code:**
It's Claude, but it lives inside your actual codebase. It can read and write your files directly. It has memory. It has workflows.

This is the main event.

---

## Slide 10: Gemini - The PRD Machine

**Notes:**

**Strengths:**
- Gemini is EXCELLENT at writing structured documents
- Specifically Product Requirements Documents (PRDs)
- Great at high-level planning
- Has Google Search built in, so it can do research
- Good for market research and competitive analysis

**Use cases:**
- Writing PRDs
- Breaking down features into stories
- Sprint planning
- Documentation
- Research

**The Gemini → Claude Code flow:**
This is actually really powerful:
1. Start in Gemini - brainstorm, create PRD, define requirements
2. Move to Claude Code - implement the features
3. Loop back to Gemini - refine based on what you learned

We'll cover this workflow in detail at the end.

---

## Slide 11: Perplexity - The Quick Researcher

**Notes:**

**What it does:**
- Web search with actual citations
- Recent information (not limited by training cutoff)
- Quick fact-checking
- Aggregates sources

**When to use:**
- "What's the latest version of React?"
- "How did Airbnb solve their scaling problem?"
- Quick research with sources
- Checking current best practices

**Pro tip:**
Use the right tool for the job.
- Don't use Claude Code to research market trends
- Don't use Perplexity to write code
- Don't use ChatGPT to generate PRDs

Each tool has its lane. Stay in your lane.

---

## Slide 12: Now, Let's Talk Claude Code

**Notes:**
- Alright, we've set expectations, we've covered the model landscape
- Now let's get into the meat of this: Claude Code
- This is where theory meets practice

---

## Slide 13: What Is Claude Code?

**Notes:**

**Simple formula:**
Claude + Direct File Access + Git + Memory + Workflows

**What this means:**

**Not a chat window:**
- It's a CLI that lives in your project directory
- You don't copy-paste code. It reads and writes files directly.

**Not copy-paste:**
- It can edit files in place
- Run commands
- Check Git status
- Run tests

**Not stateless:**
- It remembers your patterns through CLAUDE.md files
- It learns from your corrections
- It gets better over time

**Think of it as: Claude living inside your codebase**

That's the mental model. Not a separate chat window. Not a copilot that suggests single lines. It's Claude, working directly with your project.

---

## Slide 14: Claude Code Features - The Highlights

**Notes:**
Here's a quick overview of what Claude Code can do - we'll go deeper on many of these:

**Memory System:**
- CLAUDE.md files that persist knowledge across sessions
- This is what makes Claude get smarter over time
- We'll spend a lot of time on this because it's crucial

**Built-in Subagents:**
- Plan, Explore, and General purpose agents
- These are specialized subagents for different types of tasks
- Think of them as calling in specialists for specific jobs

**Tools:**
- File operations, Bash commands, Git, test runners
- Claude can directly interact with your development environment

**MCP (Model Context Protocol):**
- Connect Claude to external systems like Playwright, databases, APIs
- This is how you extend Claude's capabilities

**Skills:**
- Create custom reusable workflows
- We'll show you examples like brainstorming and QA skills

**Slash Commands:**
- Build shortcuts for tasks you repeat often

**Hooks:**
- Lifecycle events for validation and automation
- Like pre-commit hooks but for Claude actions

Don't worry if this seems like a lot - we'll focus on the most important ones.

---

## Slide 15: Memory - Teaching Claude Your Codebase

**Notes:**
- This is the secret sauce
- This is what makes Claude Code different from just using ChatGPT
- Memory is what lets Claude get better over time instead of making the same mistakes every session

---

## Slide 16: The Memory Hierarchy

**Notes:**
Claude uses a hierarchical memory system with four levels:

**Global User Memory: `~/.claude/CLAUDE.md`**
- Your personal preferences across ALL projects
- Things like: tabs vs spaces, commit message format, personal style
- Only you see this file

**Project Memory: `./CLAUDE.md`**
- THE MOST IMPORTANT ONE
- Checked into Git, shared with the team
- Contains: architecture decisions, patterns, conventions, testing strategy
- This is what makes Claude understand your specific codebase

**Local Memory: `./CLAUDE.local.md`**
- Gitignored, private to you
- Your sandbox URLs, debug tricks, WIP notes
- Keep your messy notes here

**Component Memory: `apps/ui/CLAUDE.md`**
- Component-specific rules
- Only loaded when you're working in that directory
- Keeps context focused

**How it works:**
When Claude runs, it loads all CLAUDE.md files from your current directory up to the root, plus your global file. So it always has the right context.

---

## Slide 17: What Goes In CLAUDE.md?

**Notes:**

**Do include:**
- What Claude keeps getting wrong ("You keep using the old API, use the new one")
- "Use X instead of Y" guidance
- Where to find documentation
- Testing requirements
- Security rules
- Common gotchas specific to your codebase

**Don't include:**
- Entire documentation files (just point to them)
- Obvious things
- Pure "never" rules without context
- Massive embedded guides

**Think of it as:**
Training notes for your overly confident coding student.

**Example:**
"When working with our API, always check docs/api-guide.md for rate limiting rules. We've hit rate limits in production twice because this was ignored."

That's specific, actionable, and explains WHY.

---

## Slide 18: Update Memory Automatically

**Notes:**
Here's a pattern that will change how you work:

**At the end of good sessions, ask Claude:**
- "What did you learn about this codebase today?"
- "What mistakes did you make that we corrected?"
- "What patterns should you remember for next time?"

**Then have Claude:**
- Add shared learnings to CLAUDE.md
- Add your personal tips to CLAUDE.local.md
- Update component docs if needed

**The result:**
- Claude gets better over time
- You stop re-teaching the same things every session
- New team members can ask Claude for context instead of reading docs

This is the flywheel. Better memory → better code → less rework → more time for real features.

---

## Slide 19: Skills, Agents, and Prompts

**Notes:**
- Now let's talk about how to extend Claude Code with custom behaviors
- Skills are reusable workflows
- Agents are built-in modes
- Let's look at examples

---

## Slide 20: Skills - Reusable Workflows

**Notes:**
Skills are like custom commands that Claude can invoke automatically.

**What Are Skills?**
Skills are custom workflows that Claude can invoke automatically when it detects relevant situations.

Think of them as reusable processes that you define once and Claude applies when appropriate.

**Use them for:**
- Repetitive workflows (like code review checklists)
- Best practices enforcement (like security checks)
- Process automation (like documentation generation)
- Team standards (like commit message formats)

**Example: Brainstorming Skill**
When the user has a rough idea, this skill tells Claude to:
1. Ask clarifying questions first
2. Explore alternatives
3. Identify tradeoffs
4. Document decisions
5. Create action items

And crucially: **Don't jump straight to code!**

This prevents Claude from implementing half-baked ideas. Instead, it forces a thoughtful design process first.

**Key insight:**
Skills formalize your workflows so you don't have to explain them every time. Write it once, Claude applies it automatically when relevant.

---

## Slide 21: Built-in Agents

**Notes:**
Claude Code comes with three built-in agent types:

**Explore Agent:**
"Go figure out how authentication works in this codebase"
- Fast codebase exploration
- Answers architectural questions
- Finds files and patterns
- Use this for reconnaissance

**Plan Agent:**
"Think hard about how to implement OAuth"
- Extended thinking before coding
- Strategic planning
- Proposes implementation plans
- Use this for complex features

**General Agent:**
"Fix all the TypeScript errors"
- Multi-step tasks
- Research + implementation
- General purpose work
- Use this for straightforward work

**When to use which:**
- Explore when you don't understand something
- Plan when you're about to build something complex
- General for everything else

---

## Slide 22: Subagents - Tasks, Not Roles

**Notes:**
This is important to get right.

**Don't think of subagents as roles:**
- ❌ "Backend developer agent"
- ❌ "Frontend developer agent"
- ❌ "DevOps agent"

This doesn't work. AI isn't good at broad, role-based work.

**Think of them as specific tasks:**
- ✅ "Research how we handle auth"
- ✅ "Test this specific feature"
- ✅ "Fix these 5 TypeScript errors"
- ✅ "Document this component"

Focused. Specific. Bounded.

**Key insight:**
Subagents keep your main chat clean and focused. You spawn them for specific research or tasks, they do their work, they report back, and you continue your main work.

It's like pair programming where you occasionally send your pair off to research something while you keep coding.

---

## Slide 23: Workflows - How To Actually Use This

**Notes:**
- Okay, enough theory
- Let's talk about real workflows you can use
- I'm going to give you three core workflows
- Then we'll cover the ultimate workflow combining Gemini and Claude Code

---

## Slide 24: Workflow A - Explore → Plan → Code → Commit

**Notes:**
This is the classic full-cycle workflow for most development tasks.

**The workflow:**

1. **Explore**
   - Ask Claude to read specific files or areas: "read logging.py" or "read files related to authentication"
   - Tell it explicitly: "Don't write code yet, just read and summarize"
   - Use subagents for fact-checking and investigating uncertainties
   - Keeps the main context light

2. **Plan**
   - Ask Claude to "think hard" or "ultrathink" and propose a plan
   - Get architecture changes, file-by-file edits, test strategy
   - Ask it to write the plan into plan.md or a GitHub issue so you can revert or refer

3. **Code**
   - Once you agree on the plan, ask Claude to implement step by step
   - Encourage it to verify and reflect as it goes
   - Insert explicit checkpoints: "Stop after creating the API schema and show me the diff"

4. **Commit**
   - When satisfied, ask Claude to clean up, stage changes, and create a PR
   - Update README, changelog, or architecture docs as appropriate

**Key point:** This workflow forces you to understand before coding, and keeps you in control with checkpoints.

---

## Slide 25: Workflow B - Test-Driven Development

**Notes:**
When you want to do it right from the start.

**The workflow:**

1. **Write tests**
   - Tell Claude you're doing TDD
   - Write tests only, based on expected I/O and behavior
   - Must not implement the feature yet

2. **Run tests and confirm failure**
   - Run test suite and verify new tests fail as expected
   - This proves your tests actually test something

3. **Commit tests**
   - Once happy with test quality, commit just the tests

4. **Implement to pass tests**
   - Implement code that passes the tests
   - Explicitly tell it not to modify the tests
   - Let it iterate: implement → run tests → fix → run again
   - Optionally use subagents to ensure it's not overfitting

5. **Commit implementation**
   - When tests are green and code looks good, commit

**Why this works:**
- Tests first ensures you're testing actual behavior, not implementation details
- Separate commits means you can revert just the implementation if needed
- Forces clear thinking about interfaces before diving into code

---

## Slide 26: Workflow C - Code → Screenshot → Iterate

**Notes:**
Perfect for UI/UX work where visual feedback is critical.

**The workflow:**

1. **Provide a visual target**
   - Give Claude design mocks, images, or screen captures

2. **Implement UI**
   - Typical prompt: "Implement this screen, match colors, typography, spacing, shadows, border-radius, layout"
   - "Use our shared UI packages (e.g., shadcn/ui)"
   - "Only modify this specific page or component"
   - "Hard-code data from JSON, no real backend yet"

3. **Capture and iterate**
   - Ask Claude to run screenshot tools via MCP (Puppeteer, Playwright, iOS simulator)
   - Compare result visually to the mock
   - Refine until UI closely matches

4. **Commit**
   - Once satisfied, clean up and commit

**Why this works:**
- Visual feedback loop is incredibly fast
- Claude is good at matching designs when given clear references
- Hard-coding data first lets you focus purely on the visual layer

This is especially powerful with the Playwright MCP integration.

---

## Slide 27: The Ultimate Workflow

**Notes:**
- Okay, we've covered three core workflows
- Now let's put it all together with the ultimate workflow
- This combines Gemini for planning and Claude Code for execution
- This is how you build actual features end-to-end

---

## Slide 28: Step 1 - Brainstorm in Gemini

**Notes:**
**Start with Gemini for high-level planning:**

- Brainstorm the feature idea
- Write a full PRD (Product Requirements Document):
  - Goals and non-goals
  - User stories
  - Functional and non-functional requirements
  - Success metrics
  - Open questions

- Break it down into sprints, epics, user stories with acceptance criteria

**Why Gemini?**
It's excellent at structured documents and has Google Search for research. It's better at this than Claude.

You get a clear, shared artifact that you can hand off to Claude Code for implementation.

---

## Slide 29: Step 2 - Move to Claude Code

**Notes:**
**Bring the PRD into your project:**

**Setup:**
- Save the PRD to `docs/prd-new-feature.md` in your repo
- Update `CLAUDE.md` to reference it: "Read this PRD when working on feature X"

**Plan Mode:**
- Start Claude Code in Plan mode
- "Read the PRD and think hard about implementation"
- Claude will create:
  - `plan.md` - Implementation checklist
  - `architecture.md` - Technical design
  - `decisions.md` - Key choices and tradeoffs

**Ready to build:**
- You now have a clear roadmap
- Claude knows the context
- Time to start implementing

---

## Slide 30: Step 3 - Build Incrementally

**Notes:**
**Small chunks, constant progress:**

**Feature by feature:**
- Pick one small feature from plan.md
- "Implement user registration (no OAuth yet)"

**Choose your mode:**
- Default for learning or high-stakes changes
- Auto when you trust the pattern
- Plan for complex pieces

**Use the right workflow:**
- TDD for logic-heavy features
- Screenshot iteration for UI
- Refactor workflow for cleanup

**Update docs as you go:**
- Check off tasks in plan.md
- Update decisions.md with any changes
- Add learnings to CLAUDE.md

**Key point:** You're building incrementally, documenting as you go, and keeping memory up to date.

---

## Slide 31: Step 4 - Loop Back to Gemini

**Notes:**
**After implementing, refine the plan:**

- Take what you learned back to Gemini
- Update the PRD with new insights
- "We thought feature X would take 2 days, it took 5 because of Y"
- Plan the next sprint
- Adjust requirements based on reality

**Then back to Claude Code:**
- Implement the next batch
- Update docs
- Commit progress

**The magic:**
Gemini for strategy, Claude Code for execution, constant iteration between the two.

This is how you build real products with AI assistance.

---

## Slide 32: Step 5 - Continuous Improvement

**Notes:**
**Make the system better over time:**

**At the end of each session:**
- "What did we learn?"
- "What patterns should we remember?"
- "What did you get wrong that we fixed?"

**Update your memory:**
- Shared learnings → CLAUDE.md
- Personal tips → CLAUDE.local.md
- Component-specific rules → apps/X/CLAUDE.md

**Create a flywheel:**
- Better docs → Better first attempts
- Better first attempts → Less rework
- Less rework → More time for features

This compounds over time. A year from now, your CLAUDE.md will be an incredible asset.

---

## Slide 33: Key Takeaways

**Notes:**
- Alright, let's wrap up
- Let me leave you with four key takeaways

---

## Slide 34: What We Learned

**Notes:**

**1. AI agents are overly confident coding students**
- They need guidance, review, and iteration
- Small steps win every single time
- Don't expect perfection

**2. Use the right tool for the job**
- ChatGPT for quick questions and concepts
- Gemini for PRDs and high-level planning
- Claude Code for actual building
- Perplexity for research
- Don't try to use one tool for everything

**3. Memory is your superpower**
- CLAUDE.md makes Claude better over time
- Stop re-teaching the same things
- Let your whole team benefit from collective knowledge
- This is what separates good AI usage from great AI usage

**4. Workflows > Features**
- Don't just use Claude Code randomly
- Follow established patterns: TDD, UI iteration, refactoring
- Build your own workflows that work for your team
- Consistency matters

---

## Slide 35: The Real Secret

**Notes:**
Let me leave you with this:

**It's not about the AI.**

**It's about you + AI.**

The best developers with AI aren't the ones with the fanciest prompts or the most expensive tools.

They're the ones who know:
- When to iterate
- When to stop and think
- When to just write it themselves

AI is a tool. A powerful one, but still a tool.

Your judgment is what makes it valuable.

---

## Slide 36: Start Small, Iterate Often

**Notes:**
**My advice to you:**
- Pick ONE workflow from today
- Try it tomorrow on a real task
- See what works for you
- Adjust and iterate

Don't try to implement everything at once. That's the same mistake as asking AI to build Facebook.

Start small. Iterate. Learn what works for YOUR team and YOUR codebase.

**Opening for questions:**
I'd love to hear your questions or experiences with AI coding tools.

---

## Slide 37: Thanks!

**Notes:**
Thanks for your time!

Now go build something cool.

**Closing thoughts:**
Remember:
- AI won't replace you
- But developers who know how to work with AI will be more productive than those who don't
- Start experimenting today
- Share what you learn with your team
- Build your CLAUDE.md files
- And most importantly: iterate in small steps

**Contact/resources:**
[If you have contact info or resources to share, mention them here]

---

## Anticipated Q&A

**Common questions to be prepared for:**

**Q: "How much does this cost?"**
A: Varies by usage. Heavy usage might be $20-50/month. But compare that to developer time saved. The ROI is usually clear within a week.

**Q: "What if Claude ignores my CLAUDE.md?"**
A: Make your instructions more explicit. Use "YOU MUST" or "IMPORTANT". Reorder to put critical info first. Claude respects emphasis.

**Q: "How do we handle multiple people editing CLAUDE.md?"**
A: Treat it like code. Review in PRs. Use conventional commits. Resolve conflicts like any other file.

**Q: "Is this secure? What about our proprietary code?"**
A: Your code goes to Anthropic's API. Read their terms. For sensitive work, use Claude Code's permission system and hooks. Never commit secrets to CLAUDE.md.

**Q: "Can this work with other editors besides CLI?"**
A: Claude Code is primarily CLI-based. But there are integrations being built. Check the docs for current options.

**Q: "What about GitHub Copilot?"**
A: Copilot is great for line-by-line suggestions. Claude Code is for multi-file edits, refactors, and higher-level work. They're complementary, not competitive.

**Q: "How long does it take to set up CLAUDE.md?"**
A: Start with 10-15 minutes writing down your top 5 pain points. Evolve it over sessions. Don't try to write the perfect CLAUDE.md on day one.

**Q: "What if my team won't adopt this?"**
A: Start with yourself. Build your own CLAUDE.local.md. Show results. Let success speak for itself. Don't force adoption.
