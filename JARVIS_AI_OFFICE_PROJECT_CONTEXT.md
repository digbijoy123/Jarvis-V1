# JARVIS AI OFFICE --- PROJECT CONTEXT / HANDOFF

## Purpose

This file is the canonical project context for continuing development without losing the current architecture, decisions, repository, deployment, controls, and known issues.

Do not invent missing facts. If something is not documented here or verified from the repository, treat it as unknown and inspect the repo before changing it.

## 1. Current Project

**Project name:** JARVIS AI OFFICE / AI Web Office

**User-facing boss agent:** MAX

**Underlying orchestration/runtime concept:** JARVIS

MAX is the visible boss agent. JARVIS can remain the underlying orchestration/runtime layer.

The product is intended to become a browser-based AI operating environment where the user gives MAX natural-language tasks and MAX delegates real work to specialist AI agents.

Core concept:

```
User
 ↓
MAX
 ↓
Planner
 ↓
Specialist Agents
 ↓
Tool Gateway
 ↓
GitHub / Vercel / Browser / APIs / Files / Database
 ↓
QA
 ↓
Deployment
 ↓
Result
```

The physical 3D office is intended to visualize the actual AI workforce/runtime rather than being only decorative.

## 2. Current Repository

**GitHub repository:**

`https://github.com/digbijoy123/Jarvis-V1.git`

IMPORTANT:

The user originally mentioned `Ai-web-office`, but explicitly chose:

> "No let's use the repo u created"

Therefore the current canonical repository is **Jarvis-V1**.

Do not switch repositories unless the user explicitly asks.

## 3. Current Deployment

Vercel project:

**Name:** `jarvis-v1`

Current known production deployment associated with the latest fix:

`jarvis-v1-ie0cbryw8-digbijoy-das-projects.vercel.app`

Known project ID:

`prj_EZxcYfhL0X0yvC3ov1KrksRR8r0k`

Known Vercel team:

`digbijoy-das-projects`

Team ID:

`team_cnllAZyzk0t8UQixAxcJaZ4b`

The older public URL used by the user:

`jarvis-v1-eight.vercel.app`

## 4. Latest Verified GitHub Commit

Latest verified commit at the time this document was created:

`b0160aa3db031af99f795c53e89040c39a0e23cc`

Commit message:

`fix: make GLTF loading non-blocking and use Three addons import map`

The immediately previous commit was:

`94dd110f23d75896465f770c7988cfb39292056a`

Message:

`fix: add Three.js import map for GLTF modules`

## 5. Important Commit History

Known recent commits:

- `839980170acaa83cf5889fa7f0c9996c8ede1fe0` — initial index.html
- `059d33c0a16502a634d7036e9e64c043b667d8c0` — README
- `0be0791cb87958cb99132d784ce6d35ac5fe4f7e` — desktop/mobile controls
- `0c3cd43e24bb7f2e78b9ac043986c2166f57bef2` — mobile joystick/touch camera redesign
- `cfd3f6310f6dbc4fbf8690e54e1bff6d6670148e` — touch-device detection, joystick and mobile camera
- `55648c03f8f2760be5f3fc9e830d32e893298ad2` — joystick movement state fix
- `df9a46979365aa2074e926c850eba6d5caddb2e2` — camera/joystick direction fix
- `453f6106aaf88343874462a59c1895c2e4a57c76` — dedicated AI cubicles + AI slots
- `8dbedd357390477c556f3f94eff4efe7d98a83be` — Max command office + specialist hallway
- `be01dcf0a68ccca1d98e14d09d57a1ead70abfa9` — real glTF agent models + interior redesign
- `4049e9424d40164f7f7f7f726f594edc0c3f840e` — Max model initialization/click target fix
- `94dd110f23d75896465f770c7988cfb39292056a` — Three.js import map fix
- `b0160aa3db031af99f795c53e89040c39a0e23cc` — non-blocking GLTF loader / addons import fix

## 6. Current Frontend Architecture

The current app is intentionally simple:

- Single `index.html`
- Vanilla HTML/CSS/JavaScript
- Three.js loaded from CDN
- No React/Next.js yet
- No backend orchestration yet
- Vercel serves the static application

Three.js version:

`0.180.0`

Main Three.js module:

`https://cdn.jsdelivr.net/npm/three@0.180.0/build/three.module.js`

The current import map contains:

```json
{
  "imports": {
    "three": "https://cdn.jsdelivr.net/npm/three@0.180.0/build/three.module.js",
    "three/addons/": "https://cdn.jsdelivr.net/npm/three@0.180.0/examples/jsm/"
  }
}
```

GLTFLoader is now loaded dynamically through:

```js
await import('three/addons/loaders/GLTFLoader.js')
```

This was deliberately changed so an addon import failure does not prevent the main Three.js renderer from starting.

## 7. Current 3D Model

Current external model:

Khronos glTF CesiumMan:

`https://cdn.jsdelivr.net/gh/KhronosGroup/glTF-Sample-Models@main/2.0/CesiumMan/glTF-Binary/CesiumMan.glb`

The same rigged humanoid model is cloned for MAX and specialists.

Current cloning uses:

```js
asset.scene.clone(true)
```

There is a fallback primitive if the external model cannot load.

IMPORTANT:

If model loading becomes unreliable, consider moving a stable GLB asset into the repository or switching to a stable current asset source. Do not silently invent a model URL.

## 8. Current Physical Office

The 3D environment currently contains:

### Main hallway

- Long modern corridor
- Dark architectural walls
- Architectural wall panels
- Glass cubicle dividers
- Ceiling lights
- Floor light strips
- Individual workstation areas
- Long hallway layout

### MAX office

MAX has a separate executive office at the far end.

It contains:

- Desk
- Monitor
- Chair
- Carpet
- Beacon/ring
- MAX humanoid model
- MAX label
- COMMAND OFFICE label

MAX role:

`Boss agent · receives user commands and orchestrates the office`

## 9. Current Specialist Agents

The current specialist registry contains:

1. CODER — Full-stack software engineering
2. RESEARCHER — Web research and evidence
3. DESIGNER — UI UX and product design
4. GAME ENGINEER — Games simulations and 3D
5. TEACHER — Teaching tutoring and explanations
6. ENGINEER — Technical engineering and systems
7. BUSINESS — Business sales and operations
8. WRITER — Writing documents and communication
9. DATA ANALYST — Data analysis and automation
10. QA — Testing debugging and verification
11. DEVOPS — GitHub deployment and infrastructure

## 10. Current Controls

### Desktop

- WASD
- Arrow keys
- Mouse look
- On-screen WASD control pad

### Mobile

The app detects touch devices using:

```js
navigator.maxTouchPoints > 0 || matchMedia('(pointer:coarse)').matches
```

Mobile controls:

- Virtual analog joystick on the lower-left
- Right-side touch camera zone
- Horizontal swipe controls yaw
- Vertical swipe controls pitch

Known previous bugs that were fixed:

### Joystick state bug

`updateKeys()` previously overwrote joystick movement.

Fixed by keeping:

```js
joyX
joyZ
```

separate from keyboard state.

### Joystick direction

Forward/backward direction was reversed.

Fixed with:

```js
joyZ = -dy / max
```

### Camera swipe direction

Horizontal touch camera direction was reversed.

Fixed with:

```js
yaw += dx * 0.006
```

## 11. Current UI

Top-left:

`MAX // AI OFFICE`

Subtitle:

`COMMAND CENTER · AI WORKFORCE`

Top-right:

`SYSTEM ONLINE`

Command panel:

`AI COMMAND`

Current description:

`Talk to Max to give the office a task, or visit a specialist cubicle.`

Initial log:

`JARVIS: Max is online. Command center ready.`

Input:

`Tell Max what you want done…`

## 12. Current Interaction Model

Specialist cubicles are clickable.

MAX is also clickable.

Selecting an agent changes:

- panel title
- panel description
- task placeholder
- activity log

Current task submission is NOT real backend execution.

It currently says:

`Task queued in AI slot. Backend orchestration will execute it in the next build.`

This is a known limitation.

DO NOT claim that the agents are currently executing real tasks.

## 13. What MAX Is Supposed To Eventually Do

MAX should become the main user interface for the AI workforce.

Example:

User:

> Make me a simple website for my business.

Expected architecture:

```
User
 ↓
MAX
 ↓
Planner
 ↓
DESIGNER
 ↓
CODER
 ↓
QA
 ↓
DEVOPS
 ↓
GitHub
 ↓
Vercel
 ↓
Live URL
 ↓
MAX reports result
```

Another example:

> Make me a simple game.

Expected delegation:

```
MAX
 ↓
GAME ENGINEER
DESIGNER
CODER
QA
DEVOPS
```

The agents must eventually perform actual work using tools.

They must not merely produce fake status messages.

## 14. Target Task State Machine

Future task states:

```
planned
assigned
running
waiting
testing
completed
failed
```

A task should have a persistent identity and project context.

## 15. Target Agent Architecture

Future agent registry should contain:

- agent ID
- name
- role
- capabilities
- tools
- current status
- current task
- workspace/project
- model/provider
- permissions

Example:

```json
{
  "id": "coder",
  "name": "CODER",
  "role": "Full-stack software engineering",
  "status": "idle",
  "capabilities": [
    "frontend",
    "backend",
    "database",
    "debugging"
  ]
}
```

## 16. Target Tool Gateway

MAX should eventually have access to controlled tools such as:

- GitHub
- Vercel
- Browser/web research
- Files
- Database
- APIs
- Code execution
- Testing
- Deployment

Credentials/API keys must NEVER be placed directly into the browser frontend.

The browser UI should communicate with a secure backend/tool gateway.

## 17. Target Software Factory

For software-development tasks:

```
Plan
 ↓
Code
 ↓
Run
 ↓
Test
 ↓
Debug
 ↓
Retest
 ↓
Commit
 ↓
Deploy
 ↓
Verify
 ↓
Return result
```

QA should verify the actual result before MAX claims completion.

## 18. Target Persistent Project Memory

Each project should eventually retain:

- project name
- stack
- GitHub repository
- Vercel project
- active task
- known issues
- previous decisions
- files
- deployment state
- agent activity
- task history

This allows MAX to continue work without starting from zero.

## 19. Recommended Next Major Development Phase

After the 3D office is stable, stop adding decorative features temporarily.

Build the real execution architecture:

### Phase 1 — Backend foundation

Create:

```
/api/tasks
/api/agents
/api/projects
/api/events
```

or equivalent server-side routes.

### Phase 2 — Task engine

Natural language:

```
"Build me a website"
```

becomes a structured task graph.

### Phase 3 — Tool gateway

Connect real tools.

Priority:

1. GitHub
2. Vercel
3. Browser/research
4. Files
5. Code execution
6. Testing

### Phase 4 — Real software factory

Implement:

```
Plan → Code → Test → Fix → Commit → Deploy → Verify
```

### Phase 5 — Office visualization

Make the 3D office reflect real runtime state:

- idle
- planning
- working
- waiting
- testing
- failed
- completed

Agents should visibly change behavior based on actual task state.

## 20. Important Development Rules

### Rule 1 — Verify before modifying

Before changing code:

1. Fetch the current file from GitHub.
2. Inspect the actual current content.
3. Identify the exact bug.
4. Make the smallest correct change.
5. Re-fetch the file after updating.
6. Verify the intended code exists.
7. Check the resulting commit.
8. Check Vercel deployment state when relevant.

### Rule 2 — Never claim visual verification without doing it

If the deployment has not actually been opened/tested, say so.

Do not claim:

> "It works"

based only on successful GitHub or Vercel deployment.

A successful deployment only proves that Vercel accepted/built/deployed the project.

### Rule 3 — Never invent tool capabilities

If a tool is unavailable, inspect available tools first.

### Rule 4 — Never invent repository state

Always fetch the repository file when current contents matter.

### Rule 5 — Preserve the user's architecture decisions

Do not switch to:

- Android APK
- another repository
- another framework
- another hosting provider

unless explicitly requested.

The current direction is the website.

### Rule 6 — Do not turn the project into a fake demo

The long-term goal is a real autonomous work platform.

UI status messages must eventually represent actual backend task state.

## 21. Current Known Technical Risk

The biggest current frontend risk is external 3D model/module loading.

Current approach:

- Three.js from jsDelivr
- GLTFLoader via Three addons import map
- CesiumMan from a GitHub CDN URL

The latest fix makes model loading non-blocking.

If the model fails:

- renderer should still start
- fallback model should appear
- console should report the model-loader failure

## 22. Current Known Functional Limitation

The task input is still only a UI simulation.

There is currently no real:

- LLM orchestration backend
- planner
- task queue
- agent execution runtime
- GitHub task executor
- Vercel deployment executor
- QA execution pipeline
- persistent project database

These are future implementation stages.

## 23. User's Development Environment

The user frequently works from an Android phone and may not have access to their PC.

Therefore:

- Prefer cloud development workflows.
- GitHub and Vercel are important.
- Avoid requiring a local development server unless necessary.
- Changes should be deployable from GitHub.
- Browser-based testing is valuable.

## 24. Current User Experience Goal

The user should feel like they are entering a real AI company/office.

The conceptual experience:

1. Enter the office.
2. Walk around in first person.
3. See MAX's executive office.
4. Visit specialist cubicles.
5. Give MAX a natural-language command.
6. MAX plans the work.
7. Agents physically represent the work.
8. Real tools execute the work.
9. QA verifies it.
10. MAX returns the result.

The 3D office is the interface to the AI operating system.

## 25. Do Not Lose These Decisions

- Boss agent is **MAX**, not JARVIS.
- JARVIS is the underlying orchestration/runtime concept.
- Current repository is **Jarvis-V1**.
- Current product is a **website**, not an Android APK.
- Mobile controls are part of the website.
- Desktop uses WASD/mouse.
- MAX has a separate executive office.
- Specialists have dedicated cubicles.
- Specialists should eventually be real autonomous workers.
- The 3D office should eventually visualize actual agent state.
- The ultimate objective is a real AI workforce/work operating system.

## 26. Immediate Continuation Point

When continuing development from this file:

1. Inspect `Jarvis-V1/index.html`.
2. Verify the latest commit instead of assuming it is unchanged.
3. Verify the latest Vercel deployment.
4. Fix the current rendering/model issue if one remains.
5. Only then proceed with the next architecture step.
6. Do not hallucinate missing implementation details.

This document is a project handoff/context file, not a claim that future architecture has already been implemented.


## 27. Mandatory Change Verification Rule

For **every future code change** in this project:

1. Fetch the current file from GitHub before editing.
2. Make the requested change.
3. Re-fetch the edited file from GitHub **after the edit**.
4. Verify the exact changed logic exists in the resulting file.
5. Check for initialization/order/reference errors introduced by the change.
6. Verify important dependencies and runtime state are defined before they are used.
7. Check the resulting commit SHA.
8. Check the Vercel deployment state when the change affects the deployed site.
9. Do not tell the user a change is fixed merely because GitHub accepted the commit.
10. If a change can be runtime-tested, actually test it before claiming it works.

This rule applies to **every future change**, not only bug fixes or settings changes.

The recent settings bug is an example: `applySettings()` was accidentally executed before the settings/runtime declarations were initialized, causing the JavaScript module to stop during startup. The corrected version moves the call after all settings bindings and before the animation loop.
