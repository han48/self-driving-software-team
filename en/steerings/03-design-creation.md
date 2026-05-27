---
inclusion: fileMatch
fileMatchPattern: '**/design.md'
---

# Steering: Creating Design

## When to Apply
When AI generates design.md from approved requirements.

## Rules

1. **Mandatory format:**
   - Heading: `# Design Document: [Feature name]`
   - Sections in order: Overview → Site Map → GUI Design → Architecture → Data Models / Database Design + ERD → Infrastructure Design → Components and Interfaces → Correctness Properties → Error Handling → Testing Strategy

2. **GUI Design:**
   - Do not write directly in design.md
   - Create HTML prototype in `openspec/specs/{feature-name}/gui/`
   - In design.md only reference: `> 📁 Refer: openspec/specs/{feature-name}/gui/`
   - HTML prototype must have full navigation flow, clickable, responsive
   - Use lightweight CSS framework (TailwindCSS, Bootstrap)

3. **Architecture:**
   - Use Mermaid diagrams (graph TD for system, sequenceDiagram for flows)
   - Include both system overview and sequence diagrams

4. **Correctness Properties:**
   - Use format: `*For any* [condition], [property must hold]`
   - Each property must have: `**Validates: Requirements X.X**`

5. **Self-check loop before presenting to Human for approval:**
   - Does the design cover all requirements?
   - Is the architecture scalable and secure?
   - Is the DB design properly normalized?
   - Are there conflicts between components?
   - Are there technical risks? → create RISK-XXX
   - Are there questions needing clarification? → create QA-XXX
   - Repeat until no issues remain

6. **Output location:** `openspec/specs/{feature-name}/design.md`
