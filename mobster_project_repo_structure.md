# Mobsta Setup Repository Structure

## Overview

A mobsta project repository contains both the **collaboration infrastructure** (configs, documentation, mobsta contexts) and the **generated code artifacts** organized by solution architecture.

## Repository Structure

```
mobsta-project-repo/
├── config/
│   ├── project.json              # Setup-level configuration
│   ├── sessions/                 # Session configurations
│   │   ├── session-001.json
│   │   ├── session-002.json
│   │   └── ...
│   └── mobstas/                 # Individual mobsta configurations
│       ├── architect.json
│       ├── backend-dev.json
│       ├── frontend-dev.json
│       └── ...
├── docs/
│   ├── global/                   # Setup-wide documentation
│   │   ├── requirements.md
│   │   ├── architecture.md
│   │   └── decisions.md
│   └── mobstas/                 # Mobsta-specific documentation
│       ├── architect/            # Documentation organized by mobsta interest
│       │   ├── system-design.md
│       │   └── patterns.md
│       ├── backend-dev/
│       │   ├── api-specs.md
│       │   └── database-design.md
│       ├── frontend-dev/
│       │   ├── ui-mockups.md
│       │   └── user-flows.md
│       └── devops/
│           ├── deployment.md
│           └── infrastructure.md
└── code/                         # Generated code organized by solution architecture
    ├── frontend/                 # Front-end applications
    │   ├── web-app/
    │   ├── mobile-app/
    │   └── admin-dashboard/
    ├── backend/                  # Back-end services
    │   ├── api-server/
    │   ├── auth-service/
    │   └── data-processor/
    ├── infrastructure/            # Infrastructure as Code
    │   ├── terraform/
    │   ├── kubernetes/
    │   └── docker/
    ├── shared/                   # Shared libraries and utilities
    │   ├── common-types/
    │   ├── utilities/
    │   └── configs/
    └── scripts/                  # Build, deployment, and utility scripts
        ├── build/
        ├── deploy/
        └── utils/
```

## Configuration Files

### Setup Configuration (`config/project.json`)
```json
{
  "name": "project-name",
  "description": "Setup description",
  "tech_stack": ["rails", "react", "postgresql", "docker"],
  "created_at": "2024-01-15T10:00:00Z",
  "updated_at": "2024-01-15T10:00:00Z",
  "active_mobstas": ["architect", "backend-dev", "frontend-dev"],
  "solution_architecture": {
    "frontend": ["web-app", "mobile-app"],
    "backend": ["api-server", "auth-service"],
    "infrastructure": ["docker", "kubernetes"]
  }
}
```

### Session Configuration (`config/sessions/session-001.json`)
```json
{
  "session_id": "session-001",
  "started_at": "2024-01-15T10:00:00Z",
  "completed_at": "2024-01-15T12:30:00Z",
  "focus": "Initial API design and database schema",
  "participating_mobstas": ["architect", "backend-dev"],
  "commits": ["abc123", "def456", "ghi789"],
  "outcomes": [
    "Created user authentication API",
    "Designed database schema",
    "Set up initial project structure"
  ]
}
```

### Mobsta Configuration (`config/mobstas/backend-dev.json`)
```json
{
  "mobsta_type": "backend-developer",
  "expertise_areas": ["api-design", "database-modeling", "performance"],
  "active_contexts": [
    "docs/mobstas/backend-dev/api-specs.md",
    "docs/global/requirements.md"
  ],
  "learning_state": {
    "project_patterns": ["REST API", "JWT auth", "PostgreSQL"],
    "recent_decisions": ["chose FastAPI over Flask", "PostgreSQL over MongoDB"]
  },
  "collaboration_preferences": {
    "works_well_with": ["architect", "devops"],
    "communication_style": "technical-detailed"
  }
}
```

## Documentation Organization

### Global Documentation (`docs/global/`)
- **Setup-wide context** that all mobstas need
- **Requirements, architecture decisions, constraints**
- **Cross-cutting concerns and shared understanding**

### Mobsta-Specific Documentation (`docs/mobstas/[mobsta-name]/`)
- **Organized by mobsta interest and expertise**
- **Enables focused search and context loading**
- **Reduces noise - each mobsta sees relevant docs first**
- **Supports specialized knowledge accumulation**

## Code Organization

### Solution Architecture Alignment
- **Frontend, backend, infrastructure** separation
- **Multiple projects per category** (web-app, mobile-app, etc.)
- **Shared libraries and utilities** for cross-project code
- **Scripts for build, deployment, utilities**

### Benefits for Mobsta Collaboration
- **Clear ownership boundaries** - frontend mobsta focuses on frontend/
- **Solution architecture drives organization** - matches real-world patterns
- **Scalable structure** - can add new projects within categories
- **Search optimization** - mobstas know where to look for relevant code

## Search and Discovery Benefits

### Hierarchical Context Loading
1. **Global context** - always relevant
2. **Mobsta-specific context** - filtered by expertise
3. **Code context** - organized by solution component
4. **Session context** - temporal organization of decisions

### Optimized for AI Consumption
- **Structured paths** enable focused document loading
- **Mobsta interest alignment** reduces irrelevant context
- **Solution architecture organization** matches code generation patterns
- **Git history** provides temporal context for all changes

This structure supports both **efficient mobsta collaboration** and **organized code generation** while maintaining **searchable, version-controlled knowledge evolution**.
