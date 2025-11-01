# [COMPONENT_NAME] Panel Implementation - Complete Holonic Implementation

**Template Version**: 1.0  
**Based on**: FortePanel successful implementation (Oct 2025)  
**Usage**: Fill in [PLACEHOLDERS] with component-specific values

---

## Overview

This plan implements the complete holonic pattern: frontend widget development coupled with backend policy implementation. The work builds the [STATION_NAME]'s [REGION_NAME] region with proper [COMPONENT_NAME]Panel UI while ensuring the Rails backend has complete authorization policies.

---

## Architecture Context

### Station vs Bench Parallels

- **Station**: Multi-region layout extending `RegionLayout` (e.g., [STATION_NAME] with 3 regions for comprehensive resource views)
- **Bench**: Single-resource focus extending `ResourceLayout` with associations panel (e.g., detailed [component] editing)
- Both use Shipments for state management and StateManaging mixin for Provider/Consumer patterns

### [REFERENCE_COMPONENT]Panel Pattern (Reference)

- Extends `ResourcePanel` → `Panel`
- State extends `ResourcePanelState` with `isConsumer=true, isProvider=false`
- Uses `AssociationFrame` to display related resources
- Shipment: `Shipment.named('[resource_name]').copyWith(notifier: shipment.notifier)`

### Rails Policy Pattern (Critical)

Policies define authorization scope for resources. [Container-based or participable] models ([COMPONENT_NAME] [belongs_to PARENT or has participations]) need:

- Policy class extending `ParticipatingPolicy`
- Inner `Scope` class extending `ParticipatingPolicy::Scope`
- Scope resolution based on collaboration mode (mine/shared/explore/combined)
- Participation-based filtering through joins [through parent if container-based]

---

## Implementation Steps

### Part 1: Flutter UI Components

#### 1. Remove [OLD_STUB_NAME] (if applicable)

**File**: `/Users/markratjens/Development/anthro/wispera_components/lib/src/components/[parent_component]/widgets/stubs.dart`

**Current state may contain**: `[OLD_STUB_NAME]` class as placeholder

**Required state SHALL be**: File SHALL NOT contain `[OLD_STUB_NAME]` class

**Verification**: Search for class name - should not exist

---

#### 2. Create [COMPONENT_NAME]Panel Widget

**File**: `/Users/markratjens/Development/anthro/wispera_components/lib/src/components/[component_plural]/widgets/panel.dart`

**Required state SHALL be**: File SHALL exist with following structure:

```dart
import 'package:flutter/material.dart';
import 'package:wispera_framework/wispera_framework.dart';

class [COMPONENT_NAME]Panel extends ResourcePanel {
  [COMPONENT_NAME]Panel(Shipment shipment) : super(
    null, // No content by default
    shipment: shipment,
    icon: Icons.[icon_name],
    title: '[Display Name]',
  );

  @override
  createState() => [COMPONENT_NAME]PanelState();
}

class [COMPONENT_NAME]PanelState extends ResourcePanelState<[COMPONENT_NAME]Panel> {

  @override
  List<Widget> get structure => [
    if (shipment.focus != null)
      AssociationFrame(
        shipment, 
        [component]Shipment,
        canCreate: false,
        inPanel: true,
      ),
  ];

  get [component]Shipment => Shipment.named('[component_plural]').copyWith(
    notifier: shipment.notifier,
  );
}
```

**Verification**: File exists and contains both Panel and PanelState classes

---

#### 3. Export [COMPONENT_NAME]Panel in Package

**File**: `/Users/markratjens/Development/anthro/wispera_components/lib/wispera_components.dart`

**Current state**: May or may not have export for panel

**Required state SHALL include** (in appropriate section around line [APPROX_LINE]):
```dart
// [COMPONENT_NAME] widgets
export 'src/components/[component_plural]/widgets/beam.dart';
export 'src/components/[component_plural]/widgets/panel.dart';
```

**Verification**: grep for `export.*[component_plural].*panel` succeeds

---

#### 4. Update [COMPONENT_NAME]Prototype Registry

**File**: `/Users/markratjens/Development/anthro/wispera_components/lib/src/components/[component_plural]/prototype.dart`

**Current state may have**: Direct file imports (`import 'widgets/beam.dart';`)

**Required state SHALL be**:
```dart
import 'package:wispera_framework/wispera_framework.dart';
import 'package:wispera_components/wispera_components.dart';

import 'state/core.dart';

class [COMPONENT_NAME]Prototype extends Prototype {

  @override
  get protoMap => {
    'related': (core) => [COMPONENT_NAME].related(core),
    'fromJson': (json) => [COMPONENT_NAME].fromJson(json),
    'beam': (shipment) => [COMPONENT_NAME]Beam(shipment),
    'panel': (shipment) => [COMPONENT_NAME]Panel(shipment),
  };

}
```

**Verification**: 
- No direct `widgets/` imports
- Has package imports only
- protoMap includes 'panel' entry

---

#### 5. Update [STATION_NAME] Integration

**File**: `/Users/markratjens/Development/anthro/wispera_components/lib/src/components/[parent_component]/widgets/station.dart`

**Current state may have**: 
- Direct file import for panel
- Old stub panel in region

**Required state SHALL be**:
- Only package-level imports
- Panel getter returns [COMPONENT_NAME]Panel instance
- Old stub panel removed

```dart
import 'package:flutter/material.dart';
import 'package:wispera_framework/wispera_framework.dart';
import 'package:wispera_components/wispera_components.dart';

class [STATION_NAME] extends Station {
  // ... existing code ...
}

class [STATION_NAME]State extends StationState {
  
  @override
  List<Region> get regions => [
    regionWith([contextPanel], 300.0, top: taskBar),
    // ... other regions ...
  ];

  Panel get contextPanel => [COMPONENT_NAME]Panel(shipment);
  
  // ... other panels ...
}
```

**Verification**:
- No import lines like `import '../../[component_plural]/widgets/panel.dart';`
- contextPanel returns [COMPONENT_NAME]Panel(shipment)
- Old stub class removed from file

---

### Part 2: Rails Backend Policy

#### 6. Create [COMPONENT_NAME]Policy with Scope

**File**: `/Users/markratjens/Development/anthro/in-concert/app/policies/elastic_mob/[component_singular]_policy.rb`

**Required state SHALL be**: File SHALL exist with proper Scope class

**If container-based (belongs_to parent)**:
```ruby
module ElasticMob
  class [COMPONENT_NAME]Policy < ParticipatingPolicy

    class Scope < ParticipatingPolicy::Scope
      def resolve
        case user.collaboration
        when 'mine'
          mine
        when 'shared'
          shared
        when 'explore'
          explore
        when 'combined'
          combined
        else
          scope.none
        end
      end

      def mine
        scope
          .joins([parent_association]: :participations)
          .where('participations.participant_id = ? AND participations.power = ?', participant.id, 'creator')
          .distinct
      end

      def shared
        scope
          .joins([parent_association]: :participations)
          .where('participations.participant_id = ?', participant.id)
          .distinct
      end

      def explore
        scope.none
      end

      def combined
        scope
          .joins([parent_association]: :participations)
          .where('participations.participant_id = ?', participant.id)
          .distinct
      end
    end

  end
end
```

**If directly participable (has_many participations)**:
```ruby
module ElasticMob
  class [COMPONENT_NAME]Policy < ParticipatingPolicy

    class Scope < ParticipatingPolicy::Scope
      def resolve
        case user.collaboration
        when 'mine'
          mine
        when 'shared'
          shared
        when 'explore'
          explore
        when 'combined'
          combined
        else
          scope.none
        end
      end

      def mine
        scope
          .left_outer_joins(:participations)
          .where('participations.participant_id = ? AND participations.power = ?', participant.id, 'creator')
          .distinct
      end

      def shared
        scope
          .left_outer_joins(:participations)
          .where('participations.participant_id = ?', participant.id)
          .distinct
      end

      def explore
        scope.none
      end

      def combined
        scope
          .left_outer_joins(:participations)
          .where('participations.participant_id = ?', participant.id)
          .distinct
      end
    end

  end
end
```

**Critical Points**:
- Container-based: Scope through parent association (`.joins(parent: :participations)`)
- Directly participable: Scope through own participations (`.left_outer_joins(:participations)`)
- Controller `Nesting` concern handles filtering by parent_id from route
- Policy controls WHO sees resources (based on participation)
- Nesting controls WHICH resources shown (filtered by parent_id)

**Verification**: 
- File exists at correct path
- Contains Scope class
- Scope responds to all collaboration modes
- Uses correct join strategy for model type

---

## Files Modified

### Flutter (wispera_components)

1. `lib/src/components/[parent_component]/widgets/stubs.dart` - Remove stub (if applicable)
2. `lib/src/components/[component_plural]/widgets/panel.dart` - NEW: [COMPONENT_NAME]Panel widget
3. `lib/wispera_components.dart` - Export [COMPONENT_NAME]Panel
4. `lib/src/components/[component_plural]/prototype.dart` - Register panel, fix imports
5. `lib/src/components/[parent_component]/widgets/station.dart` - Integrate [COMPONENT_NAME]Panel, fix imports

### Rails (in-concert)

6. `app/policies/elastic_mob/[component_singular]_policy.rb` - NEW: Authorization policy with scope

---

## Expected Outcome

### Frontend

- [STATION_NAME] [REGION_NAME] region displays [COMPONENT_NAME]Panel showing [component_plural] for selected [parent_resource]
- Clean package-level imports (no direct file references)
- Proper prototype registration for dynamic widget creation

### Backend

- [COMPONENT_NAME]Policy::Scope resolves authorization for [component] collections
- Scoping through [parent] participations ([component_plural] inherit [parent] access) OR direct participations
- Nested routes `/[parent_plural]/:id/[component_plural]` work correctly (if container-based)
- No more `Pundit::NotDefinedError`

---

## Testing Strategy

### Frontend Testing
1. Run `flutter pub get` in wispera_components
2. Verify no linter errors: check imports and widget structure
3. Run wispera-mobsta app
4. Navigate to [parent_resource] in UI
5. Confirm [COMPONENT_NAME]Panel renders in [REGION_NAME] region
6. Verify panel shows [component_plural] associated with [parent_resource]

### Backend Testing
1. Verify Rails server starts without errors
2. Test authorization: `GET /api/elastic_mob/[parent_plural]/:id/[component_plural]?collaboration=combined`
3. Verify [component_plural] returned match [parent_resource] participation
4. Verify only [component_plural] from specified [parent_resource] returned (if container-based)
5. Test all collaboration modes (mine, shared, combined)
6. Confirm no `Pundit::NotDefinedError` in logs

### Integration Testing
1. In UI, select [parent_resource] with [component_plural]
2. Verify [COMPONENT_NAME]Panel populates with data
3. Test panel scrolling and interaction
4. Verify state management (select different [parent_resource], panel updates)

---

## Execution Instructions

**For AI Assistant**:

When given this plan, follow the Holonic Implementation Protocol:

1. READ current state of each file mentioned
2. COMPARE current state to required state
3. APPLY only minimal changes needed to satisfy requirements
4. VERIFY holonic completeness (frontend AND backend)
5. UPDATE todos as work completed

Do NOT overwrite working code. Do NOT skip verification steps.

See: `/elastic-mob/system_foundation/holonic_implementation_protocol.md`

---

## Component-Specific Values

**Fill in before using template**:

- `[COMPONENT_NAME]`: Capitalized class name (e.g., Talent, Game, Mobsta)
- `[component_singular]`: Lowercase singular (e.g., talent, game, mobsta)
- `[component_plural]`: Lowercase plural (e.g., talents, games, mobstas)
- `[STATION_NAME]`: Station class name (e.g., SyndicateStation)
- `[REGION_NAME]`: Which region (e.g., first, second, context)
- `[parent_component]`: Parent component (e.g., syndicates)
- `[parent_association]`: Parent association symbol (e.g., :syndicate)
- `[parent_resource]`: Parent resource name (e.g., syndicate)
- `[parent_plural]`: Parent plural (e.g., syndicates)
- `[icon_name]`: Material icon name (e.g., extension, star, psychology)
- `[Display Name]`: UI title (e.g., 'Fortes', 'Talents', 'Games')
- `[REFERENCE_COMPONENT]`: Existing similar component to reference (e.g., Mobsta)
- `[OLD_STUB_NAME]`: Stub class to remove (e.g., TalentContextStub)
- `[APPROX_LINE]`: Approximate line number for export (find by searching similar components)

---

## To-dos

- [ ] Remove [OLD_STUB_NAME] from stubs.dart (if applicable)
- [ ] Create [COMPONENT_NAME]Panel widget following [REFERENCE_COMPONENT]Panel pattern
- [ ] Export [COMPONENT_NAME]Panel to wispera_components.dart exports
- [ ] Update [COMPONENT_NAME]Prototype registry with panel and fix imports to use packages
- [ ] Update [STATION_NAME] to use [COMPONENT_NAME]Panel and fix imports
- [ ] Create [COMPONENT_NAME]Policy with Scope for Rails authorization
