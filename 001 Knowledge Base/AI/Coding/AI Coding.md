---
created: 2026-08-09
last_edited: 2026-08-09
tags:
  - ai
connections: []
ai_generated: false
human_approved: false
category:
  - Knowledge Base
  - AI
  - Coding
---
## Working notes

Kids today don’t just use agents; they use asynchronous agents. They wake up, free-associate 13 different
things for their LLMs to work on, make coffee, fill out a TPS report, drive to the Mars Cheese Castle, and
then check their notifications. They’ve got 13 PRs to review. Three get tossed and re-prompted. Five of them
get the same feedback a junior dev gets. And five get merged.
“I’m sipping rocket fuel right now,” a friend tells me. “The folks on my team who aren’t embracing AI? It’s
like they’re standing still.” He’s not bullshitting me. He doesn’t work in SFBA. He’s got no reason to lie.

## My AI Skeptic Friends Are All Nuts
Emerging issues when coding with AI:

It's taking the AI a short time to spin up boilerplate garbage but it takes a long time to get the details
right and actually ship an MVP

Long waiting times usually not used productively

Limits of capacity

e.g. session or context length

Limits of complexity

any complex project split into multiple files poses a problem

High cost
Also, open questions are:

how do junior devs learn the basics nowadays when there's no real need to go through that struggly
## Strategy
The first question one should ask oneself is which strategy is the right one for the problem at hand.

Vibe coding

Copiloting
In vibe coding, the user rarely writes code at all. She prompts the LLM with instructions and QAs the result.
This strategy is useful when starting a project from scratch and using the LLM to jumpstart a basic structure.
It works best on standard coding tasks within the respective programming language and paradigm in contrast to
more special requirements.
In copiloting, one uses AI to help writing or adjusting small bits of code to speed up smaller chunks of work
in the broader process of coding. The developer remains in the driver seat and has a deep understanding of the
code base. This strategy is the best approach in most circumstances due to the vast downside risks of vibe
coding like security concerns, complexity creep, high costs, resource limits and lack of sanity checking.
Depending on the integration of the AI into coding environment, there might be a lot of tedious copy pasting
involved.
## Prompting
The next decision is on which level to communicate with the LLM. Basically, do I take the role of a product
owner that talks about features and functionality and is ignorant about the implementation approach or do I
mimic a pair programmer who sits beside the person at the keyboard but is involved in the technical approach
of implementation?
## Rules
As a general rule, it is good to set some guardrails for the AI on a technical level in order to rule out
implementation decisions that go against your general intent. Which programming

