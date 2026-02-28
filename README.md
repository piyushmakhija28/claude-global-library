# Claude Global Library

**Reusable Skills and Agents for Claude Code**

[![GitHub](https://img.shields.io/badge/GitHub-claude--global--library-blue?logo=github)](https://github.com/piyushmakhija28/claude-global-library)
[![Skills](https://img.shields.io/badge/Skills-21+-brightgreen)](##skills)
[![Agents](https://img.shields.io/badge/Agents-12-blue)](##agents)
[![Version](https://img.shields.io/badge/Version-1.1.0-green)](VERSION)

A curated collection of **21 reusable skills** (knowledge modules) and **12 agents** (autonomous task runners) that enhance Claude Code's capabilities across backend, frontend, DevOps, desktop, and system domains.

---

## Table of Contents

1. [Overview](#overview)
2. [3-Tier Ecosystem Architecture](#3-tier-ecosystem-architecture)
3. [The ~/.claude/ Directory](#the-claude-directory)
4. [What Is This Library?](#what-is-this-library)
5. [Quick Reference](#quick-reference)
6. [How to Use](#how-to-use)
7. [Backend Skills (7)](#backend-skills-7)
8. [Frontend Skills (3)](#frontend-skills-3)
9. [DevOps Skills (3)](#devops-skills-3)
10. [Desktop Skills (1)](#desktop-skills-1)
11. [System Skills (1)](#system-skills-1)
12. [Meta/Intelligence Skills (6)](#metaintelligence-skills-6)
13. [All 12 Agents](#all-12-agents)
14. [Migration Skill and Expert Agent](#migration-skill-and-expert-agent)
15. [Integration with Claude Code IDE](#integration-with-claude-code-ide)
16. [Adding New Skills and Agents](#adding-new-skills-and-agents)
17. [Naming Standards](#naming-standards)
18. [Project Structure](#project-structure)
19. [Contributing](#contributing)
20. [Version History](#version-history)
21. [License](#license)

---

## Overview

Claude Global Library provides a **distributed knowledge base** and **agent system** organized by domain for use with Claude Code IDE and Claude Code CLI. Skills provide coding patterns, standards, and best practices. Agents handle autonomous multi-step task execution.

| Component | Count | Purpose |
|-----------|-------|---------|
| **Skills** | 21 | Coding patterns, knowledge, guidance, standards enforcement |
| **Agents** | 12 | Autonomous task execution for complex, multi-step workflows |

---

## 3-Tier Ecosystem Architecture

Claude Global Library is **Tier 3** of a 3-tier architecture:

```
Tier 1: User Interface         (claude-code-ide)
   |  uses hooks from
Tier 2: Policy Enforcement      (claude-insight)
   |  uses skills from
Tier 3: Knowledge Base          (THIS PROJECT - claude-global-library)
```

| Tier | Repository | Purpose |
|------|-----------|---------|
| **Tier 1** | [claude-code-ide](https://github.com/piyushmakhija28/claude-code-ide) | JavaFX IDE + hook-downloader that executes hooks |
| **Tier 2** | [claude-insight](https://github.com/piyushmakhija28/claude-insight) | Python hook scripts, policies, Flask monitoring dashboard |
| **Tier 3** | [claude-global-library](https://github.com/piyushmakhija28/claude-global-library) | 21 reusable skills + 12 agents for code generation |

---

## The ~/.claude/ Directory

All three tiers share a single runtime directory: `~/.claude/`. This is where Claude Code (both IDE and CLI) stores configuration, scripts, session data, and logs.

### Why ~/.claude/?

1. **Single source of truth** - One location for all Claude configuration, no confusion
2. **Tool-agnostic** - IDE and CLI share the same hooks, policies, and session data
3. **Portable** - `~/` resolves correctly on Windows, macOS, and Linux
4. **No overhead** - Files are lightweight JSON summaries, not large datasets
5. **Persistent** - Survives IDE restarts, system reboots, and tool upgrades

### How This Library Fits In

Skills and agents from this repository are loaded **on-demand** via `ImportManager` - they are fetched directly from GitHub when needed, not pre-deployed to `~/.claude/`.

```
~/.claude/
|-- CLAUDE.md                # Global config (references skill/agent names)
|-- settings.json            # Hook configuration
|
|-- scripts/                 # Hook scripts (from claude-insight)
|   +-- architecture/        # 3-Level Architecture
|
|-- memory/                  # Session data (local only)
|   |-- logs/sessions/       # Session traces
|   |-- sessions/            # Session state
|   |-- tasks/               # Task tracking
|   +-- config/              # Preferences
|
+-- policies/                # Policy definitions (from claude-insight)
```

**This library (claude-global-library)** does NOT deploy files to `~/.claude/`. Instead, `ImportManager` in claude-insight fetches skills and agents directly from this repo's GitHub when a task matches a skill.

### Data Flow

```
User sends message
    |
3-level-flow.py (in ~/.claude/scripts/) analyzes task
    |
Step 3.5: Skill/Agent Selection
    |
ImportManager.get_skill('docker')
    |
Fetches from GitHub: claude-global-library/skills/devops/docker/skill.md
    |
Skill content injected into Claude's context
    |
Claude generates response following skill patterns
```

---

## What Is This Library?

### Key Concepts

**Skills** are knowledge modules that teach Claude coding patterns and best practices:
- Spring Boot architecture and design patterns
- Database design and optimization (SQL, NoSQL)
- CSS, animations, SEO best practices
- Docker, Kubernetes, CI/CD patterns
- Desktop UI design (JavaFX)
- Python automation and hooks

**Agents** are autonomous specialists that execute complex workflows:
- DevOps Engineer: Deploys applications, manages infrastructure
- Spring Boot Microservices: Implements enterprise Java services
- QA Testing Agent: Designs and validates test strategies
- UI/UX Designer: Builds web interfaces with proper patterns
- Mobile Engineers: Android, iOS development specialists

**Meta Skills** are auto-applied on every task to enable intelligence:
- Adaptive skill selection (picks the right skill automatically)
- Context optimization (manages token budget)
- Model selection (Haiku/Sonnet/Opus)
- Task planning (breaks down complexity)

---

## Quick Reference

### All 21 Skills

| Skill | Domain | Purpose | Status |
|-------|--------|---------|--------|
| `java-spring-boot-microservices` | Backend | Spring Boot REST APIs, microservices, security | Active |
| `java-design-patterns-core` | Backend | Gang of Four patterns in Java | Active |
| `spring-boot-design-patterns-core` | Backend | Spring Boot patterns, AOP, layers | Active |
| `rdbms-core` | Backend | PostgreSQL/MySQL design, SQL optimization | Active |
| `nosql-core` | Backend | MongoDB, Elasticsearch patterns | Active |
| `payment-integration` | Backend | Stripe, Razorpay, PayPal integration | Active |
| `migration` | Backend | Framework/DB migrations with rollback | Active |
| `css-core` | Frontend | CSS/SCSS, Flexbox, Grid, responsive design | Active |
| `animations-core` | Frontend | CSS/JS animations, GSAP, scroll effects | Active |
| `seo-keyword-research-core` | Frontend | SEO research, meta tags, rankings | Active |
| `docker` | DevOps | Dockerfiles, multi-stage, Compose | Active |
| `kubernetes` | DevOps | K8s deployments, Helm, HPA | Active |
| `jenkins-pipeline` | DevOps | Jenkins declarative pipelines, CI/CD | Active |
| `javafx-ide-designer` | Desktop | JavaFX UI, RichTextFX, dark themes | Active |
| `python-system-scripting` | System | Python hooks, sessions, Windows-safe | Active |
| `adaptive-skill-intelligence` | Meta | Auto-detects and creates skills on-the-fly | Auto |
| `context-management-core` | Meta | Context optimization, caching, cleanup | Auto |
| `model-selection-core` | Meta | Claude model selection (Haiku/Sonnet/Opus) | Auto |
| `task-planning-intelligence` | Meta | Task breakdown, complexity scoring | Auto |
| `phased-execution-intelligence` | Meta | Phased execution, agent coordination | Auto |
| `memory-enforcer` | Meta | Memory system rules and enforcement | Auto |

### All 12 Agents

| Agent | Domain | Purpose | Status |
|-------|--------|---------|--------|
| `orchestrator-agent` | Multi-domain | Coordinates multiple agents | Active |
| `spring-boot-microservices` | Backend | Spring Boot service implementation | Active |
| `devops-engineer` | DevOps | Docker, K8s, Jenkins, infrastructure | Active |
| `qa-testing-agent` | Testing | Test strategy, regression, QA | Active |
| `angular-engineer` | Frontend | Angular components, routing | Active |
| `ui-ux-designer` | Design | Web UI/UX layout, dashboards | Active |
| `android-ui-designer` | Mobile | Android XML layouts, Material Design | Active |
| `android-backend-engineer` | Mobile | Kotlin backend, Coroutines, REST | Active |
| `swiftui-designer` | Mobile | SwiftUI views, iOS layout design | Active |
| `swift-backend-engineer` | Mobile | Swift server-side, Vapor, REST APIs | Active |
| `static-seo-agent` | SEO | Static site SEO, meta tags, JSON-LD | Active |
| `dynamic-seo-agent` | SEO | Angular/React SPA SEO, routes | Active |

---

## How to Use

### Step 1: Skills are loaded automatically

The 3-level architecture (claude-insight) automatically selects the right skill for your task:
- **Spring Boot task?** -> `java-spring-boot-microservices` skill loaded
- **CSS question?** -> `css-core` skill loaded
- **Kubernetes task?** -> `kubernetes` skill loaded
- **Custom framework?** -> `adaptive-skill-intelligence` creates a new skill on-the-fly

### Step 2: Agents can be invoked for complex tasks

```python
Task(
    subagent_type="devops-engineer",
    prompt="Deploy my Spring Boot app to Kubernetes"
)
```

### Step 3: ImportManager loads from GitHub

```python
from utils.import_manager import ImportManager

docker_skill = ImportManager.get_skill('docker')
devops = ImportManager.get_agent('devops-engineer')
```

### File Structure

Each skill has a standardized structure:
```
skills/
|-- backend/
|   |-- java-spring-boot-microservices/
|   |   +-- skill.md              <- The skill (standardized name)
|   +-- rdbms-core/
|       +-- skill.md
```

Each agent has a standardized structure:
```
agents/
|-- devops-engineer/
|   +-- agent.md                  <- The agent (standardized name)
+-- qa-testing-agent/
    +-- agent.md
```

---

## Backend Skills (7)

### 1. Java Spring Boot Microservices
**Location:** `skills/backend/java-spring-boot-microservices/skill.md`
**Covers:** Spring Boot 3.x, REST APIs, Spring Data JPA, Spring Security, microservices patterns, testing
**Use When:** Building Spring Boot applications and microservices

### 2. Java Design Patterns Core
**Location:** `skills/backend/java-design-patterns-core/skill.md`
**Covers:** Creational, Structural, Behavioral patterns with real-world Java examples
**Use When:** Designing architectures, solving structural problems

### 3. Spring Boot Design Patterns Core
**Location:** `skills/backend/spring-boot-design-patterns-core/skill.md`
**Covers:** DI, AOP, layered architecture, service patterns, Spring Cloud
**Use When:** Spring Boot applications, service layer design

### 4. RDBMS Core
**Location:** `skills/backend/rdbms-core/skill.md`
**Covers:** Schema design, SQL optimization, indexing, transactions, PostgreSQL/MySQL
**Use When:** Database design, query optimization, migrations

### 5. NoSQL Core
**Location:** `skills/backend/nosql-core/skill.md`
**Covers:** MongoDB document design, Elasticsearch, aggregation pipelines, sharding
**Use When:** Choosing NoSQL, document structures, search optimization

### 6. Payment Integration
**Location:** `skills/backend/payment-integration/skill.md`
**Covers:** Stripe, Razorpay, PayPal, webhooks, PCI compliance, testing
**Use When:** Payment processing, e-commerce, subscriptions

### 7. Migration
**Location:** `skills/migration/skill.md`
**Covers:** Framework/DB/API/architecture migrations, zero-downtime, rollback
**Use When:** Upgrading frameworks, migrating databases, version transitions

---

## Frontend Skills (3)

### 1. CSS Core
**Location:** `skills/frontend/css-core/skill.md`
**Covers:** Flexbox, Grid, responsive design, SCSS, Bootstrap, Tailwind, performance
**Use When:** Responsive layouts, styling, animations

### 2. Animations Core
**Location:** `skills/frontend/animations-core/skill.md`
**Covers:** CSS animations, GSAP, Anime.js, scroll animations, accessibility
**Use When:** Smooth animations, interactive experiences, scroll effects

### 3. SEO Keyword Research Core
**Location:** `skills/frontend/seo-keyword-research-core/skill.md`
**Covers:** Keyword research, meta tags, JSON-LD, ranking factors, technical SEO
**Use When:** Search optimization, keyword research, SEO strategies

---

## DevOps Skills (3)

### 1. Docker
**Location:** `skills/devops/docker/skill.md`
**Covers:** Dockerfiles, multi-stage builds, Compose, security, image optimization
**Use When:** Containerizing applications, multi-container environments

### 2. Kubernetes
**Location:** `skills/devops/kubernetes/skill.md`
**Covers:** Deployments, Services, Ingress, RBAC, Helm, HPA, monitoring
**Use When:** Container orchestration, scaling, service mesh

### 3. Jenkins Pipeline
**Location:** `skills/devops/jenkins-pipeline/skill.md`
**Covers:** Declarative/scripted pipelines, Docker/K8s integration, GitOps
**Use When:** CI/CD pipelines, build automation, continuous delivery

---

## Desktop Skills (1)

### 1. JavaFX IDE Designer
**Location:** `skills/desktop/javafx-ide-designer/skill.md`
**Covers:** JavaFX UI, RichTextFX, dark themes, layout management, custom components
**Use When:** Building desktop IDEs, code editors, JavaFX applications

---

## System Skills (1)

### 1. Python System Scripting
**Location:** `skills/system/python-system-scripting/skill.md`
**Covers:** Python hooks, session management, Windows Unicode safety, cross-platform paths
**Use When:** System automation, Python hooks, integration scripts

---

## Meta/Intelligence Skills (6)

These skills are **automatically applied** to every task. You don't invoke them explicitly.

### 1. Adaptive Skill Intelligence
Auto-detects the right skill for any task. Creates new skills on-the-fly if no match exists.

### 2. Context Management Core
Monitors context window usage. Triggers cleanup if >80%. Archives old logs.

### 3. Model Selection Core
Selects Haiku (0-8), Sonnet (9-17), or Opus (18-25) based on complexity scoring.

### 4. Task Planning Intelligence
Breaks down complex tasks into sub-tasks. Scores complexity and suggests plan mode.

### 5. Phased Execution Intelligence
Coordinates multi-agent execution. Manages phases, dependencies, and auto-commits.

### 6. Memory Enforcer
Enforces Claude Memory System rules. Validates session state and policy compliance.

---

## All 12 Agents

### Backend

| Agent | Specialization | Key Capabilities |
|-------|---------------|-----------------|
| **Spring Boot Microservices** | Enterprise Java | REST APIs, Spring Cloud, JPA, Security, Testing |
| **Android Backend Engineer** | Kotlin mobile backend | Coroutines, REST, authentication, DB design |
| **Swift Backend Engineer** | Server-side Swift | Vapor, REST APIs, DB integration, auth flows |

### Frontend/UI

| Agent | Specialization | Key Capabilities |
|-------|---------------|-----------------|
| **Angular Engineer** | Angular web apps | Components, routing, RxJS, state management |
| **UI/UX Designer** | Web interfaces | Responsive layouts, Material Design, accessibility |
| **Android UI Designer** | Android Material | XML layouts, Material Design 3, custom components |
| **SwiftUI Designer** | iOS SwiftUI | View hierarchy, navigation, iOS guidelines |

### DevOps and Quality

| Agent | Specialization | Key Capabilities |
|-------|---------------|-----------------|
| **DevOps Engineer** | Infrastructure | Docker, K8s, Jenkins, cloud, monitoring |
| **QA Testing Agent** | Quality assurance | Test strategy, unit/E2E, regression, automation |

### SEO

| Agent | Specialization | Key Capabilities |
|-------|---------------|-----------------|
| **Static SEO Agent** | Static site SEO | Meta tags, JSON-LD, sitemaps, performance |
| **Dynamic SEO Agent** | SPA SEO | Angular/React routing, SSR, prerendering |

### Orchestration

| Agent | Specialization | Key Capabilities |
|-------|---------------|-----------------|
| **Orchestrator Agent** | Multi-agent coordination | Workflow management, progress tracking, communication |

---

## Migration Skill and Expert Agent

A comprehensive system for **zero-downtime migrations** with automatic rollback.

**Handles:** Framework migrations, database migrations, API migrations, dependency upgrades, cloud migrations, architecture migrations, auth migrations.

**Phases:** Discovery (20%) -> Planning (15%) -> Backup (10%) -> Testing (30%) -> Execution (15%) -> Verification (10%) -> Monitoring -> Cleanup (5%)

**Risk Levels:**
- LOW: Patch updates (3.2.0 -> 3.2.1)
- MEDIUM: Minor updates (3.1 -> 3.2)
- HIGH: Major updates (2.7 -> 3.2)
- CRITICAL: Architecture changes (monolith -> microservices)

---

## Integration with Claude Code IDE

### How Skills Are Used

1. **IDE sends message** -> Claude Code IDE
2. **Message analyzed** -> Task planning identifies task type
3. **Skill loaded** -> ImportManager loads matching skill from this repo
4. **Claude enhanced** -> Skill patterns injected into prompt
5. **Better response** -> Claude generates code following skill patterns

### How Agents Are Invoked

```python
Task(
    subagent_type="devops-engineer",
    prompt="Deploy my Spring Boot app to Kubernetes"
)
```

---

## Adding New Skills and Agents

### Creating a New Skill

```bash
# 1. Create directory
mkdir -p skills/domain/my-new-skill

# 2. Create skill.md
cat > skills/domain/my-new-skill/skill.md << 'EOF'
# My New Skill

**Purpose**: What this skill teaches

## Key Concepts
- Concept 1
- Concept 2

## Best Practices
1. Practice 1
2. Practice 2

## Examples
[Code examples]
EOF

# 3. Commit and push
git add skills/domain/my-new-skill/skill.md
git commit -m "Add my-new-skill"
git push origin main
```

### Creating a New Agent

```bash
# 1. Create directory
mkdir -p agents/my-new-agent

# 2. Create agent.md
cat > agents/my-new-agent/agent.md << 'EOF'
# My New Agent

**Specialization**: What this agent does

## Capabilities
- Capability 1
- Capability 2

## Workflow
[Step-by-step]
EOF

# 3. Commit and push
git add agents/my-new-agent/agent.md
git commit -m "Add my-new-agent"
git push origin main
```

---

## Naming Standards

### Skills
- File: `skill.md` (always this exact name)
- Location: `skills/domain/skill-name/skill.md`
- Names: lowercase with hyphens (e.g., `java-spring-boot-microservices`)

### Agents
- File: `agent.md` (always this exact name)
- Location: `agents/agent-name/agent.md`
- Names: lowercase with hyphens (e.g., `devops-engineer`)

### Why Standardization?
- ImportManager finds skills reliably
- Auto-discovery of new skills/agents
- Consistent patterns across projects
- Easy onboarding for contributors

---

## Project Structure

```
claude-global-library/
|-- README.md                         This file
|-- CLAUDE.md                         Project setup guide
|-- LICENSE                           MIT License
|-- VERSION                           Version number
|
|-- skills/                           21 reusable skills
|   |-- backend/                      Backend skills (7)
|   |   |-- java-spring-boot-microservices/skill.md
|   |   |-- java-design-patterns-core/skill.md
|   |   |-- spring-boot-design-patterns-core/skill.md
|   |   |-- rdbms-core/skill.md
|   |   |-- nosql-core/skill.md
|   |   |-- payment-integration/skill.md
|   |   +-- migration/skill.md
|   |-- frontend/                     Frontend skills (3)
|   |   |-- css-core/skill.md
|   |   |-- animations-core/skill.md
|   |   +-- seo-keyword-research-core/skill.md
|   |-- devops/                       DevOps skills (3)
|   |   |-- docker/skill.md
|   |   |-- kubernetes/skill.md
|   |   +-- jenkins-pipeline/skill.md
|   |-- desktop/                      Desktop skills (1)
|   |   +-- javafx-ide-designer/skill.md
|   |-- system/                       System skills (1)
|   |   +-- python-system-scripting/skill.md
|   +-- meta/                         Meta skills (6 - auto-applied)
|       |-- adaptive-skill-intelligence/skill.md
|       |-- context-management-core/skill.md
|       |-- model-selection-core/skill.md
|       |-- task-planning-intelligence/skill.md
|       |-- phased-execution-intelligence/skill.md
|       +-- memory-enforcer/skill.md
|
|-- agents/                           12 reusable agents
|   |-- orchestrator-agent/agent.md
|   |-- spring-boot-microservices/agent.md
|   |-- devops-engineer/agent.md
|   |-- qa-testing-agent/agent.md
|   |-- angular-engineer/agent.md
|   |-- ui-ux-designer/agent.md
|   |-- android-ui-designer/agent.md
|   |-- android-backend-engineer/agent.md
|   |-- swiftui-designer/agent.md
|   |-- swift-backend-engineer/agent.md
|   |-- static-seo-agent/agent.md
|   +-- dynamic-seo-agent/agent.md
|
|-- agents-system/                    Custom agent extensions
|   +-- migration-expert-agent.md
|
+-- docs/                             Documentation
    |-- SKILLS-REGISTRY.md
    +-- AGENTS-REGISTRY.md
```

---

## Contributing

1. **Identify a gap** - Is there a domain or tool we're missing?
2. **Create skill/agent** - Follow naming standards above
3. **Document thoroughly** - Include examples, best practices, use cases
4. **Test** - Verify it loads via ImportManager
5. **Submit PR** - Clear description of what you added and why

**Quality over quantity** - One excellent skill is better than three mediocre ones.

**High Priority Skills Wanted:**
- React patterns
- GraphQL expertise
- Terraform / IaC
- Golang backend
- Python Django/FastAPI

---

## Version History

- **1.1.0** (2026-02-28) - README updated with ecosystem architecture, ~/.claude/ directory explanation, correct paths
- **1.0.0** (2026-02-26) - Comprehensive documentation consolidation, all skill/agent descriptions

---

## License

MIT License - see [LICENSE](LICENSE) for details.

---

**GitHub:** https://github.com/piyushmakhija28/claude-global-library
**Claude Insight:** https://github.com/piyushmakhija28/claude-insight
**Claude Code IDE:** https://github.com/piyushmakhija28/claude-code-ide

**Version:** 1.1.0 | **Skills:** 21+ | **Agents:** 12
**Last Updated:** 2026-02-28
