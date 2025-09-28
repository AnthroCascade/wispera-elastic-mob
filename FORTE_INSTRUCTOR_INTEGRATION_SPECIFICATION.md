# Forte-Instructor Integration Specification

## **Overview**

This specification details the architectural transformation of Fortes from simple text storage to sophisticated Instructor-based clause composition, leveraging the mature Instructor system's proven infrastructure.

## **Core Architectural Transformation**

### **Current State (Incorrect)**
```ruby
# Simple text storage model
class ElasticMob::Forte < ApplicationRecord
  validates :definition, presence: true  # Raw markdown text
end
```

### **Target State (Instructor-Based)**
```ruby
# Sophisticated instruction system
class Forte < Instructor
  # Inherits: clauses, substitutions, packs, previews, LLM integration
end
```

---

## **1. DATABASE ARCHITECTURE**

### **A. Importable Superclass with STI + Game/Caper Terminology**
```ruby
# Migration: Create importable table with STI
class CreateImportables < ActiveRecord::Migration[7.0]
  def change
    create_table :importables do |t|
      t.string :title, null: false
      t.text :description
      t.string :git_repository
      t.string :repository_branch, default: 'main'
      t.datetime :exported_at  # Renamed from loaded_at
      t.datetime :imported_at  # Renamed from loaded_at
      t.string :type, null: false  # STI column
      
      t.timestamps
    end
    
    add_index :importables, :type
    add_index :importables, :git_repository
  end
end

# Migration: Create games table (renamed from setups)
class CreateGames < ActiveRecord::Migration[7.0]
  def change
    create_table :games do |t|
      t.string :title, null: false
      t.text :description
      t.references :syndicate, foreign_key: { to_table: :importables }
      t.timestamps
    end
  end
end

# Migration: Create capers table (renamed from sessions)
class CreateCapers < ActiveRecord::Migration[7.0]
  def change
    create_table :capers do |t|
      t.string :title
      t.text :description
      t.references :game, foreign_key: true
      t.datetime :started_at
      t.datetime :completed_at
      t.timestamps
    end
  end
end

# Migration: Update existing packs to use importables
class MigratePacksToImportables < ActiveRecord::Migration[7.0]
  def up
    # Move existing packs to importables table
    execute <<-SQL
      INSERT INTO importables (id, created_at, updated_at, title, description, type)
      SELECT id, created_at, updated_at, title, description, 'Pack'
      FROM packs;
    SQL
    
    # Update instructors to reference importables instead of packs
    execute <<-SQL
      UPDATE instructors 
      SET importable_id = pack_id, importable_type = 'Pack'
      WHERE pack_id IS NOT NULL;
    SQL
    
    # Drop old packs table
    drop_table :packs
  end
  
  def down
    # Reverse migration logic
  end
end

# Migration: Update instructors table
class UpdateInstructorsForImportables < ActiveRecord::Migration[7.0]
  def change
    add_reference :instructors, :importable, polymorphic: true, null: true
    remove_reference :instructors, :pack, foreign_key: true
  end
end
```

### **B. Importable Superclass**
```ruby
class Importable < ApplicationRecord
  include Synchronizing  # Git repository import/export
  include Participated   # User participation
  include Previewable    # Preview generation
  
  has_many :instructors, as: :importable, dependent: :destroy
  
  validates :title, presence: true
  validates :git_repository, presence: true
  
  # Shared import/export functionality
  def export_to_repository
    # Export pack contents to Git repository
  end
  
  def import_from_repository
    # Import pack contents from Git repository  
  end
end
```

### **C. Pack and Syndicate Inheritance**
```ruby
class Pack < Importable
  # Existing pack functionality
  # Gets: import/export, synchronization, participation
  
  def self.derivations
    [
      super,
      :instructors,
      :instructor_count,
    ].flatten.uniq
  end
  
  def instructor_count = instructors.count
end

class Syndicate < Importable
  # Syndicate-specific functionality
  # Gets: import/export, synchronization, participation
  
  has_many :fortes, -> { where(type: 'Forte') }, class_name: 'Instructor', as: :importable
  has_many :games, dependent: :destroy
  
  def forte_count = fortes.count
  
  def self.derivations
    [
      super,
      :fortes,
      :forte_count,
      :games,
    ].flatten.uniq
  end
end
```

---

## **2. FORTE MODEL TRANSFORMATION**

### **A. Forte as Instructor Subclass**
```ruby
class Forte < Instructor
  # Inherits: clauses, substitutions, importables, previews, LLM integration
  
  belongs_to :syndicate, class_name: 'Syndicate', foreign_key: :importable_id, 
             foreign_type: :importable_type
  
  validates :title, presence: true
  
  # Syndicate association (via importable)
  def syndicate
    importable if importable.is_a?(Syndicate)
  end
  
  def syndicate_id
    importable_id if importable.is_a?(Syndicate)
  end
end
```

### **B. Game and Caper Models**
```ruby
class Game < ApplicationRecord
  belongs_to :syndicate
  has_many :mobstas, dependent: :destroy
  has_many :capers, dependent: :destroy
  
  validates :title, presence: true
  
  def mobsta_count = mobstas.count
  def caper_count = capers.count
end

class Caper < ApplicationRecord
  belongs_to :game
  has_many :mobsta_participations, dependent: :destroy
  has_many :mobstas, through: :mobsta_participations
  
  validates :title, presence: true
  
  def duration
    return nil unless started_at && completed_at
    completed_at - started_at
  end
end
```

### **C. Preserve ElasticMob::Forte Behavior**
**CRITICAL**: Before removing `ElasticMob::Forte`, audit for any unique behavior:

```ruby
# Check what ElasticMob::Forte currently does:
module ElasticMob
  class Forte < ApplicationRecord
    belongs_to :syndicate
    validates :definition, presence: true
    validates :title, presence: true
    
    # AUDIT: Any custom methods?
    # AUDIT: Any validations?
    # AUDIT: Any associations?
    # AUDIT: Any business logic?
  end
end
```

---

## **3. SYNCHRONIZING CONCERN (Import/Export)**

```ruby
module Synchronizing
  extend ActiveSupport::Concern
  
  included do
    # Importable-specific import/export functionality
  end
  
  def import_from_repository
    return false unless git_repository.present?
    
    git_service = VersionControl::Git::Service.new(resource: self)
    git_service.ready
    
    ensure_instructors_from_repository(git_service.path)
    update!(imported_at: Time.current)
    
    true
  rescue => e
    Rails.logger.error "Failed to import #{self.class.name} #{title}: #{e.message}"
    false
  end
  
  def export_to_repository
    return false unless git_repository.present?
    
    git_service = VersionControl::Git::Service.new(resource: self)
    git_service.ready
    
    export_instructors_to_repository(git_service.path)
    update!(exported_at: Time.current)
    
    true
  rescue => e
    Rails.logger.error "Failed to export #{self.class.name} #{title}: #{e.message}"
    false
  end
  
  private
  
  def ensure_instructors_from_repository(repo_path)
    if self.is_a?(Syndicate)
      ensure_fortes_from_repository(repo_path)
    else
      ensure_pack_instructors_from_repository(repo_path)
    end
  end
  
  def ensure_fortes_from_repository(repo_path)
    forte_files = find_forte_definition_files(repo_path)
    forte_files.each { |file| ensure_forte_from_file(file) }
  end
  
  def ensure_forte_from_file(file_path)
    return unless File.exist?(file_path)
    
    title = File.basename(file_path, '.md').titleize
    content = File.read(file_path)
    
    forte = instructors.where(type: 'Forte').find_or_initialize_by(title: title)
    forte.clauses.destroy_all
    
    clause_data = parse_markdown_to_clauses(content)
    clause_data.each do |clause_attrs|
      forte.clauses.create!(clause_attrs)
    end
    
    forte.save!
  end
  
  def find_forte_definition_files(repo_path)
    fortes_dir = File.join(repo_path, 'fortes')
    return [] unless Dir.exist?(fortes_dir)
    
    Dir.glob(File.join(fortes_dir, '*.md')).reject do |file|
      File.basename(file).downcase == 'list.md'
    end
  end
  
  def parse_markdown_to_clauses(content)
    sections = content.scan(/##?\s*(.+?)\s*\n(.+?)(?=\n##|\z)/m)
    
    if sections.any?
      sections.map.with_index do |(header, content), index|
        {
          clause_type: header.strip.titleize,
          content: content.strip,
          position: index
        }
      end
    else
      [{
        clause_type: 'Definition',
        content: content.strip,
        position: 0
      }]
    end
  end
end
```

---

## **4. CONTROLLER ARCHITECTURE UPDATES**

### **A. FortesController Inheritance**
```ruby
class ElasticMob::FortesController < InstructorsController
  # Inherits all Instructor controller functionality
  # Gets: clauses, substitutions, previews automatically
end
```

### **B. Remove Anti-Pattern Routes**
**File**: `in-concert/config/routes.rb`
**Action**: Remove `load_fortes` route (anti-pattern)

---

## **5. FLUTTER COMPONENT UPDATES**

### **A. Forte Model Updates**
**File**: `wispera_components/lib/src/components/fortes/state/core.dart`

```dart
class Forte extends Instructor {
  // Inherits all Instructor capabilities
  // Gets: clauses, substitutions, importables automatically
  
  Forte({
    super.explanation,
    super.importable,  // Changed from pack
    super.type,
    super.transformable,
    super.title,
    super.timeAgo,
    super.power,
    super.deletable,
    super.savable,
    super.sharable,
    super.id,
  });
  
  // Syndicate association
  Syndicate? get syndicate => importable is Syndicate ? importable as Syndicate : null;
}
```

### **B. Repository Updates**
**File**: `wispera_components/lib/src/components/fortes/state/repository.dart`

```dart
class ForteRepository extends InstructorRepository {
  // Inherits all Instructor repository functionality
  // Gets: clauses, substitutions, previews automatically
}
```

### **C. Importable Model Updates**
**File**: `wispera_components/lib/src/components/importables/state/core.dart`

```dart
class Importable extends Core {
  final String? description;
  final String? gitRepository;
  final String? repositoryBranch;
  final String? exportedAt;
  final String? importedAt;
  
  Importable({
    this.description,
    this.gitRepository,
    this.repositoryBranch,
    this.exportedAt,
    this.importedAt,
    super.title,
    super.timeAgo,
    super.power,
    super.deletable,
    super.savable,
    super.sharable,
    super.id,
  });
}
```

---

## **6. IMPLEMENTATION PHASES**

### **Phase 1: Database Foundation (Early - Test Wispera App)**
1. Create `importables` table with STI
2. Migrate existing `packs` to `importables` with type 'Pack'
3. Update `instructors` table for polymorphic `importable` association
4. **TEST**: Verify wispera app still works with Pack changes
5. Create `Importable` superclass
6. Update `Pack` to inherit from `Importable`

### **Phase 2: Syndicate Integration**
1. Create `Syndicate < Importable` model
2. Update syndicate associations to use `importable` pattern
3. Test syndicate creation and association
4. **COMMIT**: Working server-side syndicate functionality

### **Phase 3: Forte Integration (Iterative with Flutter)**
1. Create `Forte < Instructor` model
2. **AUDIT**: Preserve any `ElasticMob::Forte` behavior
3. Update Flutter `Forte` model to inherit from `Instructor`
4. Test forte creation and clause parsing
5. **COMMIT**: Working server + Flutter forte functionality

### **Phase 4: Import/Export Synchronization**
1. Remove `SyndicateSynchronizer` service object
2. Implement `Synchronizing` concern with import/export terminology
3. Update Flutter components for import/export functionality
4. Test syndicate import from repository with clause parsing
5. **COMMIT**: Working import/export functionality

### **Phase 5: Cleanup (Specific Classes Only)**
1. Remove old `ElasticMob::Forte` model (after behavior audit)
2. Remove old `ElasticMob::Syndicate` model
3. **PRESERVE**: Remaining `ElasticMob::*` classes for mobsta app
4. Update documentation
5. **COMMIT**: Final cleanup

---

## **7. CRITICAL VALIDATION STEPS**

### **A. Early Validation (Phase 1)**
- [ ] **Wispera app still works** after Pack → Importable migration
- [ ] **Existing Pack functionality** preserved
- [ ] **Database migrations** run successfully
- [ ] **STI inheritance** works correctly

### **B. Behavior Preservation Checklist**
- [ ] **Audit `ElasticMob::Forte`** for all methods, validations, associations
- [ ] **Preserve syndicate association** via importable relationship
- [ ] **Maintain title validation** and any other business rules
- [ ] **Test all existing functionality** before removing old models

### **C. Import/Export Validation**
- [ ] **Import functionality** works for both Pack and Syndicate
- [ ] **Export functionality** works for both Pack and Syndicate
- [ ] **Git integration** is consistent across both types
- [ ] **Clause parsing** creates appropriate structure

### **D. Flutter Integration Validation**
- [ ] **Inheritance works** correctly in Flutter components
- [ ] **Importable associations** work in Flutter
- [ ] **Clause/substitution system** works in Flutter
- [ ] **UI components** reuse Instructor patterns correctly

---

## **8. TERMINOLOGY CHANGES**

### **Load/Unload → Import/Export**
- `load_fortes_from_repository` → `import_from_repository`
- `loaded_at` → `imported_at`
- `unload!` → `export_to_repository`
- All documentation updated to use import/export terminology

### **Pack Association Updates**
- `pack_id` → `importable_id`
- `pack_type` → `importable_type`
- `belongs_to :pack` → `belongs_to :importable, polymorphic: true`

---

## **9. FILES TO UPDATE**

### **Server-Side Files**
- `app/models/importable.rb` (new)
- `app/models/pack.rb` (update inheritance)
- `app/models/syndicate.rb` (new, inherit from Importable)
- `app/models/forte.rb` (new, inherit from Instructor)
- `app/models/concerns/synchronizing.rb` (new)
- `app/controllers/elastic_mob/fortes_controller.rb` (update inheritance)
- `config/routes.rb` (remove anti-pattern routes)

### **Flutter Files**
- `lib/src/components/importables/` (new directory)
- `lib/src/components/fortes/state/core.dart` (update inheritance)
- `lib/src/components/packs/state/core.dart` (update for importable)
- All instructor-related Flutter components

### **Documentation Files**
- `SYNDICATES_ARCHITECTURE.md` (update for import/export)
- `ARCHITECTURAL_CONTRACT_SYNDICATE_LOADING.md` (update terminology)
- All other elastic-mob documentation

---

## **10. ARCHITECTURAL COMPLIANCE**

### **Success Metrics**
- [ ] Fortes inherit all Instructor capabilities (clauses, substitutions, importables)
- [ ] Syndicate import/export uses concern-based approach (no service objects)
- [ ] Markdown parsing creates appropriate clause structure
- [ ] UI components reuse Instructor patterns
- [ ] No anti-pattern routes or controller actions
- [ ] Wispera app continues to work throughout migration
- [ ] LLM integration works via existing Assistant composition

### **Framework Compliance**
- [ ] Follows `ensure_*` pattern for atomic operations
- [ ] Uses concern-based architecture (no service objects)
- [ ] Outcome-oriented method naming (import/export vs load/unload)
- [ ] Leverages existing framework patterns
- [ ] Maintains backward compatibility during migration
- [ ] Iterative development with working commits at each phase

This specification transforms Fortes from simple text storage into a **sophisticated, modular instruction system** that leverages the mature Instructor architecture while maintaining architectural consistency with established framework patterns.
