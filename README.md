# wispera-agents

# Elastic Mob Programming for No-Code Software Development

## Core Vision

We're creating a no-code system that captures and facilitates the conversations that birth systems into existence. Rather than asking users to learn visual programming, they engage in natural language conversations with specialized AI agents that collaboratively build working software.

## Foundation: Skill-Blocks

The system is built on **skill-blocks** - atomic units of expertise that can be composed into agent-stakeholders. We currently have **37 skill blocks** organized into families:

- **Technical Creation** (Frontend, Backend, Database, DevOps, Mobile, Integration...)
- **Design & Experience** (UX, UI, Information Architecture, Accessibility...)
- **Quality & Reliability** (Testing, Security, Performance, Monitoring...)
- **Communication** (Documentation, Training Materials, API Docs, User Guides...)
- **Process & Coordination** (Requirements Gathering, Project Planning, Prompt Engineering...)
- **Business & Strategy** (Market Analysis, Pricing, Sales Enablement, ROI Analysis...)
- **Architectural Principles** (Secure-by-Default, Type-Enforced Security, Performance Architecture...)
- **Code Generation** (Agreement Recognition, Context Translation, Generation Triggering, Quality Validation)

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

5. **AI-Powered Code Generation**: When the mob reaches consensus, specialized skill blocks automatically translate architectural decisions into code generation context, ensuring that all principles are preserved in the final implementation.

## Bootstrap Strategy

We've completed the **foundation phase** with a comprehensive set of 37 skill blocks and 25 agent-stakeholders. The system now includes:

1. **Complete skill block coverage** across all major development domains
2. **AI-powered code generation** system that preserves architectural principles
3. **Integrated architectural principles** (security, performance, quality) declared at specification time
4. **Coordinated mob conversation** system with process facilitation and agreement recognition

**Current Status**: Foundation complete, ready for implementation and testing.

**Next Phase**: Building the actual Elastic-Mob system using the skill blocks and ASSes we've designed.

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

### Research & User Experience

For research into non-technical user experiences with LLM-guided software development, see our research knowledge base:

- **[non_technical_user_experiences.md](docs/non_technical_user_experiences.md)** - Curated knowledge base of successful patterns, processes, and user experiences
- **[research_findings.md](docs/research_findings.md)** - Raw research data, case studies, and specific findings
- **[research_process_guide.md](docs/research_process_guide.md)** - Practical guide for conducting research and gathering user experience data

These documents provide:
- Successful process patterns and methodologies
- Curated prompt scripts and templates
- Case studies and failure analysis
- Tool combinations and learning curves
- Research methodology and platform strategies
- Integration opportunities with elastic-mob design

### Technology & Implementation

For critical analysis of current technology choices and specific recommendations for implementing Elastic-Mob, see **[architecture_critique.md](architecture_critique.md)**.

This document provides:
- Assessment of current Rails backend and Flutter frontend
- Technology recommendations for Elastic-Mob implementation
- Recommended hybrid architecture approach
- Implementation strategy and phase planning

### Implementation Approach

For our agreed implementation strategy and technical architecture, see **[implementation_approach.md](implementation_approach.md)**.

This document provides:
- **Agreed architecture** - integrated engine within in-concert
- **Frontend architecture** - plain JS + web components with holonic design
- **Implementation phases** - 3-phase development plan (6-8 weeks)
- **Technical details** - user isolation, git integration, LLM service access
- **Component design** - layered architecture (layouts → partials → web components)
- **User experience model** - single user focus with autonomous operation
- **Success criteria** - measurable outcomes for validation

### Elastic-Mob Persona

For the meta-instruction persona that enables any AI assistant to simulate elastic-mob behavior, see **[elastic_mob_persona.md](elastic_mob_persona.md)**.

This document provides:
- **Meta-ASS orchestration** instructions for AI assistants
- **Response structure** patterns for elastic-mob simulation
- **ASS activation rules** and skill block integration
- **Consensus building process** and code generation triggering
- **Behavioral guidelines** and success metrics
- **Usage instructions** for immediate elastic-mob experience

### Persona Optimization

For continuous improvement of the elastic-mob persona based on real-world performance feedback, see **[persona_optimizer.md](persona_optimizer.md)**.

This document provides:
- **Problem analysis framework** for persona performance issues
- **Root cause identification** and solution design
- **Specific persona modifications** to address failures
- **Quality assurance guidelines** for maintaining system integrity
- **Iteration planning** for continuous persona evolution

### Meta-ASS Architecture

For analysis of self-improvement capabilities and the balance between self-awareness and system stability, see **[meta_ass_architecture.md](meta_ass_architecture.md)**.

This document provides:
- **Meta-ASS vs. external orchestrator** analysis and trade-offs
- **Hybrid approach** for self-awareness without circular complexity
- **Layered meta-architecture** design principles
- **Implementation strategy** for emergent optimization
- **Stability considerations** for evolving systems