# Holonic Implementation Protocol
## Meta-Strategy for Idempotent Component Development

**Purpose**: Ensure AI assistant can resume partially-completed holonic implementations without reinvention or duplication.

---

## Core Principle

Plans are **specifications of desired state**, not procedural instructions. Execution must be **idempotent** - running the same plan multiple times produces the same final state regardless of starting point.

---

## Execution Protocol

### Before Implementing ANY Step:

1. **READ** the target file(s) first
2. **VERIFY** current state against plan requirements
3. **COMPARE** what exists vs what's required
4. **ACT ONLY IF NEEDED** - apply minimal change to satisfy requirement
5. **NEVER OVERWRITE** working code that already meets requirements

### Tool Selection Strategy:

- Use `search_replace` for precision edits (changing specific lines)
- Use `write` only for NEW files that don't exist
- Read before writing, always
- When updating, replace exact changed portions only

### State Tracking:

- Use plan todos as state checklist
- Checked `[x]` = requirement verified as satisfied
- Unchecked `[ ]` = requirement needs implementation
- Verify checked items actually satisfy requirements (don't trust blindly)

---

## Plan Structure Requirements

### Required Sections:

1. **Overview** - What the holonic implementation achieves (frontend + backend)
2. **Architecture Context** - Patterns being followed, parallels to existing code
3. **Implementation Steps** - Numbered, file-specific, with code snippets
4. **Files Modified** - Complete list organized by repository
5. **Expected Outcome** - Observable results for verification
6. **Testing Strategy** - How to verify completion

### Step Format (Declarative):

**BAD** (Imperative):
```
Delete X from file Y
Add Z after line 123
```

**GOOD** (Declarative with verification):
```
File: /path/to/file.dart

Current state may contain: [description]
Required state SHALL be: [description]

Code snippet showing required state:
[code]

Verification: File SHALL [not] contain [specific element]
```

### Code Snippets:

- Include COMPLETE desired state for new files
- Include CONTEXT + CHANGE for modifications
- Show exact imports, exact class names, exact structure
- Use real file paths (absolute where possible)

---

## Holonic Completeness Checklist

A component implementation is complete when ALL layers are satisfied:

### Frontend (Flutter/Dart):
- [ ] Widget created (Panel, Beam, Board, etc.)
- [ ] Widget exported in package `lib/[package_name].dart`
- [ ] Prototype updated with widget factory methods
- [ ] Prototype uses package imports (no direct file imports)
- [ ] Integration point updated (Station, Bench, etc.)
- [ ] Integration point uses package imports
- [ ] No linter errors

### Backend (Rails/Ruby):
- [ ] Model exists with associations
- [ ] Controller exists with proper concerns
- [ ] Policy exists with Scope class
- [ ] Policy handles collaboration modes (mine/shared/explore/combined)
- [ ] Policy scopes through parent associations where applicable
- [ ] Routes configured (nested if container-based)
- [ ] Schema/migrations current

### Integration:
- [ ] Repository registered in app
- [ ] Prototype registered in app
- [ ] API endpoints accessible
- [ ] Authorization working (no Pundit errors)
- [ ] UI renders data from backend

---

## Container-Based Model Pattern

When implementing child resources (e.g., Forte belongs_to Syndicate):

### Backend Policy Critical:
```ruby
# Fortes don't have direct participations
# They inherit access through syndicate
class FortePolicy < ParticipatingPolicy
  class Scope < ParticipatingPolicy::Scope
    def combined
      scope
        .joins(syndicate: :participations)  # Join through parent!
        .where('participations.participant_id = ?', participant.id)
        .distinct
    end
  end
end
```

### Frontend Panel Pattern:
```dart
class FortePanel extends ResourcePanel {
  FortePanel(Shipment shipment) : super(
    null,
    shipment: shipment,
    icon: Icons.extension,
    title: 'Fortes',
  );

  @override
  createState() => FortePanelState();
}

class FortePanelState extends ResourcePanelState<FortePanel> {
  @override
  List<Widget> get structure => [
    if (shipment.focus != null)
      AssociationFrame(
        shipment, 
        forteShipment,
        canCreate: false,
        inPanel: true,
      ),
  ];

  get forteShipment => Shipment.named('fortes').copyWith(
    notifier: shipment.notifier,
  );
}
```

---

## Resumption Strategy

When given a plan to execute:

1. **Scan all todos** - identify checked vs unchecked
2. **For checked items**: Read files to verify they actually satisfy requirements
3. **For unchecked items**: Read files to see if partially complete
4. **Create mental diff**: Required state - Current state = Work needed
5. **Execute minimal changes** to reach required state
6. **Update todos** as requirements verified complete

---

## Anti-Patterns to Avoid

### ❌ Don't:
- Overwrite entire files when only a line needs changing
- Use direct file imports when package exports exist
- Create new code that duplicates existing functionality
- Assume checked todos mean work is done (verify first)
- Apply changes without reading current state
- Skip backend when implementing frontend (must be holonic)

### ✅ Do:
- Read before writing, always
- Use search_replace for surgical precision
- Follow existing patterns (find similar code, copy structure)
- Verify holonic completeness (frontend AND backend)
- Check linter errors after changes
- Test the actual behavior (don't just trust the code)

---

## Success Metrics

Implementation is successful when:

1. ✅ All plan todos checked and verified
2. ✅ No linter errors in modified files
3. ✅ UI renders and responds to user interaction
4. ✅ API returns data without authorization errors
5. ✅ No direct file imports (all package-level)
6. ✅ Code follows established framework patterns
7. ✅ Can run plan again with zero changes (idempotent proof)

---

## Context Transfer

When starting fresh in new conversation:

**User provides**:
- Plan file (this template filled in for specific component)
- Instruction: "Execute plan, accommodating for partial implementation"

**Assistant MUST**:
- Read current state first
- Compare to plan requirements
- Apply only minimal needed changes
- Verify holonic completeness

No additional meta-instructions should be needed if plan follows this protocol.

---

## Related Documents

- `/elastic-mob/templates/component_panel_plan_template.md` - Reusable plan template
- `/elastic-mob/ANTHRO_CONTEXT_BRIEFING.md` - System overview
- `/elastic-mob/HOLONIC_ARCHITECTURE_OBSERVATIONS.md` - Architectural patterns
- `/in-concert/docs/coding_idioms.md` - Rails patterns
- `/wispera_framework/docs/MANDATORY_FRAMEWORK_ANALYSIS_PROTOCOL.md` - Flutter patterns
