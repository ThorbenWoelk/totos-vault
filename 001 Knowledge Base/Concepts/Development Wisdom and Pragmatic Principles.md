---
created: 2025-07-08
last_edited: 2026-04-24
tags:
- pragmatic-development
- collaborative-development
- interface-driven-development
- parallel-development
- technical-debt
- testing
- performance
- documentation
- tradeoffs
connections:
- - Software Design Principles - Comprehensive Guide
- - Everything is a Tradeoff
- - 22 Development
ai_generated: true
human_approved: false
category:
- Concepts
---
## Context
Pragmatic development wisdom and experience-based principles that complement formal software design principles. These insights come from years of practical development experience and focus on real-world application rather than theoretical perfection.

## Pragmatic Development Philosophy

### "Done is Better Than Perfect" / "Make it Work, Then Make it Right"
**Philosophy**: Prioritize shipping functional solutions over endless optimization.

**Pro**:
- Prevents **analysis paralysis** and perfectionism
- Enables rapid feedback and iteration cycles
- Real user feedback beats theoretical perfection
- Working software delivers value; perfect software often never ships

**Contra**
- Technical debt must be addressed. Sometimes hard to make time for that after feature has been released already. 
- Can feel like building everything twice. 

**Note**
- No alternative to testing main hypotheses in a PoC that never goes live

**Application strategy**:
1. **Make it work**: Focus on core functionality first
2. **Make it right**: Refactor based on actual usage patterns
3. **Make it fast**: Optimize where metrics show it matters

### Collaborative Development Wisdom

#### Code is a Team Sport
**Reality**: Most code is written, read, and maintained by teams, not individuals.

**Practical implications**:
- Write code for the next developer (who might be future you)
- Invest in clear naming and documentation
- Code reviews are conversations, not criticism
- Shared ownership prevents knowledge silos
- Team coding standards matter more than individual preferences

#### Version Control as Communication
**Beyond backup**: Git history tells the story of your project.

**Best practices from experience**:
- Commit messages should explain *why* + *what*
- Small, focused commits enable better debugging
- Branch naming conventions help team coordination and should help commit discipline
- Git blame is for understanding, not assigning fault

### Interface-Driven Development Wisdom

#### "Developers Work Against Interfaces. Interfaces can be mocked." 

As long as the interface is agreed upon early, there's some practical benefits to this mindset: 
- Enables parallel development of dependent components
- Makes unit testing significantly easier
- Reduces coupling between team members' work
- Allows development to proceed before all dependencies are complete

**Implementation wisdom**:
- Define interfaces before implementation and document interface contracts clearly
- Mock external dependencies from day one
- Version interfaces explicitly

### Team Coordination Principles

#### "Parallel Work Shortens Delivery Cycles"

**Enabling strategies**:
- Clear interface definitions allow simultaneous development
- Feature branches enable independent work streams
- API-first development prevents blocking dependencies
- Automated testing enables confident integration
- Good architecture supports parallel development

**Team coordination**:
- Daily standups identify blocking dependencies
- Sprint planning considers team parallelization
- Architecture decisions should enable, not hinder, parallel work
- Communication overhead vs. parallel benefits is a key tradeoff

### Contextual Wisdom

### Code Evolution Wisdom

#### Incremental Improvement Over Wholesale Rewrites
**Experience shows**:
- Big bang rewrites often fail or massively overrun 
- Bit less so in the age of AI - but still matters
- Incremental refactoring is less risky and more sustainable
- *Strangler fig pattern* for legacy system replacement
- Feature flags enable gradual rollouts

#### Technical Debt is Not Always Bad
**Nuanced view**:
- Strategic technical debt can accelerate learning
- Document debt deliberately rather than accidentally
- Plan debt retirement as part of development cycles
- Some debt naturally resolves as requirements clarify

### Testing and Quality Wisdom

#### Test Pyramid in Practice
**Real-world insights**:
- Unit tests should be fast and reliable
- Integration tests catch real-world issues
- End-to-end tests are expensive but valuable for critical paths
- Manual testing still has a place for user experience
- Test what you fear breaking most

#### Code Reviews as Learning Tools
**Beyond bug catching**:
- Share knowledge and patterns across team
- Discuss tradeoffs and alternatives
- Maintain coding standards
- Onboard new team members
- Challenge assumptions constructively

### Performance and Optimization

#### Premature Optimization is Evil, But...
**Balanced approach**:
- Profile before optimizing
- Optimize for the bottlenecks that matter to users
- Simple algorithms often outperform complex ones
- Readability vs. performance is a conscious tradeoff
- Sometimes "fast enough" is fast enough

#### Monitoring Guides Optimization
**Data-driven decisions**:
- Measure what users actually experience
- Optimize based on real usage patterns

### Communication and Documentation

#### Code Documentation Philosophy
**Practical approach**:
- Code should be self-documenting through clear naming
- Comments explain *why*, not *what*
- README files are user manuals for your code
- Architecture decisions records (ADRs) capture context
- Update documentation as part of development, not afterward

#### Cross-Team Communication
**Enabling effective collaboration**:
- APIs are contracts between teams
- Deprecation strategies prevent breaking changes
- Service level agreements set expectations
- Incident postmortems spread learning

### Meta-Wisdom: Context is King

#### Know Your Constraints
**Critical factors that shape decisions**:
- Team size and experience level
- Timeline and business pressures
- Technical landscape and existing systems
- User impact and quality requirements
- Regulatory and compliance needs

#### Principles as Guidelines, Not Laws
**Wisdom application**:
- Every principle has exceptions
- Context determines which principles to prioritize
- Experience teaches when to break rules thoughtfully
- Dogmatic adherence to principles can be counterproductive
- Pragmatism beats purity in most real-world scenarios

## Cross-References

This document complements [[Software Design Principles - Comprehensive Guide]].

All wisdom here connects to [[Everything is a Tradeoff]] by acknowledging that every principle has costs and benefits.

The key insight is that development wisdom comes from understanding not just what to do, but when, why, and how to do it in real-world contexts with real constraints and real teams.
