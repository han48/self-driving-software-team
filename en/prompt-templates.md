# Prompt Templates – AI Prompt Templates for Each Phase

> Use the templates below when you need AI to generate output in the correct process format.
> Replace `{...}` with your actual content.

---

## 1. Generate Requirements (Spec)

### Basic Prompt

```
Generate requirements.md for the following feature:

{Raw requirement description – can be 1-2 sentences or meeting notes}

Requirements:
- Use EARS notation (THE/WHEN/IF + SHALL)
- Include User Story for each requirement
- Cover both happy path and error cases
- Define Glossary for domain-specific terms
- Number acceptance criteria (1, 2, 3...)
- If ambiguity detected → create QA-XXX
- If risk detected → create RISK-XXX

Output format: follow structure in Appendix A.2
File location: openspec/specs/{feature-name}/requirements.md
```

### Advanced Prompt (with context)

```
Context:
- Project: {project name}
- Tech stack: {Node.js + PostgreSQL + React...}
- Existing specs: {reference to related specs if any}
- Constraints: {deadline, budget, technical limitations}

Raw requirement from client:
"{Copy paste original requirement}"

Generate requirements.md. Notes:
- Must be compatible with current architecture
- Check for conflicts with existing specs
- Flag if requirement exceeds contract scope
```

---

## 2. Generate Design

### Basic Prompt

```
Based on the approved requirements.md (file: openspec/specs/{feature}/requirements.md),
generate design.md including:

1. Overview (tech stack, approach)
2. Site Map (Mermaid graph)
3. GUI Design (generate HTML prototype into gui/ folder)
4. Architecture (sequence diagrams)
5. Database Design + ERD
6. Infrastructure Design
7. Components & Interfaces
8. Correctness Properties (for property-based testing)
9. Error Handling strategy
10. Testing Strategy

Requirements:
- Each component must reference the corresponding requirement
- GUI prototype must be directly openable in browser
- Database design must include index strategy
- Correctness Properties in "For any..." format

Output format: follow structure in Appendix A.3
```

### Prompt for small features (skip sections)

```
Feature size: Small (< 1 day)
Only generate design.md with sections:
- Overview
- Components & Interfaces
- Testing Strategy

Skip: Site Map, GUI, Architecture, Database, Infrastructure
(Feature too small, full design not needed)
```

---

## 3. Generate Tasks

### Basic Prompt

```
Based on the approved design.md (file: openspec/specs/{feature}/design.md),
generate tasks.md:

Requirements:
- Each task < 4 hours effort
- Nested checkbox format (- [ ] 1. / - [ ] 1.1)
- Each sub-task has _Requirements: X.X_ reference
- Task Dependency Graph (JSON waves)
- Include tasks for: code, unit test, integration test, E2E test
- Logical order: setup → core logic → tests → integration

Output format: follow structure in Appendix A.4
```

---

## 4. Generate RADIO Report

### Basic Prompt

```
Generate/update RADIO report for feature {name}:

Data sources:
- tasks.md: {X}/{Y} tasks done
- CI results: {pass/fail, coverage %}
- Blockers: {list if any}
- Recent changes: {recent commits}

Format:
- R: Status + specific metrics
- A: Current task + next steps
- D: Blockers/risks (or "None")
- I: Context affecting progress (or "—")
- O: Results achieved + numbers

If there are serious D items → check if RISK-XXX/ISSUE-XXX exists, create if not.
```

---

## 5. Generate Bugfix Spec

### Basic Prompt

```
Bug report: "{bug description}"
Steps to reproduce: {steps}
Expected: {expected behavior}
Actual: {actual behavior}

Generate bugfix.md with:
1. Current Behavior (Defect) – EARS format
2. Expected Behavior (Correct) – EARS format
3. Unchanged Behavior (Regression Prevention) – EARS format

Then generate design.md with:
- 5WHY root cause analysis (dig to the real root cause)
- Fix Implementation (surgical, minimal changes)
- Files Changed (clear table)
- Prevention (what to do so this type of bug doesn't recur)
- Correctness Properties

Output: openspec/bugs/{bug-name}/bugfix.md + design.md + tasks.md
```

---

## 6. Evaluate CR (Change Request)

### Basic Prompt

```
New CR received:
- From: {source}
- Description: "{CR content}"
- Details: {bullet list}

Please:
1. Assess impact on current system
2. Estimate effort (hours/days)
3. Assess risk if implemented
4. Suggest priority (High/Medium/Low) with reasoning
5. Check for conflicts with existing specs/design
6. Record in backlog.md section "Pending" in correct format
```

---

## 7. Detect Risk & Issue

### Prompt for Risk Detection

```
Review {file: spec/design/code} and detect risks:

Check for:
- Security risks (injection, auth bypass, data leak)
- Performance risks (N+1, memory leak, timeout)
- Scalability risks (single point of failure, bottleneck)
- Dependency risks (deprecated lib, breaking changes)
- Business risks (scope creep, unclear requirement)

For each risk detected:
- Assess Probability (H/M/L) + Impact (H/M/L)
- Write Prevention (what to do to avoid)
- Write Mitigation (what to do if it occurs)
- Identify Trigger (warning signs)

Output: append to openspec/risk-issue/risks.md in RISK-XXX format
```

---

## 8. Generate Q&A

### Prompt when ambiguity detected

```
While reading spec/design, I found an ambiguous point:
"{description of ambiguity}"

Please:
1. Create QA-XXX with a clear question including context
2. List possible options (if known)
3. Suggest default answer (if inferable)
4. Identify who needs to answer (Client / Architect / Lead)
5. Assess priority (High if blocks implementation)

Output: openspec/qa/QA-XXX/thread.md + update index.md
```

---

## Tips for Using Prompts

1. **More context is better:** Attach related files, don't just describe
2. **Reference existing specs:** If there are related specs, mention them
3. **Specify constraints:** Deadline, tech stack, team size affect output
4. **Iterative:** If output isn't right → give specific feedback, don't re-prompt from scratch
5. **Verify format:** After AI generates → check format matches Appendix A/B
