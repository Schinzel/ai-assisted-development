# Module Standards for Humans and AIs

## Purpose
The purpose of this document is to establish clear guidelines for creating and maintaining modules in an AI-assisted development environment.

## Module Definition
A module is a self-contained, independently buildable unit of code that:
- Implements a clearly defined domain concept or technical capability
- Has explicit dependencies and a well-defined public API
- Can be tested in isolation from other modules

## Module Structure

### Naming
- Module names must be descriptive, reflecting their core purpose
- Use kebab-case for module directory names (e.g., `user-authentication`)

### Size Guidelines
- Aim for modules between 500-5000 lines of code
- If a module exceeds 5000 lines, consider splitting it into smaller modules
- If multiple modules are under 300 lines each and tightly coupled, consider merging them

### Required Components
Each module MUST contain:
1. Clear README.md describing:
   - The module's primary purpose (one sentence)
   - Core responsibilities (3-5 bullet points maximum)
   - The command to test this module in isolation
2. Explicit public API via interfaces in a dedicated `api` package
3. Independent test suite

### New modules
When creating new modules, follow these guidelines:

1. Place the module in the appropriate part of the hierarchy
2. Create a README.md that clearly states the module's purpose
3. Follow the dependency rules of the architecture
4. Keep modules focused on a single responsibility
5. Add the new module in [Modules](../modules.md) as both a text and as part of the diagram



## Module Dependency Rules

### General Rules
- Modules must declare ALL dependencies explicitly
- Circular dependencies between modules are PROHIBITED
- Prefer narrow dependencies (import only what you need)

### Dependency Directions
- Core domain modules should have NO dependencies on infrastructure or UI modules
- Infrastructure modules can depend on domain modules but not UI
- Cross-cutting concerns (logging, security) should be in separate modules


### Module Isolation
- No shared mutable state between modules
- Communication between modules only through defined interfaces
- No reflection or bytecode manipulation to access internals of other modules

