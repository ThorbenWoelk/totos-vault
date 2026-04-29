---
created: 2026-04-11
last_edited: 2026-04-24
tags:
- ai-adoption
- enterprise-ai
- llm
- strategy
- software-development
- guardrails
- developer-experience
- model-capability
- cloud-costs
- tech-trends
connections:
- '[[Everything is a Tradeoff]]'
- '[[The Bad]]'
ai_generated: false
human_approved: false
category:
- Knowledge Base
- AI
- The Good, The Bad, The Ugly
tagging_processed_count: 1
---
# Velocity

#2026-04 
AI adoption in companies is speed-running patterns we've seen with cloud and data before.

Here's a list of realities coexisting right now:

Some employees are not allowed to use modern AI (coding) tools at all. (Yes, those exist.)

Most companies give employees selected access to chat LLMs, coding harnesses, or both.

Some companies are building chat LLMs and agents grounded in company data. A few of them fine-tune their own custom models.

Some rather lost actors went astray manically building their own "Company Cursor" coding harness after a consultancy told them to two years ago...

Mature companies centralize access to validated models, tools, MCP servers, and skills. They apply security guardrails and observability through a central gateway, even think about agent identity and access control.

**The enlightened ones allow experimentation and individual preferences inside adaptive guardrails. They also follow a deliberate strategy to create room for local breakouts, then pull proven patterns back into the central structure.**

The main challenge has always been to set standards that won't be crushed with the next hyperscaler release or employee demand. Stay flexible enough to adopt what's coming while at the same time start building the foundation for enterprise reliability and security in your internal AI usage.


# One-shotting a nightmare

From my experience, one-shotting apps with a massive and 4h running AI coding sessions is a delusion. The AI generates a compelling surface with a wow effect (except maybe for the frontend design if you're using openAI models). Then you start trying to make it work and the facade starts to crumble before your eyes. 

A much better strategy seems to be implementing small increments, trying to restrain the AI and oneself. Fool-proofing against future regressions, then continuing. This way, stable and maintainable apps are generated. But it takes patience.  

# The Mess It Created

#2026-03 Regressions, bugs, slop is created by most if not the most powerful models. The threshold for actually useful AI coding models is GTP-5.4 or Opus 4.6 imho. Everything else can assist - but don't let them drive or you'll regret. 

## Higher Costs after Cost Efficiency Analyses
Just because S3 docs promote intelligent tiering doesn't mean immediate archiving data is right for this data access patterns in this app. 

Added caching... in a second place on top of existing caching. 

Suggests and focuses on write efficiency measures when reads produce all the costs. 

Constantly underestimates cloud pricing even with exact documentation in context. Hand-waves costly features like Firestore indexing. 

# Overconfidence

Deleting important app files irreversibly, and ...
![[Pasted image 20260323114406.png|224]]

# Dumbing Down

#2026-03 
In a perfect world, you voluntarily decide what it is you want to learn and spend your time on. For the rest of things that might become necessary in the process but are not your selected area of expertise - AI can help with that. For instance, I'm not a good frontend dev and never will be. But all my pet projects need a usable UI. I actively choose to not look into the code in detail. I have no desire to learn it. 

However, the more we use AI, the more obvious it becomes that we don't look into the areas that are at the core of our interest. If AI can just do it without further scrutiny, we might more and more often just choose to let it do it's thing. 

