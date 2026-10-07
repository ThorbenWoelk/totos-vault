---
created: 2026-08-09
last_edited: 2026-08-09
tags:
  - ai
  - promptengineering
connections: []
ai_generated: false
human_approved: false
category:
  - 002 Notes
  - Courses
  - AI
---
## Elements to a complex prompt
Mnemonic: Uncle Charlie Takes Every Dog Past Fancy Parks
1. "User:"
2. Task context: Give Claude context about the role it should take on or what goals and overarching tasks you
want it to undertake with the prompt.
3. Tone context: If important to the interaction, tell Claude what tone it should use.
4. Detailed task description and rules: Expand on the specific tasks you want Claude to do, as well as any
rules
that Claude might have to follow. This is also where you can give Claude an "out" if it doesn't have an answer
or doesn't know.
5. Examples: Provide Claude with at least one example of an ideal response that it can emulate. Encase this in
<example></example> XML tags. Feel free to provide multiple examples. If you do provide multiple examples,
give Claude context about what it is an example of, and enclose each example in its own set of XML tags.
6. Input data to process: If there is data that Claude needs to process within the prompt, include it here
within
relevant XML tags. Feel free to include multiple pieces of data, but be sure to enclose each in its own set of
XML tags.
7. Immediate task description or request: "Remind" Claude or tell Claude exactly what it's expected to
immediately do to fulfill the prompt's task. This is also where you would put in additional variables like the
user's question.
8. Precognition (also "chain of thought prompting"): For tasks with multiple steps, it's good to tell Claude
to
think step-by-step before giving an answer. Sometimes, you might have to even say "Before you give your
answer..." just to make sure Claude does this first.
9. Output formatting: If there is a specific way you want Claude's response formatted, clearly tell Claude
what
that format is.
10. Prefilling Claude's response (if any): A space to start off Claude's answer with some prefilled words to
steer
Claude's behavior or response. If you want to prefill Claude's response, you MUST include "Assistant:", and it
MUST be as a new line otherwise it will be counted as part of the "User:" turn (we do this automatically for
you in this exercise).
## Style best practices

Use XML tags to structure prompt.

Put the question at the bottom after any text or document
## API prompting techniques

### Prefilling
Start assistant's answer by using "Assistant:" after "User:" in Claude API. Can be used to prime/influence not
only the response style but also the factual response.
### Explicit reasoning
Letting LLM respond in several steps: Giving Claude time to think step by step sometimes makes Claude more
accurate, particularly for complex tasks. However, thinking only counts when it's out loud. You cannot ask
Claude to think but output only the answer - in this case, no thinking has actually occurred.
## Reducing unsupported claims

Give an out: "Only answer if you know the answer with certainty."

Factual grounding in excerpts: For tasks involving long documents (>20K tokens), ask Claude to extract
word-for-word quotes first before performing its task. This grounds its responses in the actual text, reducing
hallucinations.

Verify with citations: verify each claim by finding a supporting quote

Chain-of-thought verification: Ask Claude to explain its reasoning step-by-step before giving a final answer.

Best-of-N verification: Run Claude through the same prompt multiple times and compare the outputs.
Inconsistencies across outputs could indicate hallucinations.

Iterative refinement: Use Claude’s outputs as inputs for follow-up prompts, asking it to verify or expand on
previous statements. This can catch and correct inconsistencies.

External knowledge restriction: Explicitly instruct Claude to only use information from provided documents and
not its general knowledge.
## References

Anthropic's Interactive Prompt Engineering Tutorial
