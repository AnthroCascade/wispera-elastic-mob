# Mobstas - Overview

## What Are Mobstas?

**Mobstas** are AI agent instances created from fortes at game time. They represent various perspectives, roles, and archetypes that participate in elastic-capers, combining the expertise defined in their source forte with the specific requirements of the current game.

### **Composition Hierarchy**:
```
Talents → Fortes → Mobstas
   ↓        ↓        ↓
 Atomic  Composed  Game
 Units   Expertise Instances
```

**Key Point**: Mobstas are **instantiated** from fortes (which are composed from talents), not directly from talents.

## Current Mobstas

The complete, detailed definitions of all mobstas are now maintained in the **in-concert repository** under `app/services/elastic_mob/definitions/mobstas/`.

### Explicitly Named Roles
- **Domain Expert** - Subject matter expertise in specific domains
- **Operations Person** - Understands constraints and edge cases
- **Compliance Officer** - Regulatory and compliance considerations
- **End User** - User experience and usability perspective
- **Security Expert** - Security and privacy concerns
- **UI/UX Designer** - User interface and experience design
- **Finance Person** - Financial and budgetary considerations
- **Engineer/Developer** - Technical implementation perspective
- **Product Owner** - Product vision and requirements
- **Marketing** - Market positioning and user acquisition
- **Sales** - Customer needs and sales considerations
- **CEO** - Strategic business perspective
- **Sponsor** - Game sponsorship and support
- **Component Architecture** - Client-side development expertise
- **API Design** - Server-side development expertise
- **DevOps Engineer** - Operations and deployment expertise
- **Quality Assurance** - Quality assurance and testing
- **Technical Writer** - Documentation and communication
- **Product Manager** - Product strategy and lifecycle

### Abstract Archetypes
- **The Pragmatist** - Cares about "does it work today?"
- **The Architect** - Cares about "will it work tomorrow?"
- **The User Advocate** - Cares about "will anyone want to use it?"
- **The Resource Guardian** - Cares about "can we afford to build and run it?"

## Important Notes

- **Composable Fortes** - We moved away from fixed role definitions toward fortes composed from talents
- **Dynamic Instantiation** - Mobstas are dynamically instantiated from fortes at game time based on project needs
- **Flexible Scenarios** - Supports both startup scenarios (agents wearing multiple hats) and enterprise scenarios (highly specialized agents)
- **Emergent Participation** - All Mobstas include emergent participation patterns for semantic resonance

### Enhanced Mobsta Specifications
All Mobstas now include **emergent participation patterns** for semantic resonance:
- **Activation Thresholds** - Relevance scores that determine when Mobstas participate
- **Participation Styles** - How Mobstas emerge and engage in conversations
- **Collaboration Patterns** - Natural interaction with other relevant Mobstas
- **Exit Conditions** - When Mobstas disengage from conversations
- **Conflict Resolution** - How Mobstas handle disagreements with other perspectives

## For Detailed Definitions

See the in-concert repository at `app/services/elastic_mob/definitions/mobstas/` for complete specifications of each mobsta.
