# Implementation Plan: SDLC AI Agents Platform

## Overview

This plan implements the SDLC AI Agents Platform — a multi-agent system covering the full software development lifecycle. Implementation follows a bottom-up dependency order: infrastructure first, then core frameworks, then specialized agents and management systems, and finally integration/observability layers. All code is TypeScript with PostgreSQL, Redis, RabbitMQ, Vector DB (Qdrant), and Object Storage (S3).

## Tasks

- [ ] 1. Project Setup and Core Infrastructure
  - [ ] 1.1 Initialize project structure, monorepo configuration, and shared TypeScript config
    - Create directory structure matching design: `/src`, `/tests`, `/config`, `/docs`
    - Configure TypeScript, ESLint, Prettier, Jest + fast-check
    - Set up monorepo packages: `core`, `agents`, `orchestrator`, `api`, `admin`, `services`
    - _Requirements: R16-C7, R37-C1_

  - [ ] 1.2 Create database schema and migration infrastructure
    - Implement all 25+ SQL tables from design (organizations, teams, projects, users, user_roles, agents, ai_models, pipelines, pipeline_steps, artifacts, review_gates, prompt_logs, sentiment_results, knowledge_documents, faq_entries, feedback, sku_tickets, quotas, audit_logs, change_requests, qa_threads, eval_test_suites, eval_results, templates, radio_reports, project_brain_entries)
    - Set up migration tooling (e.g., node-pg-migrate or Prisma)
    - _Requirements: R15, R16, R24, R33, R34, R35_

  - [ ] 1.3 Set up message broker infrastructure (RabbitMQ/Kafka)
    - Define exchange/queue topology for agent communication
    - Implement event publisher and consumer base classes
    - Define event schemas for pipeline events, agent events, review events
    - _Requirements: R12-C1, R12-C2, R29-C3_

  - [ ] 1.4 Set up Redis caching and rate limiting infrastructure
    - Configure Redis connection pool
    - Implement cache service with TTL management
    - Implement distributed lock primitives for quota enforcement
    - _Requirements: R28, R35-C4, R35-C5_

  - [ ] 1.5 Set up object storage (S3) for large artifacts
    - Implement storage service interface for artifact content
    - Configure bucket policies and lifecycle rules
    - _Requirements: R15-C1, R28-C2, R28-C3_

- [ ] 2. Checkpoint - Ensure all infrastructure tests pass
  - Ensure all tests pass, ask the user if questions arise.

- [ ] 3. Core Framework — Agent Base and Registry
  - [ ] 3.1 Implement IAgent interface and AgentBase abstract class
    - Define AgentRole, AgentStatus, AgentConfig, AgentTask, AgentOutput types
    - Implement lifecycle methods: initialize, execute, handleFeedback, shutdown, heartbeat
    - Implement health check mechanism with configurable intervals
    - _Requirements: R1-C1, R1-C2, R1-C4, R1-C5, R1-C7_

  - [ ] 3.2 Implement IAgentRegistry service
    - Implement register, deactivate, updateConfig, getAgent, listAgents
    - Enforce validation: name ≤ 100 chars, role required, modelId exists
    - Enforce unique (name, role) constraint for active agents
    - Implement deactivation logic (allow running tasks to complete)
    - _Requirements: R1-C1, R1-C2, R1-C3, R1-C5, R1-C6_

  - [ ]* 3.3 Write property tests for Agent Registration (Properties 1-4)
    - **Property 1: Agent Registration Validation (Round-trip Completeness)**
    - **Property 2: Agent ID Uniqueness**
    - **Property 3: Active Agent Name-Role Uniqueness Constraint**
    - **Property 4: Temperature Configuration Bounds**
    - **Validates: Requirements R1-C1, R1-C2, R1-C3, R1-C6, R1-C7**

  - [ ]* 3.4 Write property test for Agent Deactivation (Property 25)
    - **Property 25: Agent Deactivation Prevents New Task Assignment**
    - **Validates: Requirements R1-C5**

- [ ] 4. Context Store Implementation
  - [ ] 4.1 Implement IContextStore service
    - Implement write, read, listVersions, delete, query operations
    - Enforce artifact_id uniqueness + version constraints
    - Implement FIFO eviction when versions exceed 100
    - Implement version conflict detection on duplicate (artifact_id, version) writes
    - Ensure read-after-write consistency
    - Integrate with S3 for content storage and PostgreSQL for metadata
    - _Requirements: R15-C1, R15-C2, R15-C3, R15-C4, R15-C5, R15-C6_

  - [ ]* 4.2 Write property tests for Context Store (Properties 5-8)
    - **Property 5: Artifact Version Monotonic Increment**
    - **Property 6: Context Store Read-After-Write Consistency (Round-trip)**
    - **Property 7: Context Store Version Limit Invariant**
    - **Property 8: Context Store Version Conflict Detection**
    - **Validates: Requirements R15-C1, R15-C2, R15-C3, R15-C4**

  - [ ]* 4.3 Write unit tests for Context Store edge cases
    - Test not-found error for non-existent artifact_id/version
    - Test authentication enforcement (unauthenticated access rejected)
    - Test boundary: exactly 100 versions behavior
    - _Requirements: R15-C5, R15-C6_

- [ ] 5. Model Router and AI Infrastructure
  - [ ] 5.1 Implement IModelRouter service
    - Implement route, registerModel, configureRouting methods
    - Implement routing logic: data sensitivity → self-hosted, task complexity → model selection, cost profile → optimization
    - Implement fallback mechanism between deployment modes
    - Implement circuit breaker for external AI provider calls
    - _Requirements: R39-C1, R39-C2, R39-C3, R39-C6, R39-C7, R40-C1, R40-C3, R40-C4, R40-C7_

  - [ ] 5.2 Implement AI provider adapters (Cloud + Self-Hosted)
    - Implement unified inference interface for both deployment modes
    - Implement CloudConfig adapter (OpenAI, Anthropic, AWS Bedrock)
    - Implement SelfHostedConfig adapter (Ollama, vLLM endpoints)
    - Implement health check for self-hosted models
    - _Requirements: R40-C1, R40-C2, R40-C5_

  - [ ] 5.3 Implement Embedding Service
    - Implement embedding generation for Knowledge Store and FAQ matching
    - Support multiple embedding model backends
    - _Requirements: R17-C1, R17-C2_

  - [ ]* 5.4 Write property test for Model Routing — Data Sensitivity (Property 18)
    - **Property 18: Model Routing — Data Sensitivity Constraint**
    - **Validates: Requirements R39-C3, R40-C3, R40-C6**

  - [ ]* 5.5 Write unit tests for Model Router
    - Test cost-performance profile routing decisions
    - Test fallback behavior when primary model unavailable
    - Test circuit breaker state transitions
    - _Requirements: R39-C3, R39-C6, R40-C4_

- [ ] 6. Pipeline Orchestrator
  - [ ] 6.1 Implement IPipelineOrchestrator service
    - Implement createPipeline, startPipeline, pausePipeline, resumePipeline, cancelPipeline, getPipelineStatus
    - Implement pipeline state machine (Created → Running → Paused → Completed/Failed/Cancelled)
    - Implement sequential and conditional parallel execution modes
    - _Requirements: R12-C1, R12-C3, R12-C4_

  - [ ] 6.2 Implement Task Scheduler with dependency graph execution
    - Parse PipelineStep dependencies into DAG
    - Implement parallel execution for independent steps
    - Implement failure propagation (stop dependents, continue independents)
    - _Requirements: R12-C1, R12-C2_

  - [ ] 6.3 Implement agent timeout detection and retry logic
    - Monitor heartbeat responses (300s timeout)
    - Implement retry with 60s interval, max 3 total attempts
    - Transition to "failed" after exhausting retries
    - _Requirements: R12-C5_

  - [ ] 6.4 Implement Interaction Mode Router (IModeRouter)
    - Implement selectMode, switchMode, getActiveMode
    - Configure pipeline steps per mode: spec, fix, vibe
    - Preserve context when switching modes mid-session
    - _Requirements: R13-C1, R13-C2, R13-C3, R13-C4, R13-C5_

  - [ ]* 6.5 Write property tests for Pipeline (Properties 10-11)
    - **Property 10: Pipeline Dependency Graph Failure Propagation**
    - **Property 11: Pipeline Agent Timeout State Machine**
    - **Validates: Requirements R12-C2, R12-C5**

  - [ ]* 6.6 Write unit tests for Orchestrator
    - Test pipeline summary report generation on completion
    - Test mode switching preserves accumulated artifacts
    - Test conditional parallel step execution
    - _Requirements: R12-C4, R13-C5_

- [ ] 7. Checkpoint - Ensure core framework and orchestrator tests pass
  - Ensure all tests pass, ask the user if questions arise.

- [ ] 8. Review Gate Manager
  - [ ] 8.1 Implement IReviewGateManager service
    - Implement createGate, submitForReview, approve, reject, getChecklist
    - Implement checklist enforcement (all required items must be checked before approval)
    - Implement rejection counter and "blocked" status after 3 consecutive rejections
    - Implement reminder (24h) and escalation (48h) timers
    - _Requirements: R14-C1, R14-C2, R14-C3, R14-C4, R14-C5, R14-C6_

  - [ ] 8.2 Implement Approve Checklist system
    - Define default checklists per gate type (requirements, design, tasks, PR)
    - Implement conditional checklist items
    - Implement custom checklist configuration per-project/team
    - _Requirements: R47-C1, R47-C2, R47-C3, R47-C4, R47-C5_

  - [ ]* 8.3 Write property tests for Review Gate (Properties 12, 20)
    - **Property 12: Review Gate Blocking After 3 Rejections**
    - **Property 20: Approve Checklist Enforcement**
    - **Validates: Requirements R14-C6, R47-C2**

  - [ ]* 8.4 Write unit tests for Review Gate
    - Test reminder/escalation timing
    - Test checklist metrics tracking (first-time-pass rate)
    - Test conditional checklist item visibility
    - _Requirements: R14-C5, R47-C5, R47-C6_

- [ ] 9. Output Validation and Self-Review Loop
  - [ ] 9.1 Implement IValidationEngine service
    - Implement validate method with rule types: schema, required_sections, regex, guardrail
    - Implement retry loop (max 3 attempts) with "validation_failed" marking
    - Implement enterprise guardrails: PII/credentials detection, output length limits, blacklist
    - _Requirements: R22-C1, R22-C2, R22-C3, R22-C4, R22-C5_

  - [ ] 9.2 Implement Self-Review Loop Engine
    - Implement selfReview with criteria: clarity, completeness, consistency, compliance, riskDetection
    - Implement iterative self-fix loop (max 3 iterations)
    - Attach metadata: iterations count, issues found/fixed, confidence score, remaining issues
    - Wire processing chain: Self-Review → Validation → Human Review
    - _Requirements: R43-C1, R43-C2, R43-C3, R43-C4, R43-C5_

  - [ ]* 9.3 Write property tests for Validation and Self-Review (Properties 14, 19)
    - **Property 14: Validation Retry Bounded Loop**
    - **Property 19: Self-Review Loop Termination**
    - **Validates: Requirements R22-C3, R22-C4, R43-C2, R43-C4**

  - [ ]* 9.4 Write unit tests for Validation Engine
    - Test guardrail enforcement (PII detection, blacklist matching)
    - Test validation rule CRUD and assignment per agent type
    - Test self-review logging completeness
    - _Requirements: R22-C5, R22-C6, R22-C7, R43-C6_

- [ ] 10. Git Integration Service
  - [ ] 10.1 Implement IGitService
    - Implement commit, createBranch, mergeBranch, createTag, revert, push
    - Implement commit message format: `[agent_role] prompt summary` with metadata body
    - Implement branching strategy: pipeline → stage → task → sub-task branches
    - Implement merge flow: sub-task → task → pipeline → main
    - _Requirements: R16-C1, R16-C3, R16-C4, R16-C5, R16-C6_

  - [ ] 10.2 Implement Context Store ↔ Git synchronization
    - Ensure every artifact has git_commit_sha reference
    - Implement version-to-commit mapping
    - Implement tag creation on task/stage completion
    - _Requirements: R16-C9, R16-C10, R16-C11_

  - [ ]* 10.3 Write unit tests for Git Integration
    - Test commit message format compliance
    - Test branching hierarchy creation and merge flow
    - Test rollback via git revert
    - _Requirements: R16-C3, R16-C6, R16-C10_

- [ ] 11. Unified Failure Handling
  - [ ] 11.1 Implement IFailureHandler service
    - Implement handleFailure with 3-attempt escalation: self-fix → different approach → root cause analysis
    - Implement rollback strategy per scope (code, spec, feature, deployment)
    - Implement Human override mechanism for 4th retry
    - Generate failure report, RADIO "D" entry, and ISSUE-XXX creation
    - _Requirements: R50-C1, R50-C2, R50-C3, R50-C4, R50-C5_

  - [ ]* 11.2 Write property test for Unified Retry Escalation (Property 22)
    - **Property 22: Unified Retry Escalation Path**
    - **Validates: Requirements R50-C1, R50-C2**

  - [ ]* 11.3 Write unit tests for Failure Handling
    - Test rollback scope determination
    - Test failure report generation content
    - Test configurable retry count per-agent
    - _Requirements: R50-C4, R50-C5, R50-C6_

- [ ] 12. Checkpoint - Ensure validation, git, and failure handling tests pass
  - Ensure all tests pass, ask the user if questions arise.

- [ ] 13. Knowledge Store and RAG
  - [ ] 13.1 Implement IKnowledgeStore service
    - Implement ingest (chunk documents, create embeddings, index)
    - Implement search (semantic similarity with top-K, configurable scope)
    - Implement update and delete with automatic embedding refresh (within 300s)
    - Support document formats: plain text, Markdown, PDF, DOCX
    - Configure scoping: global, organization, project, agent
    - _Requirements: R17-C1, R17-C2, R17-C3, R17-C4, R17-C5, R17-C6_

  - [ ]* 13.2 Write unit tests for Knowledge Store
    - Test semantic search returns relevant results within 200ms
    - Test embedding update on document modification
    - Test scope isolation between projects
    - _Requirements: R17-C1, R17-C4, R17-C5, R17-C7_

- [ ] 14. FAQ Store and Best-Match
  - [ ] 14.1 Implement IFAQStore service
    - Implement CRUD operations for FAQ entries
    - Implement semantic matching with relevance score calculation
    - Implement threshold-based routing: score ≥ threshold → faq_direct, else → fallback_to_ai
    - Ensure matching latency < 200ms
    - _Requirements: R18-C1, R18-C2, R18-C3, R18-C4, R18-C5, R18-C6, R18-C7_

  - [ ]* 14.2 Write property test for FAQ Threshold Routing (Property 13)
    - **Property 13: FAQ Threshold Routing Decision**
    - **Validates: Requirements R18-C3, R18-C4**

  - [ ]* 14.3 Write unit tests for FAQ Store
    - Test CRUD operations and status transitions
    - Test configurable confidence threshold per-agent
    - Test logging of FAQ match events
    - _Requirements: R18-C1, R18-C5, R18-C7, R18-C8_

- [ ] 15. Project Brain
  - [ ] 15.1 Implement Project Brain service
    - Auto-collect and index artifacts, decision logs, conversation summaries, terminology
    - Implement knowledge graph: requirement ↔ design ↔ task ↔ code ↔ test relationships
    - Implement natural language query interface (< 500ms latency)
    - Auto-update embeddings within 120s of changes
    - Implement decision log extraction from review comments
    - _Requirements: R21-C1, R21-C2, R21-C3, R21-C4, R21-C5, R21-C8_

  - [ ] 15.2 Implement Project Brain terminology and retention management
    - Implement project-specific terminology detection and user confirmation
    - Implement configurable retention policies (permanent vs. archivable)
    - Implement UI for viewing, searching, and manually adding entries
    - Implement metrics: items indexed, storage size, hit rate, gap analysis
    - _Requirements: R21-C6, R21-C7, R21-C9, R21-C10_

  - [ ]* 15.3 Write unit tests for Project Brain
    - Test knowledge graph relationship queries
    - Test decision log auto-extraction
    - Test retention policy enforcement (expiry, archival)
    - _Requirements: R21-C3, R21-C5, R21-C9_

- [ ] 16. Template Registries
  - [ ] 16.1 Implement Template_Registry (source code templates)
    - Implement CRUD for code templates with versioning
    - Implement semantic matching for template suggestion (threshold ≥ 0.8)
    - Support template types: project scaffold, feature module, architecture pattern
    - Implement scoping: global, organization, team-private
    - Enforce template quality: linting, security scan, unit tests for core components
    - _Requirements: R19-C1, R19-C2, R19-C3, R19-C5, R19-C6, R19-C8, R19-C9_

  - [ ] 16.2 Implement Doc_Template_Registry (document and config templates)
    - Implement CRUD for document templates (requirements, design, task, skills, hooks, steering)
    - Implement template inheritance (child extends parent, override/add)
    - Implement schema validation for skills/hooks/steering templates
    - Implement versioning with diff capability
    - _Requirements: R20-C1, R20-C2, R20-C4, R20-C5, R20-C6, R20-C7_

  - [ ]* 16.3 Write unit tests for Template Registries
    - Test template versioning and diff
    - Test inheritance resolution
    - Test semantic matching for code templates
    - _Requirements: R19-C3, R19-C6, R20-C6, R20-C7_

- [ ] 17. Checkpoint - Ensure knowledge, FAQ, brain, and template tests pass
  - Ensure all tests pass, ask the user if questions arise.

- [ ] 18. Authorization and RBAC
  - [ ] 18.1 Implement IAuthService
    - Implement authenticate, authorize, assignRole, delegate methods
    - Implement 5 roles: Viewer, Developer, Tech Lead, Admin, Super Admin
    - Implement hierarchical scope: Organization → Team → Project
    - Implement data isolation between teams (cross-team requires Super Admin grant)
    - Implement delegation with max 30 days auto-revocation
    - Implement lockout: 5 failed auth in 5 min → lock 15 min
    - _Requirements: R33-C1, R33-C2, R33-C5, R34-C1, R34-C2, R34-C3, R34-C5, R34-C6_

  - [ ] 18.2 Implement user lifecycle management
    - Implement user disable/delete with full session/credential/delegation revocation within 60s
    - Implement approval reassignment when reviewer is disabled
    - Implement custom permission sets beyond 5 default roles
    - _Requirements: R34-C4, R34-C7, R34-C8_

  - [ ]* 18.3 Write property tests for Auth (Properties 15-16)
    - **Property 15: Authentication Lockout Rule**
    - **Property 16: Hierarchical RBAC Authorization**
    - **Validates: Requirements R33-C5, R34-C2, R34-C3, R34-C6**

  - [ ]* 18.4 Write unit tests for Authorization
    - Test role hierarchy permissions
    - Test delegation expiry auto-revocation
    - Test cross-team data isolation
    - _Requirements: R34-C2, R34-C5, R34-C6_

- [ ] 19. Quota and Rate Limiting
  - [ ] 19.1 Implement IQuotaService
    - Implement checkQuota, consumeQuota, getUsage, configureQuota
    - Implement per-agent/team/project quotas with configurable cycles (daily/weekly/monthly)
    - Implement 80% warning alert and 100% hard block (running pipelines complete)
    - Implement burst allowance (default 20%, max 1 hour)
    - Implement per-model rate limiting (requests/minute)
    - _Requirements: R35-C1, R35-C2, R35-C3, R35-C4, R35-C5_

  - [ ]* 19.2 Write property test for Quota (Property 17)
    - **Property 17: Quota Threshold Alerts and Blocking**
    - **Validates: Requirements R35-C2, R35-C3**

  - [ ]* 19.3 Write unit tests for Quota Service
    - Test burst allowance timing and limit
    - Test running pipeline completion despite quota exceeded
    - Test real-time usage report accuracy
    - _Requirements: R35-C3, R35-C5, R35-C6_

- [ ] 20. Notification and Communication Service
  - [ ] 20.1 Implement INotificationService
    - Implement send, configureChannel, getMetrics, delegate methods
    - Support channels: Slack, Teams, Email, in-app
    - Implement reminder escalation strategy: immediate → 24h remind → 48h escalate → 72h pause task
    - Implement delegation for notification recipients
    - _Requirements: R51-C1, R51-C2, R51-C3_

  - [ ]* 20.2 Write property test for Communication Reminder Escalation (Property 23)
    - **Property 23: Communication Reminder Escalation Timeline**
    - **Validates: Requirements R51-C3**

  - [ ]* 20.3 Write unit tests for Notification Service
    - Test channel configuration and routing
    - Test delegation forwarding
    - Test SLA-based priority handling
    - _Requirements: R51-C1, R51-C2, R51-C3_

- [ ] 21. Sentiment Analysis Service and Agent
  - [ ] 21.1 Implement ISentimentService and Sentiment_Agent
    - Implement analyze method: classify sentiment (positive/neutral/frustrated/angry) with confidence
    - Ensure non-blocking parallel execution (< 100ms added latency)
    - Implement escalation trigger: 2 consecutive negative with confidence ≥ 0.7
    - Implement SKU ticket creation on escalation (with retry queue if SKU unavailable)
    - Implement manual escalation support
    - _Requirements: R11-C1, R11-C2, R11-C3, R11-C4, R11-C5, R11-C6, R11-C7_

  - [ ] 21.2 Implement Sentiment dashboard and configuration
    - Store sentiment results with 90-day retention
    - Implement configurable escalation thresholds per agent/system
    - Implement enable/disable per agent
    - Implement dashboard: distribution, escalation rate, correlation analysis
    - _Requirements: R11-C8, R11-C9, R11-C10_

  - [ ]* 21.3 Write property test for Sentiment Escalation (Property 9)
    - **Property 9: Sentiment Escalation Trigger Logic**
    - **Validates: Requirements R11-C3**

  - [ ]* 21.4 Write unit tests for Sentiment Service
    - Test non-blocking execution (latency constraint)
    - Test manual escalation creates correct SKU issue
    - Test enable/disable toggle behavior
    - _Requirements: R11-C2, R11-C7, R11-C10_

- [ ] 22. Checkpoint - Ensure auth, quota, notification, and sentiment tests pass
  - Ensure all tests pass, ask the user if questions arise.

- [ ] 23. Specialized Agents — Requirements, Design, UIUX
  - [ ] 23.1 Implement Requirements_Agent
    - Implement structured requirements creation from raw input (< 60s)
    - Implement classification: functional, non-functional, constraint
    - Implement contradiction detection and flagging
    - Implement acceptance criteria generation with actor/action/outcome
    - Implement clarification question generation when input is insufficient
    - Implement artifact versioning (increment on save/update)
    - _Requirements: R2-C1, R2-C2, R2-C3, R2-C4, R2-C5, R2-C6, R2-C7_

  - [ ] 23.2 Implement Design_Agent
    - Implement design document generation from approved requirements (< 120s)
    - Implement coverage validation: all functional requirements mapped to design elements
    - Implement change detection and annotation ("CHANGED") on requirement version changes
    - Reject processing if requirements not approved
    - _Requirements: R3-C1, R3-C2, R3-C3, R3-C4, R3-C5_

  - [ ] 23.3 Implement UIUX_Agent
    - Implement HTML prototype generation from approved design
    - Generate responsive layouts using configurable CSS framework (Bootstrap default)
    - Generate navigation links between screens for user flow testing
    - Generate interaction spec Markdown per screen
    - Organize output per standard directory structure
    - **GUI Reference:** `gui/` serves as the exemplary output format for UIUX_Agent — the agent should produce HTML prototypes with similar structure, navigation patterns, and Bootstrap usage
    - _Requirements: R4-C1, R4-C2, R4-C3, R4-C4, R4-C5, R4-C6, R4-C8, R4-C9_

  - [ ]* 23.4 Write unit tests for Requirements, Design, UIUX Agents
    - Test requirements classification accuracy
    - Test contradiction detection
    - Test design coverage validation
    - Test prototype structure compliance
    - _Requirements: R2-C2, R2-C4, R3-C2, R4-C8_

- [ ] 24. Specialized Agents — Task, Coding, Testing
  - [ ] 24.1 Implement Task_Agent
    - Implement design-to-task decomposition with: title, description, AC, effort, priority, dependencies, assignee role
    - Ensure all design components covered by at least one task
    - Implement dependency graph ordering and parallel task identification
    - Implement grouping by feature/module with labels
    - Implement auto-split for XL tasks into sub-tasks
    - Implement task update on design version change
    - _Requirements: R5-C1, R5-C2, R5-C3, R5-C4, R5-C5, R5-C6, R5-C7_

  - [ ] 24.2 Implement Coding_Agent
    - Implement code generation from approved design per configured language
    - Implement unit test co-generation (≥ 80% coverage target)
    - Implement linting check + auto-fix loop
    - Implement security scan: no hardcoded credentials
    - Implement template usage via Template_Registry integration
    - _Requirements: R6-C1, R6-C2, R6-C3, R6-C4, R6-C5, R6-C6, R19-C3, R19-C4_

  - [ ] 24.3 Implement Testing_Agent
    - Implement test suite generation: unit, integration, acceptance tests
    - Implement test execution and report generation
    - Implement severity classification: critical, major, minor
    - Implement deployment gate: block if pass rate < 90%
    - Implement round-trip testing for parser/serializer components
    - _Requirements: R7-C1, R7-C2, R7-C3, R7-C4, R7-C5, R7-C6_

  - [ ]* 24.4 Write unit tests for Task, Coding, Testing Agents
    - Test task dependency graph generation correctness
    - Test code credential scanning detection
    - Test test execution report format
    - _Requirements: R5-C3, R6-C6, R7-C2_

- [ ] 25. Specialized Agents — Deployment, Operations, ProjectHealth
  - [ ] 25.1 Implement Deployment_Agent
    - Implement CI/CD pipeline trigger by environment (dev/staging/production)
    - Implement health check loop: every 10s for max 300s
    - Implement rollback on health check failure
    - Implement production deployment approval gate (60 min timeout)
    - Implement deployment logging: start, end, status per step, error messages
    - _Requirements: R8-C1, R8-C2, R8-C3, R8-C4, R8-C5, R8-C6_

  - [ ] 25.2 Implement Operations_Agent
    - Implement metrics collection: CPU, memory, latency, error rate (≤ 60s cycle)
    - Implement warning alerts on threshold breach (within 30s)
    - Implement incident detection (3 consecutive health check failures or error rate > 5% in 5 min)
    - Implement root cause analysis from logs (60-min window)
    - Implement 30-day metrics retention with archive policy
    - _Requirements: R9-C1, R9-C2, R9-C3, R9-C4, R9-C5_

  - [ ] 25.3 Implement ProjectHealth_Agent
    - Implement data collection from all system sources
    - Implement daily/weekly/sprint report generation
    - Implement health scores: progress, quality, risk, velocity, cost efficiency
    - Implement risk threshold alerts (> 7/10 → auto-alert)
    - Implement early warning detection (velocity drop, test pass drop, blocked tasks)
    - Implement natural language query interface
    - _Requirements: R10-C1, R10-C2, R10-C3, R10-C4, R10-C5, R10-C6, R10-C7, R10-C8_

  - [ ]* 25.4 Write unit tests for Deployment, Operations, ProjectHealth Agents
    - Test health check loop and rollback trigger
    - Test critical failure handling (rollback also fails)
    - Test risk score calculation accuracy
    - Test early warning threshold detection
    - _Requirements: R8-C2, R8-C6, R10-C4, R10-C5_

- [ ] 26. Checkpoint - Ensure all specialized agent tests pass
  - Ensure all tests pass, ask the user if questions arise.

- [ ] 27. Management Systems — RADIO, Change Request, Q&A
  - [ ] 27.1 Implement IRADIOGenerator service
    - Implement generate method with 5-component format (R, A, D, I, O)
    - Implement trigger-based generation: task_complete, test_fail, new_blocker, end_of_day, sprint_end, wave_complete
    - Store reports in Git at `docs/specs/{feature-name}/radio.md`
    - Implement severity-based alerting (D severity ≥ High → notify Tech Lead + PM)
    - Implement history retrieval and timeline comparison
    - _Requirements: R42-C1, R42-C2, R42-C3, R42-C4, R42-C5, R42-C6_

  - [ ] 27.2 Implement ICRManager (Change Request Management)
    - Implement lifecycle: Pending → Approved → In Progress → Review → Done | Rejected
    - Implement CR backlog at `docs/change-request/backlog.md`
    - Auto-create CR directory with requirements.md/design.md/tasks.md on approval
    - Implement small CR shortcut (effort < 1 day: skip design)
    - Implement impact analysis per CR
    - Implement dashboard: open/in-progress/done trend, throughput, cycle time
    - _Requirements: R44-C1, R44-C2, R44-C3, R44-C4, R44-C5, R44-C6, R44-C7, R44-C8_

  - [ ] 27.3 Implement Q&A Management system
    - Implement QA thread lifecycle: Open → In Discussion → Answered → Closed
    - Implement auto-creation of QA threads when agents detect ambiguity
    - Support multi-round discussion with follow-ups
    - Implement conclusion + action items workflow
    - Auto-update spec/design on QA conclusion approval
    - Block spec progression if open QA threads exist
    - Implement deadline tracking with RADIO "D" flagging on overdue
    - _Requirements: R45-C1, R45-C2, R45-C3, R45-C4, R45-C5, R45-C6, R45-C7, R45-C8_

  - [ ]* 27.4 Write unit tests for RADIO, CR, Q&A
    - Test RADIO report generation from actual data
    - Test CR lifecycle transitions
    - Test Q&A spec blocking enforcement
    - _Requirements: R42-C3, R44-C1, R45-C8_

- [ ] 28. Management Systems — Deliverables, DoD, Spec Versioning, Bugfix
  - [ ] 28.1 Implement Deliverables & Limitations Management
    - Auto-consolidate deliverables from execution data at sprint/phase end
    - Classify: feature (fully done), partial, documentation
    - Auto-detect limitations from incomplete tasks, failing tests, known issues
    - Implement handling plan for each limitation
    - Maintain baseline.md for cross-sprint comparison
    - Implement bidirectional links (limitation ↔ spec/CR/task)
    - _Requirements: R46-C1, R46-C2, R46-C3, R46-C4, R46-C5, R46-C6, R46-C7_

  - [ ] 28.2 Implement IDoDEnforcer service
    - Implement check method for artifact types: spec, design, tasks, feature, bugfix
    - Implement blocking on failed criteria with detailed failure list
    - Implement override mechanism (Tech Lead only, with audit log)
    - Implement configurable DoD criteria per-project
    - _Requirements: R48-C1, R48-C2, R48-C3, R48-C4, R48-C5, R48-C6_

  - [ ] 28.3 Implement ISpecVersioning service
    - Implement detectChangeType: Major (scope/logic), Minor (detail addition), Patch (typo/format)
    - Implement bumpVersion with auto-detection from diff
    - Implement requiresApproval logic (Major/Minor → yes, Patch → auto-approve)
    - Implement flagDownstream for affected artifact identification
    - _Requirements: R52-C1, R52-C2, R52-C3_

  - [ ] 28.4 Implement Bugfix Workflow
    - Implement bugfix document format: Current Behavior, Expected Behavior, Unchanged Behavior
    - Implement 5WHY analysis (3-5 iterations to root cause)
    - Implement prevention plan requirement
    - Implement regression test generation covering "Unchanged Behavior"
    - Implement bugfix DoD enforcement
    - _Requirements: R49-C1, R49-C2, R49-C3, R49-C4, R49-C5, R49-C6_

  - [ ]* 28.5 Write property tests for DoD and Spec Versioning (Properties 21, 24)
    - **Property 21: Definition of Done Enforcement**
    - **Property 24: Spec Versioning — Change Type Determines Approval Requirement**
    - **Validates: Requirements R48-C2, R48-C3, R52-C1, R52-C2, R52-C3**

  - [ ]* 28.6 Write unit tests for Deliverables, DoD, Spec Versioning, Bugfix
    - Test DoD override audit trail
    - Test deliverables auto-consolidation accuracy
    - Test spec version bump auto-detection
    - Test 5WHY analysis depth enforcement
    - _Requirements: R46-C1, R48-C6, R52-C1, R49-C2_

- [ ] 29. Checkpoint - Ensure all management system tests pass
  - Ensure all tests pass, ask the user if questions arise.

- [ ] 30. Observability Stack
  - [ ] 30.1 Implement IPromptLogger (AI Interaction Logging)
    - Implement append-only immutable logging
    - Log: agent_id, pipeline_id, timestamp, prompt, response, model_id, deployment_mode, params, metrics
    - Implement auto-redact for PII/credentials before storage
    - Implement configurable verbosity: full, summary, metadata_only
    - Implement 90-day minimum retention
    - Implement query API: by agent_id, pipeline_id, time range, model, keyword
    - _Requirements: R24-C1, R24-C2, R24-C3, R24-C4, R24-C5, R24-C6, R24-C7, R24-C8_

  - [ ] 30.2 Implement Structured Logging service
    - Implement JSON structured logs: timestamp, agent_id, action, pipeline_id, status, execution time
    - Implement distributed tracing with unique trace_id per request
    - Implement correlation reports linking logs + traces + metrics on error
    - _Requirements: R36-C1, R36-C2, R36-C5_

  - [ ] 30.3 Implement Metrics and Monitoring
    - Expose Prometheus-compatible metrics endpoint
    - Implement real-time dashboard (≤ 10s delay)
    - Track: pipeline throughput, agent execution times, error rates, token usage, costs
    - _Requirements: R36-C3, R36-C4_

  - [ ] 30.4 Implement Audit Log service
    - Implement immutable append-only audit log
    - Record: actor, action, target, timestamp, result, details
    - Implement 90-day minimum retention
    - Implement query interface for compliance review
    - _Requirements: R33-C3, R33-C4_

  - [ ]* 30.5 Write unit tests for Observability Stack
    - Test prompt log immutability (no update/delete operations)
    - Test PII auto-redaction accuracy
    - Test trace_id propagation across services
    - Test audit log completeness for state-changing operations
    - _Requirements: R24-C5, R24-C7, R36-C2, R33-C3_

- [ ] 31. Feedback, Evaluation, and SKU Tickets
  - [ ] 31.1 Implement Feedback & Rating Service
    - Implement thumbs up/down + comment (max 1000 chars) per artifact
    - Implement multi-dimensional rating (1-5 stars, per criteria: accuracy, speed, relevance, completeness)
    - Implement auto-classification: bug_report, quality_issue, feature_request, praise, general
    - Implement alert on 30% negative rate (7 days, min 10 feedback)
    - Implement SKU ticket creation for ≤ 2 star / bug_report
    - _Requirements: R25-C1, R25-C2, R25-C3, R25-C4, R26-C1, R26-C2, R26-C3, R26-C4_

  - [ ] 31.2 Implement Eval_Store and AI Quality Evaluation
    - Implement evaluation test suite CRUD (input, ground truth, scoring criteria)
    - Support 3 methods: Automated Metrics, LLM-as-Judge, Human Evaluation
    - Implement scheduled evaluation (manual, daily/weekly, on-change)
    - Implement quality regression detection (drop > 10% → alert)
    - Implement A/B evaluation with statistical significance (p < 0.05)
    - Implement Human Evaluation workflow: auto-sampling → expert review → update scores
    - _Requirements: R23-C1, R23-C2, R23-C3, R23-C4, R23-C5, R23-C6, R23-C7, R23-C9_

  - [ ] 31.3 Implement SKU Ticket Management
    - Implement ticket lifecycle management
    - Implement SLA configuration per priority (first response + resolution)
    - Implement auto-alert on critical ticket unassigned > 30 min
    - _Requirements: R27-C1, R27-C2, R27-C3, R27-C7, R27-C8_

  - [ ]* 31.4 Write unit tests for Feedback, Eval, and SKU
    - Test feedback auto-classification accuracy
    - Test evaluation scoring and regression detection
    - Test SLA breach detection and alerting
    - _Requirements: R26-C3, R23-C6, R27-C8_

- [ ] 32. Default Hooks, Steering, and Skills
  - [ ] 32.1 Implement default hooks system
    - Implement 7 default hooks: lint-on-code-save, validate-output-before-save, run-tests-after-task, security-scan-on-code, notify-on-pipeline-complete, sentiment-check, template-match-on-new-project
    - Implement hook enable/disable/customize per team/project
    - Implement reset-to-defaults functionality
    - _Requirements: R41-C1, R41-C4, R41-C5_

  - [ ] 32.2 Implement default steering files and skills
    - Create 8 default steering files: coding-standards, security-guidelines, architecture-principles, documentation-standards, git-workflow, testing-strategy, api-design-guidelines, deployment-checklist
    - Register 10 default skills: database-migration, api-documentation, changelog-generator, dependency-audit, code-review, performance-analysis, incident-postmortem, task-estimation, translation, diagram-generator
    - Implement opt-in mechanism for new defaults on system update
    - _Requirements: R41-C2, R41-C3, R41-C6_

  - [ ]* 32.3 Write unit tests for Hooks/Steering/Skills
    - Test hook event triggering
    - Test per-team override isolation
    - Test reset functionality preserves custom items
    - _Requirements: R41-C4, R41-C5_

- [ ] 33. Plugin Agent Architecture
  - [ ] 33.1 Implement plugin mechanism for custom agents
    - Implement plugin registration with custom role support
    - Implement plugin lifecycle (install, configure, activate, deactivate)
    - Ensure plugins integrate with existing pipeline and orchestrator
    - Ensure new agent roles can be added without modifying core
    - _Requirements: R1-C1 (plugin extension), R32_

  - [ ]* 33.2 Write unit tests for Plugin Architecture
    - Test plugin registration and discovery
    - Test plugin isolation from core
    - _Requirements: R32_

- [ ] 34. Checkpoint - Ensure feedback, eval, hooks, and plugin tests pass
  - Ensure all tests pass, ask the user if questions arise.

- [ ] 35. API Gateway and Admin Portal
  - [ ] 35.1 Implement REST/gRPC API Gateway
    - Implement OpenAPI/Protobuf documented endpoints for all services
    - Implement authentication middleware (token-based)
    - Implement rate limiting middleware
    - Implement webhook service for pipeline event notifications
    - Ensure 95th percentile response time < 1000ms
    - _Requirements: R37-C1, R37-C3, R28-C5_

  - [ ] 35.2 Implement Admin Portal — Agent and Pipeline Management
    - Implement Agent Management UI: CRUD, status monitoring, config editing
    - Implement Pipeline Management UI: create, monitor, pause, resume, cancel
    - Implement system settings: default temperature, FAQ confidence, Knowledge top-K, retention
    - **GUI Reference:** `gui/screens/agents.html`, `gui/screens/pipeline.html`, `gui/screens/admin.html`
    - _Requirements: R38-C1, R38-C2_

  - [ ] 35.3 Implement Admin Portal — AI Model and Evaluation Management
    - Implement AI Model Management: register/remove models, rate limits, usage stats
    - Implement Evaluation Management: CRUD test suites, schedules, thresholds, Human Eval assignments
    - Implement Quality Dashboard: trend, agent comparison, heat map
    - **GUI Reference:** `gui/screens/admin.html` (AI Model Usage section, Quota section)
    - _Requirements: R38-C3, R23-C8, R23-C12_

  - [ ] 35.4 Implement Admin Portal — Templates, Users, Health, Logs
    - Implement Template Management (Code + Doc): CRUD, preview, stats, import/export
    - Implement User & Permissions Management: roles, scopes, delegation
    - Implement System Health view: component status, uptime, resource usage
    - Implement Logs & Audit viewer: query prompt logs, audit logs, correlation reports
    - Implement Backup & Recovery: view history, manual backup, point-in-time restore
    - **GUI Reference:** `gui/screens/users.html`, `gui/screens/admin.html` (System Health, Settings)
    - _Requirements: R19-C7, R20-C8, R34-C8, R38-C4, R38-C5, R38-C7, R38-C8_

  - [ ] 35.5 Implement Admin Portal — SKU Tickets and User Feedback
    - Implement SKU Ticket Management: list, filter, assign, status, notes, response
    - Implement User Feedback Management: kanban, filters, bulk actions, link to tickets
    - Implement SKU Dashboard: open/resolved trend, MTTR, SLA breaches
    - Implement Feedback Dashboard: rating trend, categories, top issues, feature requests
    - **GUI Reference:** `gui/screens/feedback.html`
    - _Requirements: R27-C1, R27-C2, R27-C3, R27-C4, R27-C5, R27-C6, R27-C9_

  - [ ] 35.6 Implement Admin Portal — Dashboard and SDLC Workflow Screens
    - Implement main dashboard with stats, active pipeline view, mode selector, agent status
    - Implement Requirements screen: raw input, AI analysis, structured output, approve/reject
    - Implement Design screen: architecture view, data model, API spec, traceability
    - Implement Task board: Kanban view, dependency visualization, effort/priority labels
    - Implement Testing screen: test report, coverage ring, quality gates, severity badges
    - Implement Deployment screen: environment cards, CI/CD pipeline steps, monitoring, approval gate
    - Implement Knowledge screen: semantic search, document list, stats, Project Brain tabs
    - Implement Project Health screen: scores, RADIO report, risk alerts, metrics
    - Implement Sentiment screen: distribution chart, escalation list, config, manual escalation
    - **GUI Reference:** All files in `gui/` serve as the definitive visual reference for implementation
    - _Requirements: R4-C1, R4-C2, R10-C6, R11-C9, R38-C1_

  - [ ]* 35.7 Write unit tests for API Gateway and Admin Portal
    - Test authentication middleware enforcement
    - Test rate limiting behavior
    - Test RBAC-scoped visibility in portal
    - _Requirements: R37-C1, R28-C5, R38-C7_

- [ ] 36. Performance, Scalability, and Availability
  - [ ] 36.1 Implement horizontal scaling support
    - Implement auto-load distribution within 60s of adding instances
    - Implement priority-based queue when pipelines exceed capacity
    - Implement graceful shutdown (max 120s)
    - Configure component isolation (failure in one doesn't crash others)
    - _Requirements: R29-C3, R29-C4, R30-C3, R30-C4_

  - [ ] 36.2 Implement backup, recovery, and durability
    - Implement automated backup (RPO ≤ 1 hour)
    - Implement point-in-time recovery (RTO ≤ 15 minutes)
    - Ensure zero data loss for committed artifacts
    - Implement disaster recovery procedure
    - _Requirements: R31-C1, R31-C2, R31-C3, R31-C5_

  - [ ]* 36.3 Write performance tests
    - Test 10 concurrent pipelines with ≤ 20% latency degradation
    - Test Context Store read < 500ms and write < 2000ms for < 50MB
    - Test pipeline initialization < 5s
    - Test Knowledge Store search < 200ms
    - _Requirements: R28-C1, R28-C2, R28-C3, R28-C4, R17-C1_

- [ ] 37. Security Hardening
  - [ ] 37.1 Implement security measures
    - Enforce TLS 1.2+ for all communications
    - Implement credential scanning in all agent outputs
    - Implement per-agent authentication credentials
    - Ensure data encryption at rest and in transit
    - _Requirements: R33-C1, R33-C6, R6-C6, R15-C5_

  - [ ]* 37.2 Write security tests
    - Test TLS enforcement
    - Test unauthenticated access rejection
    - Test credential detection in code output
    - Test RBAC boundary enforcement
    - _Requirements: R33-C1, R33-C6, R15-C5, R34-C3_

- [ ] 38. Integration Wiring and E2E Tests
  - [ ] 38.1 Wire all components together — full pipeline flow
    - Connect API Gateway → Orchestrator → Agents → Context Store → Git
    - Connect Agents → Model Router → AI Providers
    - Connect Orchestrator → Review Gate → Notification
    - Connect Agents → Knowledge Store / FAQ Store / Project Brain
    - Connect Orchestrator → RADIO Generator
    - Connect all services → Observability Stack (logging, tracing, metrics)
    - _Requirements: R12, R13, R14, R15, R16, R17, R18, R21, R24, R36_

  - [ ]* 38.2 Write integration tests — Agent ↔ Context Store ↔ Git
    - Test artifact write → git commit → artifact read round-trip
    - Test version-to-commit-sha mapping
    - _Requirements: R15, R16-C11_

  - [ ]* 38.3 Write integration tests — Pipeline flows
    - Test full Spec Mode pipeline: requirements → design → UIUX → tasks → code → test → deploy
    - Test Fix Mode pipeline: bug report → 5WHY → fix → regression test
    - Test Vibe Mode pipeline: prompt → code → basic validation
    - Test mode switching mid-session preserves context
    - _Requirements: R13-C2, R13-C3, R13-C4, R13-C5_

  - [ ]* 38.4 Write integration tests — Review gate and escalation flows
    - Test review approval → pipeline resume
    - Test review rejection → agent rework
    - Test 3x rejection → pipeline blocked
    - Test sentiment escalation → SKU ticket creation
    - Test quota exceeded → new requests blocked, running pipelines complete
    - _Requirements: R14, R11-C3, R11-C4, R35-C3_

  - [ ]* 38.5 Write E2E tests — Multi-agent concurrent pipelines
    - Test 10 concurrent pipelines (performance SLA)
    - Test multi-agent coordination with shared Context Store
    - Test notification delivery across all channels
    - _Requirements: R28-C1, R29-C1, R51_

- [ ] 39. Final Checkpoint - Ensure all tests pass
  - Ensure all tests pass, ask the user if questions arise.

## Notes

- Tasks marked with `*` are optional and can be skipped for faster MVP
- Each task references specific requirements for traceability
- **GUI Reference**: HTML prototype at `gui/` provides visual reference for all UI-related tasks (Section 35 primarily). Implementation should match the layout, navigation, and data display patterns shown in the prototype.
- Checkpoints ensure incremental validation
- Property tests validate universal correctness properties (25 properties from design)
- Unit tests validate specific examples and edge cases
- Integration tests validate cross-component interactions
- All code is TypeScript; PBT uses `fast-check` library
- Database: PostgreSQL; Cache/Queue: Redis + RabbitMQ; Vector DB: Qdrant; Storage: S3
- Design decisions D1-D10 inform technology choices throughout

## Task Dependency Graph

```json
{
  "waves": [
    { "id": 0, "tasks": ["1.1"] },
    { "id": 1, "tasks": ["1.2", "1.3", "1.4", "1.5"] },
    { "id": 2, "tasks": ["3.1", "5.3"] },
    { "id": 3, "tasks": ["3.2", "4.1", "5.1", "5.2"] },
    { "id": 4, "tasks": ["3.3", "3.4", "4.2", "4.3", "5.4", "5.5"] },
    { "id": 5, "tasks": ["6.1", "6.4", "10.1", "18.1"] },
    { "id": 6, "tasks": ["6.2", "6.3", "10.2", "18.2", "19.1"] },
    { "id": 7, "tasks": ["6.5", "6.6", "10.3", "18.3", "18.4", "19.2", "19.3"] },
    { "id": 8, "tasks": ["8.1", "9.1", "11.1", "20.1", "30.4"] },
    { "id": 9, "tasks": ["8.2", "9.2", "11.2", "11.3", "20.2", "20.3"] },
    { "id": 10, "tasks": ["8.3", "8.4", "9.3", "9.4", "13.1", "14.1"] },
    { "id": 11, "tasks": ["13.2", "14.2", "14.3", "15.1", "16.1", "16.2"] },
    { "id": 12, "tasks": ["15.2", "15.3", "16.3", "21.1"] },
    { "id": 13, "tasks": ["21.2", "21.3", "21.4"] },
    { "id": 14, "tasks": ["23.1", "23.2", "23.3"] },
    { "id": 15, "tasks": ["23.4", "24.1", "24.2", "24.3"] },
    { "id": 16, "tasks": ["24.4", "25.1", "25.2", "25.3"] },
    { "id": 17, "tasks": ["25.4", "27.1", "27.2", "27.3"] },
    { "id": 18, "tasks": ["27.4", "28.1", "28.2", "28.3", "28.4"] },
    { "id": 19, "tasks": ["28.5", "28.6", "30.1", "30.2", "30.3"] },
    { "id": 20, "tasks": ["30.5", "31.1", "31.2", "31.3"] },
    { "id": 21, "tasks": ["31.4", "32.1", "32.2", "33.1"] },
    { "id": 22, "tasks": ["32.3", "33.2", "35.1", "35.2"] },
    { "id": 23, "tasks": ["35.3", "35.4", "35.5", "35.6"] },
    { "id": 24, "tasks": ["35.7", "36.1", "36.2", "37.1"] },
    { "id": 25, "tasks": ["36.3", "37.2", "38.1"] },
    { "id": 26, "tasks": ["38.2", "38.3", "38.4", "38.5"] }
  ]
}
```
