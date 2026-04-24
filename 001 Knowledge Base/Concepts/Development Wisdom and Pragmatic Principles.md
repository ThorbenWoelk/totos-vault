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

**Why this matters**:
- Prevents **analysis paralysis** and perfectionism
- Enables rapid feedback and iteration cycles
- Real user feedback beats theoretical perfection
- Working software delivers value; perfect software often never ships
- Technical debt can be addressed iteratively

**Application strategy**:
1. **Make it work**: Focus on core functionality first
2. **Make it right**: Refactor based on actual usage patterns
3. **Make it fast**: Optimize where metrics show it matters

**When to apply**: Early-stage products, prototypes, proof-of-concepts, tight deadlines

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
- Commit messages should explain *why*, not *what*
- Small, focused commits enable better debugging
- Branch naming conventions help team coordination
- Merge vs. rebase strategies affect team workflow
- Git blame is for understanding, not assigning fault

### Interface-Driven Development Wisdom

#### "Developers Work Against Interfaces" 
**German wisdom**: "EntwicklerInnen arbeiten gegen Schnittstellen. Schnittstellen kann man mocken."

**Translation**: Developers work against interfaces. Interfaces can be mocked.

**Practical benefits**:
- Enables parallel development of dependent components
- Makes unit testing significantly easier
- Reduces coupling between team members' work
- Allows development to proceed before all dependencies are complete
- Facilitates contract-first development

**Implementation wisdom**:
- Define interfaces before implementation
- Mock external dependencies from day one
- Use dependency injection consistently
- Document interface contracts clearly
- Version interfaces explicitly

### Team Coordination Principles

#### "Parallel Work Shortens Delivery Cycles"
**German insight**: "Paralleles Arbeiten verkürzt den Lieferzyklus."

**Translation**: Parallel work shortens the delivery cycle.

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

#### "Move Fast and Break Things" - With Wisdom
**When appropriate**:
- Early-stage products finding product-market fit
- Internal tools with low external impact
- Experimental features with limited scope
- Learning and discovery phases

**Required safeguards**:
- Good monitoring and alerting
- Quick rollback capabilities
- Clear blast radius boundaries
- Learning from what breaks

**When NOT appropriate**:
- Production systems with user dependencies
- Financial or safety-critical applications
- Established products with stable user base

#### The "Good Enough" Principle
**Insight**: Perfect is the enemy of done, but "good enough" requires clear standards.

**Defining "good enough"**:
- Meets user requirements and acceptance criteria
- Passes agreed-upon quality gates
- Has acceptable performance characteristics
- Includes necessary error handling
- Is maintainable by the team

### Code Evolution Wisdom

#### Incremental Improvement Over Wholesale Rewrites
**Experience shows**:
- Big bang rewrites usually fail or massively overrun
- Incremental refactoring is less risky and more sustainable
- Strangler fig pattern for legacy system replacement
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
- Set performance budgets and stick to them
- Synthetic monitoring vs. real user metrics

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

## Integration with Formal Principles

### Relationship to Software Design Principles
This document complements [[Software Design Principles - Comprehensive Guide]] by:
- Providing practical application wisdom for formal principles
- Sharing experience-based insights on when and how to apply principles
- Offering pragmatic alternatives when formal approaches are too heavy
- Bridging the gap between theory and practice

### Tradeoff Awareness
All wisdom here connects to [[Everything is a Tradeoff]] by:
- Acknowledging that every principle has costs and benefits
- Providing context for when tradeoffs are worth making
- Sharing experience about how tradeoffs play out in practice

The key insight is that development wisdom comes from understanding not just what to do, but when, why, and how to do it in real-world contexts with real constraints and real teams.

