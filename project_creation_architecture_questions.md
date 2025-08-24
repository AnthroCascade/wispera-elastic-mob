# Project Creation Architecture Questions

## Context

After cleaning up artificial complexity around "project types", we now have a clearer understanding of what elastic-mob project creation actually is: **scaffolding new software projects that will be developed through AI-powered mob sessions**.

## Current State

### What We Have
1. **MobProject** - represents software projects being built through AI-powered collaborative sessions
2. **MobSession** - collaborative AI development sessions within a project  
3. **Git integration** - version control for generated code
4. **Basic scaffolding framework** - standard project structure (src, tests, docs, config)

### What's Missing/Incomplete
- `create_initial_project_structure` method is essentially empty
- Helper methods (`initial_readme_content`, `standard_gitignore_content`, `project_metadata_content`) are referenced but not implemented
- No clear relationship to wispera ecosystem integration

## Architecture Decisions & Answers

### 1. Project Types & Scaffolding ✅ RESOLVED
**Project type is arbitrary and determined case-by-case in tech stack declarations**
- No predetermined project types or rigid templates
- Technology stack specified in `tech_stack` array drives scaffolding decisions
- Could be Flutter, Rails, Node.js, Python, or any combination
- Flexible approach adapts to declared technology choices

### 2. AI Integration Architecture ❓ NEEDS CLARIFICATION
**How does the scaffolding integrate with the AI code generation system?**
- Does the initial structure influence AI code generation patterns?
- Are there specific files/folders that AI mobsters expect to find?
- How does project structure guide AI collaboration flows?

### 3. Wispera Ecosystem Relationship ✅ PARTIALLY RESOLVED
**Mobster app interacts with wispera ecosystem through Rails server components**
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
- Project context panel initialization?
- IDE layout and workspace setup?

## Remaining Open Questions

### Critical Implementation Questions
1. **How does the tech_stack array drive scaffolding decisions?**
   - What structure does `tech_stack: ['rails', 'postgresql']` create vs `tech_stack: ['flutter', 'dart']`?
   - How granular should tech stack detection be?

2. **How does project creation integrate with mobster development interface?**
   - Project context panel initialization and setup
   - Development terminal panel integration
   - IDE layout and workspace configuration

## Implementation Priorities

Based on resolved architecture decisions:

### Immediate Next Steps
1. **Tech-stack-driven scaffolding** - implement dynamic project structure based on `tech_stack` array
2. **Git integration completion** - finish repository initialization with proper initial commits
3. **Mobster interface integration** - connect project creation to development panels
4. **AI collaboration optimization** - ensure project structure supports AI code generation

### Implementation Approach
- **Flexible scaffolding system** that reads `tech_stack` and creates appropriate structure
- **No rigid templates** - dynamic generation based on technology requirements
- **Rails server mediated integration** with wispera ecosystem when needed
- **Case-by-case project setup** driven by specifications rather than predetermined types
