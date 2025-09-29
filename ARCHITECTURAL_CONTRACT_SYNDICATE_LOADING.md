# Architectural Contract: Syndicate Import/Export System

**Date**: Current Session  
**Purpose**: Document agreed-upon architectural patterns and violations for syndicate import/export system

---

## **Agreed Architectural Violations** ❌

### **1. `load_fortes` Controller Action is Anti-Pattern (Import Pattern)**
- **Violation**: Separate controller action for loading nested resources
- **Problem**: Breaks atomic resource loading principle
- **Impact**: Creates inconsistent states and violates transactional integrity

### **2. Process-Oriented Method Naming**
- **Violation**: `create_mobstas_from_syndicate_fortes` (process name)
- **Problem**: Not framework canon - should be outcome-oriented
- **Impact**: Inconsistent with established `ensure_*` patterns

### **3. Service Object Pattern**
- **Violation**: `SyndicateLoading::SyndicateSynchronizer` (pseudo object)
- **Problem**: Not framework canon - concerns handle these patterns
- **Impact**: Inconsistent with concern-based architecture

---

## **Framework Canon Patterns** ✅

### **1. Atomic Resource Loading**
```ruby
# ✅ CORRECT: Atomic syndicate loading
def ensure_syndicate_loaded
  return if syndicate.loaded_at.present?
  synchronize_fortes_from_repository
end
```

### **2. Outcome-Oriented Method Naming**
```ruby
# ✅ CORRECT: Outcome-oriented names
after_create :ensure_mobstas
after_save :ensure_similarities

def ensure_mobstas  # ✅ Outcome name
def ensure_similarities  # ✅ Outcome name
```

### **3. Concern-Based Architecture**
```ruby
# ✅ CORRECT: Concerns, not service objects
module Synchronizing
  extend ActiveSupport::Concern
  
  def ensure_synchronized
    return if synchronized?
    synchronize_from_source
  end
end
```

---

## **Established `ensure-*` Pattern Analysis**

### **Existing Framework Patterns:**
```ruby
# Participating concern
def ensure_ownership
  participations.find_or_create_by!(
    participant: current_participant,
    power: :creator
  )
end

# Citations concern  
def ensure_cited_files
  if cited_paths.any?
    ensure_attachments
    ensure_participations
  end
  reload
end

# Similarity concern
def ensure_similarities
  if usable_for_similarity? && requires_similarities_explicity_built?
    nearest_chunks.each do |c|
      similar_chunks.find_or_create_by!(content_chunk: c)
    end
  end
end
```

### **Key Principles:**
1. **Atomic Operations**: Resource + dependencies loaded together
2. **Idempotent**: Multiple calls don't break state
3. **Outcome Names**: `ensure_*` not `create_*` or `load_*`
4. **Concern-Based**: Direct implementation in concerns

---

## **Correct Syndicate Loading Architecture**

### **1. Syndicate Model Should Include Concerns:**
```ruby
module ElasticMob
  class Syndicate < ApplicationRecord
    include Synchronizing
    include Synchronized  
    include GitRepositoryManageable
    
    def ensure_synchronized
      return if synchronized?
      synchronize_fortes_from_repository
    end
    
    protected
    
    def synchronize_fortes_from_repository
      return unless git_repository.present?
      
      # Direct implementation, no service objects
      git_service = VersionControl::Git::Service.new(resource: self)
      git_service.ready
      
      forte_files = find_forte_definition_files(git_service.path)
      forte_files.each { |file| ensure_forte_from_file(file) }
      
      update!(loaded_at: Time.current)
    end
    
    def ensure_forte_from_file(file_path)
      return unless File.exist?(file_path)
      
      title = File.basename(file_path, '.md').titleize
      content = File.read(file_path)
      
      fortes.find_or_create_by!(title: title) do |forte|
        forte.definition = content  # Store complete markdown text
        forte.definition_file = File.basename(file_path)
      end
    end
  end
end
```

### **2. Game Model Should Use ensure_* Pattern:**
```ruby
module ElasticMob
  class Game < ApplicationRecord
    include GameLifecycle
    
    after_create :ensure_mobstas  # ✅ Canon pattern
    
    protected
    
    def ensure_mobstas
      return unless syndicate&.fortes&.any?
      
      syndicate.fortes.each do |forte|
        mobstas.find_or_create_by!(forte: forte) do |mobsta|
          mobsta.title = "#{forte.title} for #{title}"
          mobsta.is_engaged = false
        end
      end
    end
  end
end
```

### **3. Controller Pattern Should Be:**
```ruby
# SyndicatesController
def show
  ensure_syndicate_loaded  # Atomic loading
  super
end

def update
  super.tap do
    ensure_syndicate_loaded if git_repository_changed?
  end
end
```

---

## **Forte Deprecation Pattern**

### **Backward Compatibility Requirements:**
```ruby
def synchronize_fortes_from_repository
  # Load new/updated fortes
  forte_files = find_forte_definition_files(git_service.path)
  forte_files.each { |file| ensure_forte_from_file(file) }
  
  # Mark missing fortes as deprecated (not deleted)
  existing_forte_titles = fortes.pluck(:title)
  loaded_forte_titles = forte_files.map { |f| extract_title(f) }
  
  deprecated_fortes = existing_forte_titles - loaded_forte_titles
  fortes.where(title: deprecated_fortes).update_all(deprecated: true)
  
  update!(loaded_at: Time.current)
end
```

### **Game Dependencies Protected:**
```ruby
def can_be_unloaded?
  games.empty?  # ✅ Already implemented correctly
end

def can_deprecate_forte?(forte)
  forte.mobstas.joins(:game).where(games: { status: 'active' }).empty?
end
```

---

## **Benefits of Correct Architecture**

### **1. Atomic Operations**
- Syndicate loading is always complete
- No partial states in the system
- Consistent with existing patterns

### **2. Idempotent Operations**
- Multiple calls to load don't break anything
- Updates work seamlessly
- Safe for concurrent access

### **3. Backward Compatibility**
- Existing games continue to work
- Deprecated fortes remain available
- Graceful degradation

### **4. Controller Simplicity**
- Controllers focus on HTTP concerns
- Business logic in models
- Follows Rails conventions

---

## **Forte Implementation Clarification**

### **Forte Purpose: LLM Prompts**
- **Fortes are LLM prompts**, not structured data
- **Complete markdown text** should be stored in `definition` field
- **No metadata parsing** is necessary or beneficial
- **Simple text storage** is the correct approach

### **Correct Forte Implementation:**
```ruby
module ElasticMob
  class Forte < ApplicationRecord
    validates :title, presence: true
    validates :definition, presence: true  # Raw markdown for LLM
    
    # That's it - no parsing methods needed
  end
end
```

### **Incorrect Over-Engineering:**
- ❌ `ForteParser` extracting metadata
- ❌ `parsed_attributes` methods
- ❌ `role_name`, `core_mission` accessors
- ❌ Controller endpoints for parsing

## **Implementation Contract**

### **What Must Be Done:**
1. **Remove** `load_fortes` controller action (anti-pattern)
2. **Refactor** `create_mobstas_from_syndicate_fortes` → `ensure_mobstas`
3. **Replace** `SyndicateSynchronizer` service object with concern-based approach
4. **Implement** `Synchronizing`/`Synchronized` concerns
5. **Add** forte deprecation pattern for backward compatibility
6. **Simplify** forte model to store only raw markdown text

### **What Must NOT Be Done:**
1. ❌ Create separate controller actions for nested resource loading
2. ❌ Use process-oriented method names (`create_*`, `load_*`)
3. ❌ Create service objects for concern-based operations
4. ❌ Break atomic resource loading principles
5. ❌ Violate established `ensure_*` patterns
6. ❌ Implement metadata parsing for LLM prompts
7. ❌ Add unnecessary accessor methods for parsed content

---

## **Architectural Principles Summary**

1. **Atomic Resource Loading**: Resources and dependencies loaded together
2. **Outcome-Oriented Naming**: `ensure_*` methods, not process names
3. **Concern-Based Architecture**: Direct implementation in concerns, not service objects
4. **Idempotent Operations**: Safe to call multiple times
5. **Backward Compatibility**: Deprecate, don't delete dependent resources

---

**This contract ensures architectural consistency with the mature codebase patterns and prevents regression to anti-patterns.**
