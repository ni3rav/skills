# Building Great Agent Skills: Core Manual

Source: Matt Pocock, [Building Great Agent Skills: The Missing Manual](https://www.youtube.com/watch?v=UNzCG3lw6O0). When these notes name a file or a frontmatter key, use the spelling in `SKILL-MECHANICS.md`.

## 1. The Core Problem: Skill Hell

* **Definition**: A state where numerous agent skills are available, but developers and organizations lack a shared rubric to evaluate, compose, or reliably execute them.
* **Failure Modes**:
* Inability to evaluate skill quality before deployment.
* Inability of organizations to translate Standard Operating Procedures (SOPs) into effective agent workflows.
* Over-accumulating skills without understanding their systemic interactions.



## 2. The Four-Part Skill Checklist

1. **Trigger**: Determining invocation mechanics and operational tradeoffs.
2. **Structure**: Organizing steps, reference documentation, and context boundaries.
3. **Steering**: Guiding model attention and controlling task execution depth.
4. **Pruning**: Minimizing token footprint, eliminating redundancies, and verifying necessity.

---

## 3. Trigger: Invocation Mechanics

### Model-Invoked Skills

* **Mechanism**:
1. A short skill description resides permanently within the agent's system prompt as a context pointer.
2. The agent reads the description and autonomously decides to load the full skill instructions into its context window.


* **Tradeoffs**:
* Increases continuous token overhead on every model request.
* Increases context load and attentional distraction for the model.
* Introduces unpredictability, as the model may fail to invoke the skill even when required.
* Requires extensive evaluation suites to verify invocation consistency.



### User-Invoked Skills

* **Mechanism**:
1. The skill resides on the local file system with model invocation explicitly disabled (`disable_model_invocation: true`).
2. The skill is manually triggered by the user via slash commands or direct prompts.


* **Tradeoffs**:
* Eliminates passive token overhead from system prompts.
* Eliminates model invocation unpredictability.
* Increases cognitive load on the user, requiring the operator to know when and how to call the skill.



---

## 4. Structure: Internal Organization

### Core Units

1. **Steps**: Procedural workflows and sequential operations the agent must follow.
2. **Reference**: Static domain knowledge, formatting templates, or schemas that support the execution of steps.

* Skills may contain only steps, only reference, or a hybrid of both.

### Sizing and Modularization Rules

* **Root File Minimization**: Keep the primary `skill.md` file as small as possible to reduce token usage and simplify auditing.
* **Single-Branch Content**: Reference material required on every execution must reside directly inside `skill.md`.
* **Multi-Branch Isolation**:
* Reference material required only for specific conditional paths must be removed from `skill.md`.
* Store branch-specific templates and reference data in separate markdown files within the skill directory.
* Use explicit context pointers inside `skill.md` instructing the agent to load the external file only when that specific branch is entered.



---

## 5. Steering: Controlling Agent Behavior

### Technique 1: Leading Words (Leitmotifs)

* **Problem**: Agents frequently default to horizontal layer-by-layer coding (e.g., writing all database schemas, then all API endpoints, then UI) instead of building testable increments.
* **Mechanism**:
1. Select dense, established engineering terminology that carries deep semantic weight (e.g., "vertical slice").
2. Repeat the term consistently across instructions rather than writing lengthy descriptive rules.
3. The model repeats the term during internal chain-of-thought tokens, steering its behavioral priors toward the desired pattern.


* **Verification**: Check agent reasoning traces to confirm the model actively repeats and applies the term.

### Technique 2: Leg Work Enforcement via Goal Hiding

* **Problem**: When a multi-step procedure includes both exploratory steps (e.g., asking clarifying questions, inspecting codebases) and a final deliverable (e.g., writing an implementation plan), models prematurely rush to the final deliverable and under-perform on exploratory leg work.
* **Mechanism**:
1. Decouple exploratory and execution phases into separate, distinct skills.
2. Execute the exploratory skill in isolation so the agent cannot observe the downstream deliverable.
3. Invoke the subsequent execution skill only after the user verifies the output of the exploratory skill.



---

## 6. Pruning: Optimization and Reduction

### 1. Single Source of Truth (DRY)

* Eliminate duplicate explanations across steps and reference materials.
* Define concepts, rules, and schemas in exactly one place per skill ecosystem.

### 2. Sediment Removal

* Identify and delete legacy additions, uncoordinated team contributions, and stale procedures that accumulate over time.
* Relocate branch-specific additions into external context files.

### 3. Deletion Testing for No-Ops

* **Definition of a No-Op**: An instruction that looks purposeful but does not measurably alter model output because the underlying model already performs the behavior by default (e.g., generic instructions to write clear commit messages).
* **Test Procedure**:
1. Delete a target instruction block.
2. Run the agent against relevant benchmarks.
3. If performance and formatting remain identical, permanently remove the instruction.
