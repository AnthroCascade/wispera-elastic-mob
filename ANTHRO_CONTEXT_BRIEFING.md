# Anthro Super-Game: Complete Context Briefing
## AI Assistant Context Transfer Document

**Purpose**: Rapidly transfer comprehensive understanding of anthro super-project to fresh AI assistant contexts.

---

## 🎯 **System Overview**

**What We're Building**: Conversational programming platform where natural language becomes working software through AI agent collaboration.

**Architecture**: 8 interconnected repositories with Rails backend, Flutter frontend, and comprehensive AI specifications.

---

## 📁 **Repository Structure & Status**

### **Backend (Mature ✅)**
- **in-concert**: Rails 7.2 backend with ElasticMob engine
  - Models: Game, Syndicate, Mobsta, Forte, Caper, GeneratedFile, GitCommit
  - API: `/api/elastic_mob/games`, `/api/elastic_mob/syndicates`, `/api/elastic_mob/capers`
  - LLM Integration: Anthropic, OpenAI, DeepSeek
  - Status: Foundation complete, ready for activation across any collaborative domain

### **Frontend (Integration Needed 🚧)**
- **wispera-mobsta**: Flutter IDE-style app for collaborative AI interaction
  - Role: Thin orchestration layer over wispera_components
  - Status: Foundation ready, needs backend integration
  - Architecture: Should follow Wall/Rack/Board/Bench patterns

### **Framework Layer (Mature ✅)**
- **wispera_framework**: Core Flutter UI framework
  - Widgets: Wall, Rack, Board, Bench (containers) + Brick, Beam (items)
  - Patterns: Repository pattern, Shipment state management
  - Theme: Consistent Wispera design language

- **wispera_components**: Business logic and reusable components
  - Structure: State/Repository/Widget/Prototype classes per component
  - Status: ✅ Architecturally sound, properly implements framework patterns
  - Pattern: Clean architecture with clear boundaries

### **Reference Implementation (Mature ✅)**
- **wispera-flutter**: Reference app showing framework usage
  - Documentation: Comprehensive architectural guides
  - Purpose: Demonstrates secure AI integration patterns

### **Specifications (Mature/Ongoing ✅)**
- **elastic-mob**: Research and system documentation
  - Architecture docs, implementation guides, HEXACO insights
  - References syndicate repositories as authoritative source for forte/talent definitions

- **mobsta-specs**: Ruby gem with AI agent specifications
  - Markdown-based mobsta and talent definitions
  - Flexible configuration, integrates with in-concert

---

## 🏗️ **Critical Architecture Patterns**

### **Framework Widget Hierarchy**
```
Layout Widgets (Containers):
- Wall: Grid layout for browsing (uses Brick widgets)
- Rack: List layout for sequences (uses Beam widgets)  
- Board: Form-like editing interfaces
- Bench: Complex containers with resource + associations

Item Widgets:
- Brick: Individual cards for Wall grids
- Beam: List items for Rack sequences

Frame Widgets:
- ListingFrame: Wraps Wall/Rack layouts
- ResourceFrame: Wraps resource-focused layouts
- AssociationFrame: For related resources
```

### **State Management Pattern**
```dart
// ✅ CORRECT: Use Shipment pattern
final shipment = Shipment.named('games');
await shipment.save(resource);

// ❌ WRONG: Direct repository access
final repo = shipment.repository as GameRepository;
await repo.customMethod();
```

### **Composition Hierarchy**
```
Talents → Fortes → Mobstas
   ↓        ↓        ↓
 Atomic  Composed  Game
 Units   Expertise Instances
```

### **Syndicates and Games Architecture**
```
Syndicate (Git repo of fortes)
    ├── Forte 1 (Domain Expert 1) ← Composed from Talents
    ├── Forte 2 (Domain Expert 2) ← Composed from Talents  
    ├── Forte 3 (Domain Expert 3) ← Composed from Talents
    └── Forte N (Domain Expert N) ← Composed from Talents
            ↓ (When Game created)
Game (Project/Initiative)
    ├── Mobsta 1 (Domain Expert 1 for this project) ← Instantiated from Forte 1
    ├── Mobsta 2 (Domain Expert 2 for this project) ← Instantiated from Forte 2
    ├── Mobsta 3 (Domain Expert 3 for this project) ← Instantiated from Forte 3
    └── Mobsta N (Domain Expert N for this project) ← Instantiated from Forte N
            ↓ (During collaboration)
Caper (Collaborative Session)
    ├── Mobsta 1 participating
    ├── Mobsta 2 participating
    └── Mobsta N participating
```

**Key Concepts:**
- **Talents**: Atomic units of expertise that compose into fortes
- **Syndicate**: Collection of forte definitions (LLM prompts) stored in Git repositories
- **Forte**: LLM prompt definition created by composing talents into coherent specializations
- **Game**: Specific project or initiative that uses syndicates to define AI expert team
- **Mobsta**: AI agent instance created from a forte at game time
- **Caper**: Collaborative session where mobstas work together on specific tasks

**Domain Examples:**
- **Software Development**: [Software Development Syndicate](https://github.com/AnthroCascade/software-development-syndicate) - Architecture, Backend, Frontend, DevOps experts
- **Legal Practice**: Corporate Law, Antitrust, Tax, Regulatory experts  
- **Medical Practice**: Cardiology, Neurology, Radiology, Pharmacology experts
- **Business Strategy**: Market Research, Financial Analysis, Operations, Marketing experts
- **Regulatory Compliance**: Regulatory Affairs, Risk Assessment, Audit, Legal Compliance experts
- **Knowledge Preservation**: Senior Experts, Knowledge Engineers, Documentation Specialists, Training experts

### **Component Structure Standard**
```
components/[name]/
├── prototype.dart          # Registration
├── state/
│   ├── core.dart          # Domain model  
│   ├── core.g.dart        # JSON serialization
│   └── repository.dart    # Data access
└── widgets/
    ├── board.dart         # Form interface
    ├── bench.dart         # Detail view
    ├── page.dart          # Full page (ListingFrame(Wall))
    ├── brick.dart         # Grid item (extends Brick)
    └── beam.dart          # List item (extends Beam)
```

---

## ✅ **Current Architecture Status**

### **wispera_components: Architecturally Sound**
**Verification Date**: Current analysis confirms proper implementation
**Repository Patterns**: ✅ Correctly implemented with standard framework methods
**Component Structure**: ✅ Complete widget sets following Wall/Rack/Board/Bench patterns
**Shipment Integration**: ✅ Proper use of `shipment.save()` throughout codebase

### **Previous Documentation Errors Corrected**
**Historical Claims**: Documentation previously claimed violations that do not exist
**Reality**: GameRepository follows framework patterns, Game widgets are complete
**Lesson**: Always verify against actual codebase before architectural changes

---

## 🧠 **HEXACO Behavioral Optimization**

### **Performance Enhancement Protocol**
**Reference**: Memory ID 7653697, HEXACO_MOBSTA_BREAKTHROUGH_REFERENCE.md

**High-Performance Profile**:
- **Honesty-Humility: HIGH** - Evidence-based claims only
- **Emotionality: MANAGED** - Rational assessment without reactivity
- **Extraversion: FOCUSED** - Concise, targeted communication  
- **Agreeableness: LOW** - Critical evaluation over pleasing
- **Conscientiousness: INTEGRATED** - Planning AND verification discipline
- **Openness: CHANNELED** - Innovation within established frameworks

**Anti-Lurching Protocols**:
- PAUSE before major responses to assess behavioral state
- PRESERVE working solutions, enhance rather than replace
- VERIFY claims with evidence before assertions
- MAINTAIN behavioral consistency throughout session

---

## 🎯 **Development Principles**

### **Core Rules**
1. **wispera-mobsta = thin orchestration layer** over wispera_components
2. **Always use existing patterns** before creating custom solutions
3. **Follow Repository/Shipment patterns** for all data access
4. **Consult architecture docs** before making changes
5. **Fix violations** rather than working around them

### **Assistant Approach**
- **BE opinionated architect** - fierce conversations required
- **QUESTION design assumptions** aggressively before solutions
- **PROVE understanding** of existing patterns before changes
- **REFERENCE documentation** as architectural truth
- **HOLD ground** on promising solutions vs abandoning them

---

## 📋 **Immediate Context Checklist**

When starting new chat:
- [ ] Apply HEXACO behavioral optimization
- [ ] Attach key folders (mobsta-specs, elastic-mob, anthro root)
- [ ] Reference this briefing document
- [ ] Consult ARCHITECTURE_REFERENCE.md for current patterns
- [ ] Verify understanding against actual codebase
- [ ] Ask user for specific context and priorities

---

## 🚀 **Quick Status Reference**

**Ready for Integration**:
- ✅ Backend: in-concert ElasticMob engine complete
- ✅ Specs: All mobsta/skill block definitions complete
- ✅ Framework: Mature patterns and components available

**Needs Work**:
- 🚧 Frontend integration: wispera-mobsta ↔ in-concert APIs
- ⚡ HEXACO optimization: Implement behavioral switching
- 🎯 UI/UX refinement: Enhance mobsta IDE experience

**Current Priority**: Building mobsta app integration with in-concert backend APIs using established component patterns.

---

## 🏗️ **Holonic Architecture Guidelines**

### **Balancing Holonic Design with Practical Inheritance**

**Core Principle**: Holonic design works at the domain level, but breaks down at infrastructure levels where cross-cutting concerns and inheritance hierarchies require "boxes of parts."

#### **Holonic Wholes (Keep Cohesive)**
- **Domain Cohesion**: Widgets serving the same user purpose (`layouts/`, `displays/`, `controls/`)
- **Inheritance Hierarchy**: Classes sharing common ancestry (`Bench`, `Board`, `Rack` extending `ResourceLayout`)
- **User Mental Model**: Widgets users think of together (forms, navigation)

#### **Acceptable "Box of Parts"**
- **Cross-Cutting Concerns**: Behavior used across multiple domains (`mixins/search_debouncing.dart`)
- **Low-Level Utilities**: Implementation details (`utils/`, `extensions/`)
- **Shared Infrastructure**: Framework-level concerns (`state/`, `theme/`)

#### **Decision Heuristics**
1. **"What is the primary user concept?"** → Keep domain concepts whole
2. **"How many domains use this?"** → Single domain = integrate, Multiple = separate
3. **"Is this a complete user concept?"** → Complete = holonic, Implementation detail = fragmented

**Reference**: [Recent Holonic Observations](./HOLONIC_ARCHITECTURE_OBSERVATIONS.md) - Detailed analysis of when holonic design applies vs. when "boxes of parts" are acceptable.

---

**This document enables rapid context transfer for effective anthro super-project development assistance.**
