# System Patterns: easepick

## Architecture Overview

### Monorepo Structure
```
packages/
├── core/           # Core date picker functionality
├── datetime/       # Date/time utilities and formatting
├── base-plugin/    # Base plugin architecture
├── [feature]-plugin/ # Feature-specific plugins
└── bundle/         # Combined distribution package
```

### Core Components

#### Core Class
- **Purpose**: Main date picker controller and API
- **Responsibilities**:
  - DOM manipulation and event handling
  - State management (selected dates, calendars)
  - Plugin coordination
  - UI rendering orchestration
- **Pattern**: Singleton per picker instance

#### Calendar Class
- **Purpose**: Calendar grid rendering and navigation
- **Responsibilities**:
  - Month/year navigation
  - Date cell generation
  - Visual state management
- **Pattern**: Composition within Core

#### PluginManager Class
- **Purpose**: Plugin lifecycle and coordination
- **Responsibilities**:
  - Plugin registration and initialization
  - Event delegation to plugins
  - Plugin dependency management
- **Pattern**: Strategy pattern for plugin types

## Key Design Patterns

### Plugin Architecture
- **Base Plugin**: Abstract foundation for all plugins
- **Extension Points**: Hooks for render, event, and lifecycle events
- **Composition**: Plugins enhance core functionality without modification
- **Interface Segregation**: Each plugin implements only relevant interfaces

### Shadow DOM Encapsulation
- **Purpose**: Style isolation and component encapsulation
- **Benefits**: Prevents CSS conflicts, enables theme scoping
- **Implementation**: Wrapper element with shadow root containing picker UI

### Event-Driven Communication
- **Custom Events**: DOM-based communication between components
- **Event Bubbling**: Events propagate through shadow boundary
- **Plugin Hooks**: Standardized event system for plugin integration

### Factory Pattern for Creation
- **Core.create()**: Factory function for picker instantiation
- **Configuration Merging**: Default options with user overrides
- **Validation**: Option type checking and normalization

## Component Relationships

### Core ↔ Calendar
- Core owns Calendar instance
- Calendar renders within Core's shadow DOM
- Core delegates rendering calls to Calendar

### Core ↔ PluginManager
- Core initializes PluginManager
- PluginManager coordinates plugin lifecycle
- Plugins can modify Core behavior through events

### PluginManager ↔ Plugins
- PluginManager maintains plugin registry
- Plugins implement BasePlugin interface
- Event delegation through PluginManager

## Data Flow

### Initialization
1. User creates picker with `create(options)`
2. Core merges options with defaults
3. Core creates Shadow DOM structure
4. PluginManager initializes configured plugins
5. Calendar renders initial state

### Date Selection
1. User clicks date cell
2. Core receives click event
3. Plugins can intercept/modify selection
4. Core updates internal state
5. UI re-renders to reflect changes
6. 'select' event dispatched to user

### Plugin Integration
1. Plugin registers event listeners
2. Core events trigger plugin hooks
3. Plugin modifies DOM/state as needed
4. Plugin can dispatch custom events
5. Core continues with modified context

## State Management

### Internal State
- `calendars[]`: Current visible calendar dates
- `datePicked[]`: User-selected dates
- `options`: Merged configuration
- `ui`: DOM element references

### State Updates
- Direct mutation with side effects
- Automatic UI re-rendering on state changes
- Plugin hooks for state modification interception

## Critical Implementation Paths

### Rendering Pipeline
```
renderAll() → trigger('render') → Calendar.render() → DOM updates
```

### Event Handling
```
User Interaction → Core event listener → Plugin hooks → State update → Re-render
```

### Plugin Loading
```
Core init → PluginManager.initialize() → Plugin.attach() → Event binding
```

## Technical Decisions

### Shadow DOM Choice
- **Rationale**: Style encapsulation, no global CSS pollution
- **Trade-offs**: Event handling complexity, limited parent document access
- **Mitigation**: Custom event system with composed path handling

### Plugin System Design
- **Rationale**: Extensibility without core modification
- **Architecture**: Interface-based with lifecycle hooks
- **Benefits**: Feature isolation, independent development and testing

### Monorepo Organization
- **Rationale**: Shared tooling, atomic releases, clear dependency management
- **Structure**: Separate packages for core, plugins, and utilities
- **Benefits**: Flexible consumption (individual plugins or full bundle)
