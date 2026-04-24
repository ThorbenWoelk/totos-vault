---
created: 2020-03-23
last_edited: 2026-04-24
tags:
  - clean-code
  - software-design
  - programming
  - functions
  - data-structures
  - error-handling
  - testing
  - object-oriented
connections:
- '[[Safranski (2013) - Goethe]]'
- '[[08 Advanced Features]]'
- '[[Agentic AI]]'
ai_generated: false
human_approved: false
category:
  - Notes
  - Books
tagging_processed_count: 1
tagging_last_processed: 2026-04-24
---
# Functions
## Arguments
Avoid to have too many/ more than 3  
  - put them in classes together  

Can actually be input AND output. Don't do output (`appendFooter(report)`)  
  - call the method owned by the object/class to change itself: `report.appendFooter()`  

## Behavior
Avoid side effects. They can result in *temporal couplings*, meaning you can only call the function at certain times  
  - change `this` only  
Command/query separation: Either you check on something or you change it  

## Explain yourself
E.g. by function names  

# Data structures and objects
## Defs
An object, in contrast to a mere data structure, hides its data and only exposes operations on it  

## Hiding data
In practice, *Interfaces* help hiding private vars (of implementation) by defining entry points to the data  

# Error handling
## Structure
Seperate exception handling from business logic (try/catch clause from function / behavior)
Use unchecked exceptions (Java)  
Wrap third-party APIs  
To declutter exception handling from logic, use SPECIAL CASE PATTERN: Instead of an exception to be handled, the object's behavior accounts for the special case

# Boundaries
## Learning external APIs/packages
Can be done by learning tests
ADAPTER: helps converting from perfect interface to provided interface

# Unit testing
## GIVEN, THEN structure
Tests should be readable like this:
Given the following situation/assumptions, the function should do the following.

# Classes  
## Should be small and cohesive and inherit from another

