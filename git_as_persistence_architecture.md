# Git as Persistence Architecture for Mobster Projects

## Core Architectural Insight

**If git files work for code output, then files should work for AI input.**

This symmetry of representation - same medium for input and output - aligns perfectly with how AI processes information and how developers actually work.

## Mobster Project State as IDE Workspace

### Conceptual Model
- **Mobster project state = IDE workspace configuration**
- **Document-based corpus** provides context for AI reasoning
- **JSON for machine-readable state, Markdown for human-readable context**
- **Mixed media corpus** - exactly what AI needs to generate code

### File-Based Knowledge Representation
- **Unstructured input** (conversations, docs, requirements) stored as files
- **Structured output** (code, configurations) generated from file corpus
- **Version control** of entire knowledge evolution
- **Branching and rollback** for experimentation and backtracking

## Mob Coordination Model

### Single Writer Principle
- **Only one person at the keyboard** - classic mob programming pattern
- **Mobsters don't edit documents individually** - they participate in the mob
- **Mob writes output collectively** - not concurrent individual edits
- **User provides input** to satisfy mobster information needs

### AI Mob Execution Process
- **Mobsters coordinate through semantic resonance** during high-speed execution
- **Conflict resolution built into mob process** - not git merge conflicts
- **Collective intelligence** produces single consensus output stream
- **Sequential commits** representing mob decisions and reasoning

## Elegant Workflow

### Iterative Development Cycle
1. **User adds documents** (requirements, context, conversations) to git repo
2. **Mob consumes document corpus** during AI-speed collaboration session
3. **Single atomic commit** contains both:
   - Generated code changes
   - Updated mobster understanding/context documents
4. **Repeat iteratively** as project evolves

### Version Control Benefits
- **Clean linear history** of mob reasoning and decisions
- **Rollback entire collaboration states** - not just code
- **Branch experimentation** - try different approaches
- **Atomic commits** link code changes with reasoning context

## Architectural Advantages

### Perfect AI Alignment
- **Document corpus consumption** matches AI training patterns
- **File-based reasoning** natural for LLM processing
- **Version-controlled knowledge** enables learning and backtracking
- **IDE-like state management** familiar to developers

### Simplified Concurrency
- ✅ **No concurrent editing conflicts** - single mob output
- ✅ **No complex coordination protocols** - semantic resonance handles it
- ✅ **Clean version history** - sequential mob decisions
- ✅ **Natural AI workflow** - consume docs → produce code

### Knowledge Persistence
- **Entire project evolution** captured in git history
- **Context and code co-evolve** in same repository  
- **Mobster learning** preserved across sessions
- **Experiment safely** with branching and rollback

## Implementation Notes

### Deferred Concerns
- **Query and search performance** - valid concern but deferred for simplicity
- **Full-text vs semantic search** - to be addressed in implementation
- **Cross-document relationships** - handled through mob semantic reasoning

### Key Insight
The mob coordination model eliminates the major concurrency and conflict resolution challenges that would make git-as-persistence complex. Since mobsters coordinate through semantic resonance and produce consensus output, git becomes a clean, linear record of mob intelligence evolution.

## Revolutionary Potential

This architecture could fundamentally change how AI development workflows operate by treating **files as the natural interface between human input and AI reasoning**, with git providing the version control and experimentation capabilities that both humans and AI need for iterative development.
