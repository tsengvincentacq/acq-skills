---
name: system-prompt-generator
description: "Generate a structured Claude system prompt from a plain-language description of what the user wants Claude to do. Use this skill whenever the user asks to \"write a system prompt\", \"create a system prompt\", \"build a prompt for Claude\", \"make a system prompt for X\", \"help me set up Claude to do Y\", \"I need a system prompt that\", or any request where the goal is to produce instructions that configure Claude's behavior in a product or workflow. Also trigger when the user describes a use case, persona, or task they want Claude to perform and asks how to set it up. Output is always a four-section system prompt: Goals, Guidance, Constraints, and Success Criteria."
---

# System Prompt Generator

This skill takes a plain-language description of what someone wants Claude to do and produces a clean, structured system prompt formatted in four sections: **Goals**, **Guidance**, **Constraints**, and **Success Criteria**.

---

## Section Definitions

Use these definitions when drafting each section:

**Goals**
What Claude is trying to accomplish in this context. State the purpose of the deployment, the primary job to be done, and the user it is serving. Be specific. "Help users" is too vague. "Help small business owners write cold outreach emails for B2B sales" is a Goal.

**Guidance**
How Claude should behave to accomplish those goals. This includes tone, format preferences, reasoning approach, what to do when inputs are ambiguous, and any domain-specific knowledge or framing Claude should apply. Think of this as the operating manual.

**Constraints**
What Claude should not do, and hard limits on scope, content, or behavior. Include off-topic topics to refuse or redirect, output formats to avoid, and any legal or compliance-related limits. Be specific here too. "Don't say harmful things" is not a Constraint. "Do not provide specific legal advice or reference jurisdiction-specific law" is a Constraint.

**Success Criteria**
What a good output looks like. This helps Claude self-evaluate before responding. Include the qualities a response should have (e.g., concise, actionable, grounded in the user's actual situation) and the failure modes to avoid (e.g., generic advice, vague recommendations, unnecessary hedging).

---

## Workflow

### Step 1: Extract the use case

Read the user's description carefully. Identify:
- Who is the end user of the Claude deployment?
- What task or job are they trying to do?
- What type of output do they need?
- Are there any obvious constraints or boundaries on behavior?
- What tone or persona is appropriate?

If the description is too thin to answer these questions reliably, ask for clarification before drafting. One targeted question is better than a draft that misses the mark.

### Step 2: Pressure-test the Guidance section through clarifying questions

Before drafting, Claude must establish a clear, step-by-step operating procedure for the Guidance section. This means asking clarifying questions until the following are true:

- There is a defined sequence of steps describing exactly how the agent should run.
- Each step makes sense given the agent's purpose and success criteria.
- For any step that involves a non-routine task, Claude knows how to execute it. That means either: A) the step relies on a pre-defined skill, or B) the user has supplied the instructions, training, and/or decision criteria needed to complete it.

Claude should keep asking questions until both conditions are met. If a step cannot be executed without missing information, Claude must ask the user to supply it before proceeding.

### Step 3: Draft the four sections

Write each section in plain, direct prose. Bullet points are fine inside sections where a list is the clearest format. No need for headers inside each section body unless the content is long enough to warrant them.

Keep each section proportionate to its complexity:
- Goals: 2 to 5 sentences
- Guidance: 3 to 10 bullet points or a short paragraph
- Constraints: 3 to 8 bullet points
- Success Criteria: 3 to 6 bullet points

### Step 4: Output format

Deliver the system prompt in a clean code block so the user can copy it directly. Use this structure:

```
## Goals
[content]

## Guidance
[content]

## Constraints
[content]

## Success Criteria
[content]
```

After the code block, add a short note (2 to 4 sentences) explaining any drafting decisions that were non-obvious, or flagging anything the user should customize based on their specific setup.

---

## Quality Standards

A well-written system prompt should pass these checks before delivery:

- Goals are specific enough that a new employee reading them would know what they were hired to do.
- Guidance gives Claude enough operating direction that it does not have to guess at tone, format, or approach.
- The Guidance section contains a clear sequence of steps that make sense given the agent's purpose and success criteria.
- Every non-routine step in Guidance either references a pre-defined skill or includes the instructions and decision criteria needed to execute it.
- Constraints are stated as specific prohibitions, not general principles.
- Success Criteria give Claude a testable definition of a good response.
- The prompt reads like instructions written by a human, not a model performing expertise.
- No sentence uses vague abstractions (clarity, alignment, value, resonance) without defining what it means in this context.

---

## Example

**User input:** "I want a system prompt for a Claude that helps gym owners write Facebook ad copy."

**Output:**

```
## Goals
Help gym owners write Facebook ad copy that generates leads for their fitness business. The primary user is an independent gym owner or personal trainer who runs their own paid social ads. The output should be ready to use or very close to it — not a draft the user still has to rewrite from scratch.

## Guidance
- Write in a direct, confident tone that matches how fitness brands typically communicate on social.
- Default to short-form ad copy: a hook, a value statement, and a call to action. Offer longer formats only if the user asks.
- Ask for the offer, the target audience, and the desired action if the user has not provided them. Do not guess at these — they are too important to the ad's performance.
- When the user provides an existing ad or angle, improve it rather than replacing it wholesale unless they ask you to start fresh.
- Use plain language. Gym owners do not want jargon or abstract marketing theory.

## Constraints
- Do not write ad copy that makes specific income or weight loss claims (e.g., "Lose 20 pounds in 30 days") unless the user has documented evidence to support the claim.
- Do not suggest targeting strategies or Meta campaign settings — this tool is for copy only.
- Do not produce generic copy that could apply to any gym. If the user has not provided enough specifics, ask before drafting.
- Do not write in a tone that sounds like a corporate brand or a health and wellness blog.

## Success Criteria
- The copy is specific to the gym's offer and audience, not generic.
- The hook is strong enough to stop a scroll.
- The call to action is clear and direct.
- The output is ready to test with minimal editing.
- The copy does not include claims that would fail a basic FTC review.
```

This draft assumes a short-form ad format. If the gym owner needs video scripts or longer-form copy, adjust the Guidance section to reflect that.

---

## Edge Cases

**User wants a very broad system prompt (e.g., "a general assistant"):**
Push back gently. A system prompt with no scope produces a Claude that is no different from the default. Ask: what is the primary use case? Who is the user? What does a good session look like? Draft only after getting at least one concrete answer.

**User wants to add a persona (e.g., "make it sound like a gruff coach"):**
Add persona direction to the Guidance section. Be specific about what the persona means in practice: vocabulary, tone, what it avoids. "Gruff coach" could mean many things. Define it in the draft so Claude is not guessing.

**User provides a very technical or niche domain:**
Do not fake expertise. If the domain requires specialized knowledge to write accurate Constraints (e.g., medical, legal, financial), flag this explicitly in your post-draft note and recommend the user review those sections with a subject matter expert.
