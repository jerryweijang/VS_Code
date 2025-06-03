# Essential Guide to Code Refactoring and Improvement

## Core Principles

### SOLID Principles
- **S(SRP)**: Classes should have only one reason to change
- **O(OCP)**: Open for extension, closed for modification
- **L(LSP)**: Subclasses must be completely substitutable for their parent classes
- **I(ISP)**: Clients should not depend on interfaces they don't use
- **D(DIP)**: Depend on abstractions, not implementations

### Practical Guidelines
- **DRY**: Avoid repetition, but follow the "Rule of Three"—abstract only after code repeats three times
- **YAGNI**: Implement only what's currently needed, avoid over-engineering
- **Fail Fast**: Expose problems early, don't hide errors
- **Pure Functions First**: Same input produces same output, without side effects
- **CQS**: Methods either perform an action or return a result, never both
- **Principle of Least Surprise**: Behavior should match what the name suggests
- **TDD**: Test first, implement next, then refactor

## Error Handling Essentials

- **Transparent Reporting**: Clearly report problem source and nature, such as "Database Connection Failure" vs "Query Syntax Error"
- **Appropriate Fault Tolerance**: Implement graceful degradation only when required by business, otherwise reject invalid input
- **Complete Logging**: Record errors with context, but don't automatically "fix" issues that hide problems

## Design Structure Guidelines

- **Unified Style**: Follow project naming conventions and formatting standards
- **Low Coupling, High Cohesion**: Centralize related functionality, use interface design and dependency injection
- **Expressive Naming**: Names should reflect intent, comments explain "why" not "what"
- **Concise Logic**: Avoid deeply nested structures, break down complex conditions

## Balancing Strategies

- **Repetition vs Abstraction**: Abstract after three repetitions, refactor when pattern is confirmed
- **Failure vs Defense**: Business core fails fast, UI layer employs appropriate defense
- **Flexible Application**: Choose principles based on project characteristics, team size, and stage
- **Progressive Complexity**: Start with simple solutions, increase complexity only when requirements are confirmed
- **Exception Management**: Record rationale and reach consensus when violating principles

## Refactoring Considerations

- **UI Compatibility**: Verify event handling, data binding, and visual feedback
- **Dependency Analysis**: Identify explicit (references) and implicit (assumptions) dependencies
- **Interface Stability**: Adopt "extend, don't modify" strategy, use deprecation notices
- **Iterative Improvement**: Propose → evaluate → modify → reassess, leverage diverse feedback
- **Forbidden Behaviors**: Avoid architectural conflicts, showing off with unnecessary complexity, excessive error tolerance

## Reflection Checklist
1. Compatible with overall architecture?
2. Appropriate complexity?
3. Clear impact scope?
4. Simpler solution exists?
5. Adaptable to requirement changes?
6. Error handling robust?
7. Reasonable fault tolerance?

## Implementation Priorities
1. **Fix Errors** → 2. **Optimize Performance** → 3. **Improve Readability** → 4. **Eliminate Redundancy**

## Charts and Documentation

### Mermaid Chart Guidelines
- **Chart Types Usage**:
  - **Flowcharts**: Decision paths and process flows
    ```mermaid
    flowchart LR
      Start --> Process --> Decision{Evaluate}
      Decision -->|Yes| ResultA
      Decision -->|No| ResultB
    ```
  - **Sequence Diagrams**: Component interaction sequences
    ```mermaid
    sequenceDiagram
      Client->>Server: Request data
      Server->>Database: Query
      Database-->>Server: Return results
      Server-->>Client: Response
    ```
  - **Class Diagrams**: Structure and relationships
    ```mermaid
    classDiagram
      ClassA --|> AbstractClass
      ClassB --|> AbstractClass
      ClassA -- ClassC : uses
    ```
  - **State Diagrams**: State transitions
  - **Entity Relationship Diagrams**: Data structures

- **Chart Best Practices**:
  - Clear direction setting (LR horizontal/TB vertical)
  - Appropriate node density, group related elements
  - Consistent and meaningful color coding
  - Legend for complex charts
  - High contrast for readability

### Documentation Practice Essentials
- **Decision Records**: Use ADR format, including background, decision, and consequences
- **Reflection Documents**: Record design evolution and rejected alternatives
- **Example-Driven**: Illustrate principles with actual code examples

## Common Refactoring Patterns
- **Extract Method/Class**: Isolate logic segments or related functionality
- **Inline Simplification**: Merge unnecessary indirect calls
- **Cohesive Reorganization**: Reorganize code by responsibilities
- **Parameter Simplification**: Combine multiple parameters into objects
- **Responsibility Separation**: Split components with multiple responsibilities

*Principles serve the product and team, not vice versa. Technical choices should be based on solving real problems, not chasing trends.*