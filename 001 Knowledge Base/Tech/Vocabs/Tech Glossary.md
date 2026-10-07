---
created: 2026-08-09
last_edited: 2026-08-09
tags:
  - glossary
connections: []
ai_generated: false
human_approved: false
category:
  - Knowledge Base
  - Tech
  - Vocabs
---
## A
### ACID
#### Atomicity

Ensures that all operations within a transaction are treated as a single unit

Either all operations complete successfully, or none do (all-or-nothing)

If any part fails, the entire transaction is rolled back
#### Consistency

Guarantees that a transaction can only bring the database from one valid state to another

Ensures all data integrity constraints are maintained

Prevents transactions from creating invalid data
#### Isolation

Ensures that concurrent execution of transactions leaves the database in the same state as if they were
executed sequentially

Prevents transactions from interfering with each other

Different isolation levels provide tradeoffs between performance and strictness
#### Durability

Guarantees that once a transaction is committed, it remains committed even in case of system failure

Changes made by committed transactions are permanent

Typically implemented using transaction logs
Example
Imagine transferring $100 from Account A to Account B:

Atomicity: Either both the debit from A and credit to B succeed, or neither happens

Consistency: The total money in both accounts remains the same before and after

Isolation: Another transaction cannot see the money in transit

Durability: Once confirmed, the transfer persists even if the system crashes immediately after
### ACME
( Placeholder ) A common placeholder name used in diagrams and discussions to represent a generic company or
system. It does not stand for a specific, universally recognized acronym in this context.
### Agent
In AI, an agent is a system that can perceive its environment, reason about it, make decisions, and take
actions to achieve a goal. AI agents are often designed to exhibit some degree of autonomy.
### Agentic Workflow
A structured and predefined sequence of steps or tasks performed by an AI system or agent. These are often
more predictable and less flexible than autonomous agents, but effective for automating specific processes.
### Agent-to-Agent (A2A) Protocol
A protocol that defines how different AI agents can communicate and collaborate with each other.
### Apache Flink
is a powerful open-source stream processing framework designed for distributed, high-performance, and
fault-tolerant data processing.
### Apache Iceberg
is an open-source table format for large analytical datasets that was originally developed by Netflix and
later donated to the Apache Software Foundation.
Key features and benefits of Apache Iceberg include:
- Schema evolution - It allows you to add, drop, rename, or update column types without requiring data
rewrites
- Partition evolution - You can change how data is partitioned over time without needing to rewrite data
- Time travel and versioning - You can access previous versions of tables and roll back to earlier states if
needed
- ACID transactions - Iceberg provides atomicity, consistency, isolation, and durability guarantees for table
operations
- Hidden partitioning - It separates the logical data model from the physical storage layout
- High performance - It includes file-level statistics and filtering to improve query speed
- Compatibility - Iceberg works with various data processing frameworks including Apache Spark, Apache Flink,
Apache Hive, Presto, and Trino
### Autonomous Agent
An AI agent designed to operate with a high degree of independence, capable of planning, adapting, and
learning in dynamic environments without constant human intervention.
## B
### BASE
Basically Available, Soft State, and Eventual Consistency, in contrast to traditional ACID properties.
## C
### ChromaDB
ChromaDB is an open-source, lightweight vector database designed for applications involving large language
models (LLMs) and other AI applications.
### Chunking
The process of dividing longer documents or text into smaller, manageable sections (chunks) that fit within an
LLM's context window. These chunks are often what are embedded and stored in a vector database.
### Context Window
The maximum number of tokens that an LLM can process at one time as input. Understanding the context window is
important for chunking documents in RAG and designing agent interactions.
## D
### Data Mesh
Data mesh is a sociotechnical approach to building a decentralized data architecture by leveraging a
domain-oriented, self-serve design (in a software development perspective), and borrows Eric Evans’ theory of
domain-driven design[1] and Manuel Pais’ and Matthew Skelton’s theory of team topologies.
### Dot
Dot, the AI Data Analyst, answers data questions for your team from Slack and in chat. Empowers everyone to
get instant, actionable insights.
## E
### Embeddings (Vector Embeddings)
Numerical representations (vectors) of text that capture semantic meaning. Text with similar meanings have
embeddings that are closer in the vector space.
## F
### Fine-tuning
The process of further training a pre-trained LLM on a smaller, specific dataset to adapt it to a particular
task or domain.
## G
### Grounding
The process of connecting the output or understanding of an AI model to real-world data or established facts.
RAG is a method of grounding LLM responses by connecting them to a knowledge source.
## H
### Hallucination
In AI, particularly with LLMs, hallucination refers to the phenomenon where the model generates information
that is factually incorrect, nonsensical, or not supported by the input data or its training knowledge. RAG is
a technique used to reduce hallucination by providing grounded context.
## I
### Interoperability
Interoperability, in the context of the Agent2Agent (A2A) Protocol, refers to the ability to connect agents
built on different platforms—such as LangGraph, CrewAI, Semantic Kernel, or custom solutions—to create
powerful, composite AI systems
## L
### LLM (Large Language Model)
A type of AI model (like GPT, LaMDA, etc.) trained on vast amounts of text data. LLMs are capable of
understanding, generating, and manipulating human language. They often serve as the "brain" for AI agents.
## M
### Mongo DB
MongoDB is a NoSQL database / document db. Most notably, the flexible schema allows for varying schemas across
records.
### MPP
Massive parallel processing. Shorthand for dbs like Snowflake and BigQuery.
## N
### nao
Cursor for data teams. LLM used is trained to understand dbt and has db connection as context. How does it
ensure the queries won't be costly?

## O
### Object-relational impedance mismatch
refers to the fundamental disconnect between object-oriented programming models and relational database
systems. This mismatch is why ORMs (Object-Relational Mappers) like Hibernate, Entity Framework, or SQLAlchemy
were developed - to bridge this gap by providing tools that automatically handle the translation between
objects in code and relational database structures.
### Orchestration (of Agents)
The process of coordinating and managing the interactions between multiple AI agents or between an agent and
external tools to achieve a larger goal.
## P
### Prompt Injection
A security vulnerability where malicious input ("prompts") is crafted to manipulate an LLM into performing
unintended actions or revealing confidential information. A key consideration when connecting LLMs to tools
via protocols like MCP.
## R
### Retrieval-Augmented Generation (RAG)
A technique that combines the power of LLMs with an external knowledge retrieval system to provide more
accurate and up-to-date information in generated text.
## S
### Sampling
In the context of Large Language Models and text generation, sampling refers to the process of choosing the
next token in a generated sequence from a probability distribution over the vocabulary. Different sampling
techniques influence the creativity and randomness of the generated text.
### Schema
A formal description of the structure and format of data. In MCP, schemas (often in .proto files) define the
structure of messages exchanged between agents and tools.
### SSE
Server-Sent Events (SSE) is a web standard that enables a server to push real-time data to web clients over a
single HTTP connection. Once established, the connection remains open and the server can continuously stream
updates to the client without the client needing to repeatedly poll for new information. SSE uses a simple
text-based protocol where the server sends formatted messages (with fields like data:, event:, and id:) over
an HTTP response that never closes. This makes it ideal for applications requiring live updates like chat
systems, real-time notifications, or streaming data feeds. The client receives these events asynchronously and
can process them as they arrive, creating a persistent, unidirectional communication channel from server to
client. In the case of an MCP server, e.g. Pieces OS is the MCP server - it's the application running in the
background that exposes tools and data (your Long-Term Memory, code snippets, conversation history, etc.)
through the MCP protocol. Pieces OS listens on localhost:39300 and provides the SSE endpoint at
/model_context_protocol/2024-11-05/sse. Cursor IDE is the client. SSE is not supported, yet, by Claude. You
need to use mcp-remote npx package to bridge that.
### Stdio
Standard Input/Output (stdio) is a fundamental computing concept that refers to the default communication
channels between a program and its environment. In the context of MCP servers, stdio transport means the
client and server communicate by writing to each other's standard input stream and reading from standard
output streams - essentially using text-based messages passed through command-line pipes. When a program is
launched, it automatically gets three streams: stdin (for receiving input), stdout (for sending output), and
stderr (for error messages). This creates a direct, synchronous communication channel where the client can
send a request via the server's stdin, and immediately read the response from the server's stdout. It's
simple, lightweight, and works well for local processes but requires the server to run as a child process of
the client. Operating systems don't allow random processes to hijack another process's stdin/stdout for
security reasons.
## T
### tldraw
An open-source canvas app: https://tldraw.dev/.
### Tokenization
The process of breaking down text into smaller units called tokens, which are the fundamental units that LLMs
and other natural language processing models process.
### Tokens
The small units of text (often words or sub-word units) that result from the tokenization process.
### Tool (in the context of Agents/MCP)
An external system, service, or function that an AI agent can interact with to perform specific actions or
access specific data. This could be a database, an API, a calculator, a web search engine, etc.
## V
### Vector Database
A specialized database designed to efficiently store, index, and search for vector embeddings based on their
similarity. Crucial for the retrieval component of RAG.
 (showing 0-100 of 150 items)
