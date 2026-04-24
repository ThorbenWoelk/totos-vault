---
created: 2026-01-28
last_edited: 2026-04-24
tags:
- engineering-principles
- amdahl-law
- cap-theorem
- kiss-principle
- moore-law
- paxos
- yagni
- distributed-systems
- scalability
connections:
- '[[001 Knowledge Base/Engineering/_index|Engineering Index]]'
- '[[Software Design Principles - Comprehensive Guide]]'
- '[[Everything is a Tradeoff]]'
ai_generated: false
human_approved: false
category:
- Knowledge Base
- Engineering
---
Laws, theorems, and normative statements.

## A

### Amdahl's Law

demonstrates the theoretical maximum speedup of an overall system and the concept of diminishing returns. If exactly 50% of the work can be parallelized, the best possible speedup is 2 times. If 95% of the work can be parallelized, the best possible speedup is 20 times. According to the law, even with an infinite number of processors, the speedup is constrained by the unparallelizable portion.

## C

### CAP theorem / Brewer’s theorem

any distributed data store can provide at most two of the following three guarantees: Consistency, Availability, Partition Tolerance.

## K

### KISS (Keep it simple, stupid)

recommends keeping systems as simple as possible, in line with the quote at the beginning of the chapter. Keeping all building blocks as simple as possible leads to a scalable solution. 

## M

### Moore’s law

is the observation that the number of transistors in an integrated circuit (IC, also: microchip) doubles about every two years. Moore’s law is an observation and projection of a historical trend. Rather than a law of physics, it is an empirical relationship. It is an experience-curve law, a type of law quantifying efficiency gains from experience in production.

## P

### Paxos Law

The Paxos algorithm is a consensus algorithm designed to achieve agreement among a group of distributed or decentralized processes in a network, even if some of those processes are unreliable.

## Y

### YAGNI (You ain’t gonna need it)

warns against building something on the suspicion that you might need it later.
