# Elastic-Mob Implementation Approach

## **Agreed Architecture**

### **Core Design Principle**
Elastic-mob will be implemented as an **integrated engine within in-concert** rather than a separate microservice. This approach prioritizes simplicity and direct access to existing infrastructure while maintaining clear separation of concerns.

**Note**: This document contains the complete implementation strategy. For current status and readiness assessment, see [current_audit.md](current_audit.md). For technology analysis and recommendations, see [architecture_critique.md](architecture_critique.md).

**📋 Implementation Resources**: The detailed talent and mobsta specifications that will be implemented are maintained in the **in-concert repository** under `app/services/elastic_mob/definitions/`. See [talents overview.md](../skill%20blocks%20overview.md) and [mobstas overview.md](../mobstas%20overview.md) for comprehensive overviews.

**📋 Templates**: Use [talent template](../templates/skill%20block.md) and [mobsta template](../templates/mobsta.md) for creating new specifications.

### **Architecture Overview**
```
Elastic-Mob Frontend (Plain JS + Web Components) → Elastic-Mob Engine (within in-concert) → Existing in-concert models/services
```

**Frontend Architecture:**
- **Plain JavaScript** with Web Components (no JS frameworks)
- **Holonic design** - every component is both system and object
- **Layered composition** - layouts → partials → web components
- **Real-time updates** via WebSocket integration

**Key Benefits:**
- **Single codebase** reduces deployment complexity
- **Direct access** to existing LLM services, authentication, and data models
- **No API overhead** between elastic-mob and in-concert internals
- **Shared infrastructure** (database, Redis, authentication, etc.)
- **Easier iteration** without service boundary complications
- **Frontend simplicity** - no framework overhead, direct DOM manipulation
- **Real-time responsiveness** - WebSocket events update DOM immediately

## **Co-Existence Model**

### **User Base Separation**
- **In-concert** serves both wispera-flutter users AND elastic-mob users
- **Wispera-flutter users** manage prompt lifecycles (separate concern)
- **Elastic-mob users** have their own mob-space for autonomous development
- **No conflicts** between the two user bases

### **Isolation Strategy**
- Each user gets their own mob-space
- Mob sessions are isolated by user
- No cross-contamination between different user contexts

## **Implementation Phases**

### **Phase 1: Engine Integration (2-3 weeks)**
```ruby
# Add to in-concert as a specialized engine
module ElasticMob
  class Engine < ApplicationRecord
    belongs_to :user
    has_many :capers
    has_many :agent_combinations
    
    def start_caper(context, skill_blocks)
      # Create caper
      # Initialize agents with talents
      # Begin autonomous conversation
    end
  end
end
```

**Deliverables:**
- Core engine models integrated into in-concert
- Basic caper management
- User isolation and mob-space creation
- Frontend component architecture design

### **Phase 2: WebSocket Layer (1-2 weeks)**
```ruby
# Use existing ActionCable infrastructure
class MobChannel < ApplicationCable::Channel
  def subscribed
    stream_from "caper_#{params[:session_id]}"
  end
  
  def receive(data)
    # Handle user interventions
    # Route to appropriate caper
  end
end
```

**Deliverables:**
- Real-time communication via ActionCable
- User intervention handling
- Session state synchronization
- Core web components (Caper, AgentDisplay, ConversationView)
- WebSocket integration with frontend components

### **Phase 3: Autonomous Agent Process (2-3 weeks)**
```ruby
class Caper
  def run_autonomous_conversation
    loop do
      # Agent coordination logic
      # Skill block activation
      # LLM calls via existing services
      # Git operations
      
      break if consensus_reached? || user_intervention_needed?
    end
  end
  
  def request_user_input(question, context)
    # Signal frontend that user input is needed
    # Wait for response
    # Continue conversation
  end
end
```

**Deliverables:**
- Autonomous agent coordination
- Skill block activation logic
- User input request system
- Basic git integration
- Advanced web components (CodePreview, InterventionControls)
- Real-time agent coordination display

## **Technical Implementation Details**

### **User Isolation**
```ruby
# Each user gets their own mob-space
class User < ApplicationRecord
  has_one :elastic_mob_engine
  has_many :capers, through: :elastic_mob_engine
end
```

### **Git Integration**
```ruby
# Direct access to existing file/document models
class Caper
  def commit_changes(conversation_context, generated_code)
    # Use existing file management
    # Create git commits with conversation metadata
    # Track which agents made which changes
  end
end
```

### **LLM Service Integration**
```ruby
# Direct access to existing LLM infrastructure
class Caper
  def request_llm_response(context, skill_blocks)
    # Use existing Assistant/LLM services
    # No API calls needed
    # Leverage existing context management
  end
end
```

## **Frontend Architecture: Plain JS + Web Components**

### **Holonic Design Principles**
- **Every component is both system and object** - can contain other components and be contained
- **Self-contained systems** - each component manages its own state, behavior, and presentation
- **Natural composition** - components compose into larger systems without central orchestration
- **Emergent behavior** - system behavior emerges from component interactions

### **Component Architecture**
```javascript
// Atomic components (smallest units)
customElements.define('agent-status', AgentStatus);
customElements.define('talent', Talent);
customElements.define('conversation-turn', ConversationTurn);

// Composite components (compose atomic components)
customElements.define('agent-display', AgentDisplay);
customElements.define('mob-conversation', MobConversation);
customElements.define('code-section', CodeSection);

// Layout components (high-level composition)
customElements.define('mob-session', Caper);
customElements.define('intervention-panel', InterventionPanel);
```

### **Real-Time Updates**
```javascript
// WebSocket integration for immediate responsiveness
class Caper extends HTMLElement {
  constructor() {
    super();
    this.websocket = new WebSocket('ws://localhost:3000/cable');
    this.websocket.onmessage = (event) => this.updateDisplay(JSON.parse(event.data));
  }
  
  updateDisplay(data) {
    // Direct DOM manipulation - no state management complexity
    this.shadowRoot.querySelector('.status').textContent = data.status;
    this.shadowRoot.querySelector('.agents').innerHTML = this.renderAgents(data.agents);
  }
}
```

### **Benefits Over JS Frameworks**
- **No virtual DOM overhead** - direct DOM manipulation for real-time updates
- **Zero build step** - edit and refresh for rapid iteration
- **Native browser support** - no polyfills or build tooling needed
- **Component isolation** - each feature is self-contained and testable
- **Real-time responsiveness** - WebSocket events update DOM immediately

## **User Experience Model**

### **Single User Focus (Initial)**
- **Long-running agent process** that writes/adjusts code to a git repo
- **User as one of the mobstas** providing answers when needed
- **Rare user interventions** for course correction
- **Autonomous operation** with minimal human oversight

### **User Participation Modes**
1. **Observer Mode**: Watch agents work autonomously (default)
2. **Participant Mode**: Join the mob with user proxy mobsta active
3. **Authority Mode**: Take control in specific domains when needed
4. **Intervention Mode**: Emergency halt and redirection capabilities

## **Iterative Enhancement Path**

### **Foundation (Phases 1-3)**
- Basic engine with simple agent coordination
- WebSocket interface for real-time updates
- Git integration for code versioning
- User intervention capabilities

### **Future Enhancements**
- Advanced agent behaviors and talent evolution
- Multi-user collaboration features
- Enhanced git workflow and conflict resolution
- Performance optimization and scaling
- Event bus for advanced system integration

## **Why This Approach Works**

1. **Simpler deployment** - one application to manage
2. **Direct model access** - no serialization/deserialization overhead
3. **Shared authentication** - users already exist in the system
4. **Easier iteration** - can enhance incrementally without service boundaries
5. **Proven infrastructure** - leverages existing in-concert patterns
6. **Reduced complexity** - no microservice overhead for initial implementation

## **Success Criteria**

- **Elastic-mob engine** successfully integrated into in-concert
- **Autonomous agent conversations** generate working code
- **User interventions** work smoothly when needed
- **Git integration** tracks conversation-driven code evolution
- **Real-time updates** via WebSocket provide clear visibility
- **User isolation** prevents cross-contamination between users

---

**Note**: This approach prioritizes **proving that elastic-mobbing can work** over architectural complexity. We can evolve to more sophisticated patterns (gateways, event buses, microservices) once the core functionality is validated and stable.

## **Related Documents**

### **Current Status & Foundation**
- **[current_audit.md](current_audit.md)** - Complete assessment of current system status and readiness
- **[deeper architecture.md](deeper%20architecture.md)** - Conceptual foundations and game theory

### **Persona System**
- **[elastic_mob_persona.md](elastic_mob_persona.md)** - Meta-instruction system for AI assistants
- **[persona_optimizer.md](persona_optimizer.md)** - Continuous improvement framework
- **[meta_mobsta_architecture.md](meta_mobsta_architecture.md)** - Self-improvement design principles

### **Technology & Integration**
- **[architecture_critique.md](architecture_critique.md)** - Technology assessment and recommendations
- **[copilot_integration_spec.md](copilot_integration_spec.md)** - Integration requirements and patterns
