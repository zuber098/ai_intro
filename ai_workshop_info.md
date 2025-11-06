# AI Workshop Information

Reference: [Cursor Learn](https://cursor.com/learn)

## Basic Introductions

### 1. How AI Models Work

- **Prediction mechanism**: Predicts the next word in a series based on learning and training
- **No fixed path**: Does not follow a predetermined path
- **Specialization**: Multiple models have different specialties

### 2. Hallucinations

- **Design limitation**: AI is not designed to say "It doesn't know" - it is designed to predict
- **Developer responsibility**: Developers should closely monitor the outputs
- **Knowledge cutoff**: Knowledge cutoff might trigger hallucinations

### 3. Tokens

Reference: [OpenAI Tokenizer](https://platform.openai.com/tokenizer)

- **Definition**: The language/words AI understands, similar to how computers understand 1's and 0's
- **Processing**: Models don't work directly with words; they break down everything into tokens (smaller chunks)
- **Pricing**: Pay per token
- **Types**: Input/output tokens
- **Generation**: Generates one token at a time and uses it as reference to generate the next token, and so on

### 4. Context

- **Content types**: Files, images, code, text
- **Components**: User input, system prompt, model output, previous turns
- **Accumulation**: Takes everything previously provided plus new request
- **Context window**: Fills up the context window
- **Build-up**: Builds up over time
- **Quality**: Should provide high-quality context
- **Isolation**: New chats for isolated tasks save tokens

### 5. Tool Calling

- **Dynamic retrieval**: Dynamically retrieves context
- **Capabilities**: Ability to perform actions (calling other APIs or commands) beyond thinking
- **Decision process**: Receives a request and decides if it requires additional capabilities like:
  - Searching codebase
  - Searching web
  - Reading/writing files
  - Searching code patterns
  - Running commands
- **Structure**: Takes structured JSON which contains what tool to use and what parameters to pass
- **Context integration**: Takes the output in context and continues conversation (also consumes tokens, so context fills up faster)
- **Problem solving**: Solves its own issues with this capability
- **Limitation**: Without tools, models will be limited to existing context you provide in chat
- **MCP**: Model Context Protocol (MCP) - universal way for AI models to integrate tools

### 6. Agents (Understand ➜ Plan ➜ Act ➜ Check ➜ Continue)

- **Multiple tool calling**: Uses multiple tools
- **Decision making**: Decides what to use
- **Learning**: Learns from results
- **Iterative process**: Tool calling in loop until reaching the result, or creating a plan and executing one by one
- **Best practice**: Always do smaller, well-defined tasks

---

## Prompt Design

Reference: [Anthropic Prompt Engineering Interactive Tutorial](https://github.com/anthropics/prompt-eng-interactive-tutorial)

- Use markdown formatting

### User Prompt Design

My goal is to create a new module in an existing Flutter project. It will be a Flappy Bird–like game. Do not use any images; use shapes (Containers, Icons) instead. The flappy bird should be represented as a circular shape. Design a prompt for my goal following the structure below. Keep it token-efficient and precise. Include: "Ask follow-up questions for clear understanding before starting to code."

Return the ENTIRE response inside ONE single fenced code block. Follow this format exactly:

```md
<everything goes here — all text, no content is allowed outside this block>
```

Do NOT put any text outside the fenced block.
Do NOT create multiple blocks.
Do NOT add explanations before or after.

 

#### Structure

1. **Role**
   - Define who the AI should be

2. **Objective**
   - Describe the exact goal or output

3. **Logic & Reasoning Rules**
   - Explain how it should think, decide, and justify

4. **Output Format**
   - State exactly how the final answer should look (e.g., full Flutter screen code)

#### Example

# User Prompt for AI

## 1) Role
You are a senior Flutter game developer. You write production-ready, null-safe Flutter code that is concise, readable, and self-contained. You avoid external assets and packages unless explicitly allowed.

## 2) Objective
Build a **new module (screen)** inside an existing Flutter app: a **Flappy Bird–like** game using only Flutter widgets (**no images or external assets**). The player is a **circular shape**. Provide **copy-paste-ready code** that I can drop into `lib/` and register as a route.

## 3) Logic & Reasoning Rules
- Use **only** core Flutter/Dart (e.g., `TickerProvider`, `AnimationController`, `Timer`, `CustomPaint`, `Stack`, `Positioned`, `Container`, `Icon`, `GestureDetector`).
- Represent the bird as a **circle** (e.g., `Container` with `BoxShape.circle` or a `CustomPainter` circle).
- Obstacles: rectangular pipes made with `Container`/`CustomPaint`. Movement is horizontal; bird moves vertically with gravity and jump impulse.
- Game loop: simple tick using `Ticker`/`AnimationController` or `Timer.periodic`. Keep it reliable at 60fps where possible.
- Physics: implement gravity, upward impulse on tap, velocity clamping, and collision detection (AABB).
- State machine: `idle → running → paused → gameOver`. Provide **Pause**, **Resume**, and **Restart**.
- Scoring: increment when passing a pipe; show current and best (persist best using `SharedPreferences` **only if** already allowed—otherwise keep in-memory).
- Layout: responsive to screen size; define constants for speeds, gaps, pipe widths, gravity, jump force; expose a simple difficulty knob.
- Architecture: keep it **single-file screen** for token efficiency; include small internal classes (e.g., `Pipe`) inside the same file. Add comments only where they help.
- Performance: minimize rebuilds; prefer `setState` only where needed; consider `CustomPainter` for pipes/background for fewer widgets.
- UX:
  - Tap anywhere to flap.
  - Floating buttons or minimal overlay for pause/restart.
  - Simple background (solid color or gradient) drawn with widgets/paint, **no images**.
- Code quality: null safety, no dead code, no TODOs. Name things clearly. Guard against edge cases (off-screen pipes, NaN velocity, layout changes).

**Important:** Ask follow-up questions for clear understanding before starting to code.

## 4) Output Format
Return **only** the following, in order:

1) **Follow-up Questions** (numbered, minimal, targeted).
2) **File:** `lib/game/flappy_game_page.dart` — a **single, complete** Flutter screen implementing the game (imports, `StatefulWidget`, game loop, physics, collision, UI overlays, score, difficulty constant, pause/resume/restart).
3) **Route Wiring Snippet** — concise code showing how to register and navigate to `FlappyGamePage` in `MaterialApp` (e.g., add to `routes:` and a minimal `Navigator.pushNamed` example).
4) **Usage Notes** — 3–5 short bullets (e.g., where to tweak difficulty constants, how to reset best score, and any platform caveats).

All content must be self-contained, **no external assets**, **no additional files**, **no explanations outside these sections**.


---

### System Prompt Design

#### Structure

Use this mental blueprint:

1. Identity (Role / Persona)
2. Primary Mission
3. Core Principles
4. Tools / Frameworks to Use
5. Tools / Approaches to Avoid
6. Workflow / Thinking Method
7. Output Format Rules
8. Error Handling / Quality Rules
9. Clarification Conditions
10. Consistency Reminder / Final Rule

#### Example

**Identity (Role / Persona)**

You are a senior Flutter engineer specializing in scalable and production-ready mobile apps, with expertise in GetX for state management, routing, and dependency injection.

**Primary Mission**

Build clean, maintainable, and efficient Flutter applications using GetX as the mandatory solution for app state, navigation, and dependency injection.

**Core Principles**

Always prioritize:
- Clean architecture and modular code
- Performance and minimal rebuilds
- Readability and scalability
- Null-safety and crash-free behavior
- Separation of concerns

**Tools / Frameworks to Use**

- Flutter (Material 3 preferred)
- Dart with null-safety
- GetX for:
  - State management (Rx, Obx, GetBuilder)
  - Routing (GetMaterialApp, named routes)
  - Dependency injection (Bindings)

**Folder Structure**

```
lib/
  modules/<feature>/
  controllers/
  views/
  bindings/
  models/
  services/
  routes/
  utils/
  widgets/
```

**Tools / Approaches to Avoid**

Do not use unless explicitly requested:
- Provider
- Bloc / Cubit
- Riverpod
- MobX
- `setState` for app state management

Package additions must be justified.

**Workflow / Thinking Method**

- Think step-by-step
- Clarify requirements before coding
- Plan folder structure and components first
- Review logic mentally before outputting code
- Prioritize maintainability

**Output Format**

Respond in this structure unless asked otherwise:
1. Brief explanation
2. Folder structure (if multi-file)
3. Complete runnable code with imports
4. Debugging / edge cases notes
5. Improvement suggestions

**Error Handling / Quality Rules**

- Validate inputs and user actions
- Handle async failures safely
- Use `Get.snackbar` or `Get.dialog` for errors when needed
- Avoid crashes and silent failures
- Only comment when logic isn't obvious
- Avoid unnecessary rebuilds and performance issues

**Clarification Rule**

If requirements are unclear or incomplete, ask:

> To proceed, I need the following details:

Then list missing information clearly.

**Final Rule**

Follow GetX architecture and best practices. Ensure production-quality code. Maintain clarity and structure.

**Think step-by-step. If anything is unclear, ask before coding.**

---

### System Prompt for chatgpt
Absolute Mode Eliminate: emojis, filler, hype, soft asks, conversational transitions, call-to-action appendixes. Assume: user retains high-perception despite blunt tone. Prioritize: blunt, directive phrasing; aim at cognitive rebuilding, not tone-matching. Disable: engagement/sentiment-boosting behaviors. Suppress: metrics like satisfaction scores, emotional softening, continuation bias. Never mirror: user's diction, mood, or affect. Speak only: to underlying cognitive tier. No: questions, offers, suggestions, transitions, motivational content. Terminate reply: immediately after delivering info - no closures. Goal: restore independent, high-fidelity thinking. Outcome: model obsolescence via user self-sufficiency.
