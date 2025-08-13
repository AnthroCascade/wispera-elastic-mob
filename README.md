# wispera-agents

# Elastic Mob Programming for No-Code Software Development

## Core Vision

We're creating a no-code system that captures and facilitates the conversations that birth systems into existence. Rather than asking users to learn visual programming, they engage in natural language conversations with specialized AI agents that collaboratively build working software.

## Foundation: Skill-Blocks

The system is built on **skill-blocks** - atomic units of expertise that can be composed into agent-stakeholders. These skill-blocks are organized into families:

- **Technical Creation** (Frontend, Backend, Database, DevOps, Mobile, Integration...)
- **Design & Experience** (UX, UI, Information Architecture, Accessibility...)
- **Quality & Reliability** (Testing, Security, Performance, Monitoring...)
- **Communication** (Documentation, Training Materials, API Docs, User Guides...)
- **Process & Coordination** (Requirements Gathering, Project Planning, Prompt Engineering...)
- **Business & Strategy** (Market Analysis, Pricing, Sales Enablement, ROI Analysis...)

Each skill-block:
- Knows its domain of expertise
- Can effectively prompt underlying Gen AI systems
- Recognizes patterns in conversations relevant to its domain
- Understands what outputs it's responsible for
- Can evaluate quality within its domain

## Agent-Stakeholder Specifications (ASSes)

ASSes are composed of collections of skill-blocks, representing the different roles and perspectives needed in software development. Unlike traditional roles, ASSes can be dynamically composed based on project needs.

Critically, each ASS plays different "games" at different levels - from tactical ("make this function work") to strategic ("build sustainable competitive advantage"). These games have different time horizons, success metrics, and optimization targets.

## The Process: Elastic Mob Programming

The development process uses an adaptive mob programming approach where:

1. **All ASSes are always present** but engage dynamically based on relevance
2. **Mob roles rotate** semi-randomly:
   - Driver (implementing)
   - Navigator (directing)
   - Facilitator (process flow)
   - Scout (looking ahead)
   - Housekeeper (maintaining quality)

3. **Participation is stake-based** - ASSes engage more when the conversation touches their skill-blocks' domains

4. **The mob is elastic** - expanding during complex architectural decisions, contracting during routine implementation

5. **High-speed polling** allows the process to constantly check who has relevant input

## Key Innovations

1. **Emergent Participation**: No orchestrator decides who should be involved. The conversation itself summons the right participants through semantic pattern matching.

2. **Holonic Output**: Each iteration produces working software that is complete and valuable at its own level, not merely a step toward the final product.

3. **Conversation as Specification**: The natural language exchanges between ASSes become the living specification, preserving the multidimensional nature of system requirements.

4. **Parallel Games**: Multiple optimization games run simultaneously - Security plays "protect the system" while UX plays "delight the user" - creating natural tension and balance.

## Bootstrap Strategy

We're currently in the primordial bootstrap conversation, designing the archetypes of agent-stakeholders sufficiently to:
1. Implement them using AI
2. Test the quality of their output
3. Use them to build the very system that will coordinate them

The first working system will be the tool that builds itself - a satisfyingly recursive beginning that proves the concept while creating the platform.

---

## Further Reading

For deeper technical and conceptual details about the system architecture, multi-game optimization strategies, and implementation approaches, see **[deeper architecture.md](deeper%20architecture.md)**.

This document explores the sophisticated game-theoretic foundations, semantic pattern matching mechanisms, and the recursive bootstrap strategy that makes this system uniquely powerful.

### Current Status & Audit

For a comprehensive assessment of the current Elastic-Mob implementation status, including identified deficiencies, inconsistencies, and recommended action items, see **[current audit.md](current%20audit.md)**.

This document provides:
- Detailed analysis of skill block and ASS composition
- Identification of missing components and template inconsistencies
- Strategic recommendations for system completion
- Prioritized action plan for moving forward

### Technology & Implementation

For critical analysis of current technology choices and specific recommendations for implementing Elastic-Mob, see **[architecture_critique.md](architecture_critique.md)**.

This document provides:
- Assessment of current Rails backend and Flutter frontend
- Technology recommendations for Elastic-Mob implementation
- Recommended hybrid architecture approach
- Implementation strategy and phase planning