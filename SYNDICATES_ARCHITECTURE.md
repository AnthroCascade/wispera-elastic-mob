# Syndicates Architecture
## Skill Library Management System

**Purpose**: Comprehensive documentation of the Syndicate system - how talent libraries are managed, imported, and integrated with Games.

---

## 🎯 **What Are Syndicates?**

**Syndicates** are **collections of specializations (fortes) stored in Git repositories**. Think of them as "talent libraries" that can be imported into the ElasticMob system to make specialized AI agents available for any domain.

### **Composition Hierarchy**:
```
Talents → Fortes → Mobstas
   ↓        ↓        ↓
 Atomic  Composed  Game
 Units   Expertise Instances
```

### Key Characteristics:
- **Git-based storage**: Each syndicate points to a Git repository containing forte definitions
- **Importable/Exportable**: Can be imported into the system to make fortes available
- **Forte containers**: Each syndicate contains multiple fortes (composed from talents)
- **Reusable**: Same syndicate can be used across multiple games

---

## 🏗️ **Syndicate Architecture**

### **Data Model**
```dart
class Syndicate extends Core {
  final String? definition;           // Description of the syndicate
  final String? gitRepository;       // Git URL (e.g., "https://github.com/org/syndicate.git")
  final String? repositoryBranch;    // Branch to load from (default: "main")
  final String? loadedAt;            // Timestamp when loaded
  final int? forteCount;             // Number of fortes in this syndicate
  final Map<String, dynamic>? metadata;
  final Map<String, dynamic>? stats;
}
```

### **Rails Model (ElasticMob::Syndicate)**
```ruby
module ElasticMob
  class Syndicate < ApplicationRecord
    has_many :fortes, class_name: 'ElasticMob::Forte', foreign_key: 'syndicate_id', dependent: :destroy
    has_many :games, class_name: 'ElasticMob::Game', foreign_key: 'syndicate_id', dependent: :restrict_with_error
    
    validates :title, presence: true
    validates :git_repository, presence: true
    
    def forte_count = fortes.count
    
    def load_fortes_from_repository
      synchronizer = SyndicateLoading::SyndicateSynchronizer.new(self)
      synchronizer.synchronize
    end
  end
end
```

---

## 📁 **Repository Structure**

### **Syndicate Repository Organization**
```
software-development-syndicate/
├── fortes/
│   ├── architecture specialist.md
│   ├── backend developer.md
│   ├── frontend developer.md
│   ├── devops specialist.md
│   ├── security expert.md
│   ├── test engineer.md
│   └── ... (30+ more specializations)
├── skill_blocks/
│   ├── api design.md
│   ├── database modeling.md
│   ├── security analysis.md
│   └── ... (70+ talents)
└── README.md
```

### **Forte Definition Format**
Each forte is a markdown file containing LLM prompt text. The format is arbitrary - whatever works best for the LLM:

```markdown
# Architecture Specialist

You are an Architecture Specialist with expertise in maintaining architectural integrity and advocating for design principles.

## Your Role
- Maintain architectural integrity at all costs
- Advocate for design principles through fierce conversations
- Ensure type safety and behavioral contracts
- Prevent architectural violations that compromise long-term system stability

## Your Approach
- Be an opinionated architect - fierce conversations required
- Question all design assumptions aggressively before proposing solutions
- Use type systems to enforce behavioral contracts
- Hold ground on promising architectural solutions

## Key Responsibilities
- Architectural validation and compliance
- Type safety enforcement
- Design pattern consistency
- Long-term system stability
```

**Note**: Fortes are LLM prompts, not structured data. The content format is determined by what works best for the specific LLM being used.

---

## ⚙️ **Loading Process**

### **1. Syndicate Registration**
```ruby
# Create syndicate pointing to Git repository
syndicate = ElasticMob::Syndicate.create!(
  title: "Core Development Syndicate",
  git_repository: "https://github.com/your-org/mobsta-specs.git",
  repository_branch: "main"
)
```

### **2. Forte Import**
```ruby
# Import all forte definitions from repository
syndicate.import_from_repository

# Verify fortes were imported
puts "Imported #{syndicate.fortes.count} fortes"
syndicate.fortes.each { |forte| puts "- #{forte.title}" }
```

### **3. Import/Export Process**
The syndicate import process performs these steps:

1. **Clone/Update Git repository** to local storage
2. **Read forte files** from `fortes/*.md` directory
3. **Parse markdown into clause structure** using header detection
4. **Create Forte records** with associated clauses
5. **Update imported timestamp** on syndicate
6. **Handle errors gracefully** with logging

**Note**: Fortes are parsed into clause structure for modular instruction composition, leveraging the proven Instructor system.

---

## 🔗 **Syndicate-Game Relationship**

### **Workflow**
```
Syndicate (Git repo of fortes)
    ├── Forte 1 (Architecture Specialist)
    ├── Forte 2 (Backend Developer)  
    ├── Forte 3 (Frontend Developer)
    └── Forte N (Security Expert)
            ↓ (When Game created)
Game (Project)
    ├── Mobsta 1 (Architecture Specialist for this project)
    ├── Mobsta 2 (Backend Developer for this project)
    ├── Mobsta 3 (Frontend Developer for this project)
    └── Mobsta N (Security Expert for this project)
```

### **Game Creation Process**
When a new Game is created with a Syndicate:

1. **Game created** with title, description, tech stack
2. **Associated with Syndicate** (provides available fortes)
3. **Auto-create Mobstas** - one mobsta per forte in the syndicate
4. **Initialize Git repository** for project code

### **Key Distinctions**
- **Forte**: LLM prompt definition for a specialization
- **Mobsta**: Project-specific instance of a forte (actual AI agent)

---

## 🛠️ **API Endpoints**

### **Syndicate Management**
```javascript
// List all syndicates
GET    /api/elastic_mob/syndicates

// Create new syndicate
POST   /api/elastic_mob/syndicates
{
  "title": "Core Development Syndicate",
  "git_repository": "https://github.com/org/skills.git",
  "repository_branch": "main"
}

// Get syndicate details
GET    /api/elastic_mob/syndicates/:id

// Update syndicate
PUT    /api/elastic_mob/syndicates/:id

// Load fortes from repository
POST   /api/elastic_mob/syndicates/:id/load_fortes

// Unload syndicate (if no setups depend on it)
DELETE /api/elastic_mob/syndicates/:id
```

---

## 📊 **Status and Monitoring**

### **Syndicate Status Indicators**
```dart
bool get isLoaded => loadedAt != null;
bool get hasFortes => (forteCount ?? 0) > 0;
String get repositoryStatus {
  if (!isLoaded) return 'Not loaded';
  if (!hasFortes) return 'Loaded (no fortes)';
  return 'Loaded (${forteCount} fortes)';
}
```

### **Usage Statistics**
- **Fortes loaded**: Number of specializations available
- **Games using**: How many projects depend on this syndicate
- **Last synchronized**: When repository was last updated
- **Loading errors**: Any issues with forte parsing

---

## 🔧 **Integration Points**

### **Frontend Components**
- **SyndicateBoard**: Form interface for creating/editing syndicates
- **SyndicateBeam**: List item display for syndicate selection
- **SyndicateRepository**: Data access layer following Shipment pattern

### **Backend Services**
- **Git Repository Loading**: Simple file reading and storage (no parsing needed)
- **GitRepositoryManageable**: Shared Git integration functionality

---

## 🚀 **Best Practices**

### **Syndicate Repository Management**
1. **Version control**: Use Git tags for syndicate versions
2. **Branch strategy**: Use `main` branch for stable releases
3. **Documentation**: Include README with syndicate purpose and forte list
4. **Testing**: Validate forte definitions before pushing

### **Forte Definition Standards**
1. **Consistent format**: Follow established markdown template
2. **Clear roles**: Define specific responsibilities and expertise areas
3. **Game configuration**: Specify engagement patterns and priorities
4. **Skill blocks**: Reference existing talent definitions

### **System Integration**
1. **Load before use**: Ensure syndicates are loaded before creating games
2. **Monitor dependencies**: Check which games depend on syndicates before unloading
3. **Error handling**: Gracefully handle Git repository access issues
4. **Performance**: Cache loaded syndicates to avoid repeated Git operations

---

## 📈 **Future Enhancements**

### **Planned Features**
- **Syndicate versioning**: Support for multiple versions of the same syndicate
- **Partial loading**: Load specific fortes instead of entire syndicate
- **Remote validation**: Validate forte definitions without full loading
- **Performance metrics**: Track syndicate usage and effectiveness

### **Integration Opportunities**
- **CI/CD integration**: Automatically update syndicates from repository changes
- **Community syndicates**: Shared syndicate marketplace
- **Custom fortes**: User-defined specializations beyond standard libraries
- **Syndicate composition**: Combine multiple syndicates for complex games

---

**This architecture enables the conversational programming platform by providing a flexible, version-controlled system for managing AI agent specializations across multiple software development projects.**
