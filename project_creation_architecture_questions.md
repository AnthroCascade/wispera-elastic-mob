# Game Creation Architecture Questions

## Context

After cleaning up artificial complexity around "project types", we now have a clearer understanding of what elastic-mob game creation actually is: **scaffolding new software projects that will be developed through AI-powered capers**.

## Current State

### What We Have
1. **Game** - represents software projects being built through AI-powered collaborative capers
2. **Syndicate** - collections of specializations (fortes) imported from Git repositories
3. **Mobsta** - AI agent instances created from syndicate fortes for specific games
4. **Caper** - collaborative AI development sessions within a game  
5. **Git integration** - version control for generated code
6. **Basic scaffolding framework** - standard project structure (src, tests, docs, config)

### What's Missing/Incomplete
- `create_initial_project_structure` method is essentially empty
- Helper methods (`initial_readme_content`, `standard_gitignore_content`, `project_metadata_content`) are referenced but not implemented
- No clear relationship to wispera ecosystem integration

## Architecture Decisions & Answers

### 1. Game Types & Scaffolding ✅ RESOLVED
**Game type is arbitrary and determined case-by-case in tech stack declarations**
- No predetermined project types or rigid templates
- Technology stack specified in `tech_stack` array drives scaffolding decisions
- Could be Flutter, Rails, Node.js, Python, or any combination
- Flexible approach adapts to declared technology choices

### 2. AI Integration Architecture ❓ NEEDS CLARIFICATION
**How does the scaffolding integrate with the AI code generation system?**
- Does the initial structure influence AI code generation patterns?
- Are there specific files/folders that AI mobstas expect to find?
- How does project structure guide AI collaboration flows?

### 3. Wispera Ecosystem Relationship ✅ PARTIALLY RESOLVED
**Mobsta app interacts with wispera ecosystem through Rails server components**
- Could create wispera-based applications, but not always
- Not limited to Flutter - wispera ecosystem works across technologies
- Integration happens through implemented components in the Rails server
- Not direct Flutter-to-framework integration

### 4. Template Strategy ✅ RESOLVED
**Dynamic scaffolding based on tech_stack declarations**
- No fixed templates - structure determined by project specifications
- Case-by-case basis driven by `tech_stack` array contents
- Flexible system that adapts to technology requirements
- Example: `['rails', 'postgresql']` creates different structure than `['flutter', 'dart']`

### 5. Development Environment Integration ❓ NEEDS CLARIFICATION
**How does project creation establish the development environment?**
- Integration with development terminal panels?
- Game context panel initialization?
- IDE layout and workspace setup?

## Remaining Open Questions

### Critical Implementation Questions
1. **How does the tech_stack array drive scaffolding decisions?**
   - What structure does `tech_stack: ['rails', 'postgresql']` create vs `tech_stack: ['flutter', 'dart']`?
   - How granular should tech stack detection be?

2. **How does project creation integrate with mobsta development interface?**
   - Game context panel initialization and setup
   - Development terminal panel integration
   - IDE layout and workspace configuration

## Implementation Priorities

Based on resolved architecture decisions:

### Immediate Next Steps
1. **Tech-stack-driven scaffolding** - implement dynamic project structure based on `tech_stack` array
2. **Git integration completion** - finish repository initialization with proper initial commits
3. **Mobsta interface integration** - connect project creation to development panels
4. **AI collaboration optimization** - ensure project structure supports AI code generation
