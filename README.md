# Claude Global Library

**Reusable Skills and Agents for Claude Code**

A curated collection of skills (knowledge modules) and agents (autonomous task runners) that enhance Claude Code's capabilities across backend, frontend, DevOps, desktop, and system domains.

---

## Table of Contents

- [Overview](#overview)
- [Skills](#skills)
- [Agents](#agents)
- [Project Structure](#project-structure)
- [How to Use](#how-to-use)
- [Contributing](#contributing)

---

## Overview

Claude Global Library provides **21 skills** and **12 agents** organized by domain. Skills provide knowledge, patterns, and coding guidance. Agents handle autonomous multi-step workflows.

| Component | Count | Purpose |
|-----------|-------|---------|
| Skills | 21 | Knowledge, patterns, rules, coding guidance |
| Agents | 12 | Autonomous complex task execution |

---

## Skills

### Backend (7 skills)

| Skill | Description |
|-------|-------------|
| `java-spring-boot-microservices` | Spring Boot microservices - REST APIs, JPA, security, architecture |
| `rdbms-core` | Relational databases - PostgreSQL, MySQL, schema design, SQL |
| `nosql-core` | Document databases - MongoDB, Elasticsearch, NoSQL patterns |
| `java-design-patterns-core` | GoF design patterns in Java - factory, singleton, observer |
| `spring-boot-design-patterns-core` | Spring Boot patterns - AOP, service layer, architecture |
| `payment-integration` | Payment gateways - Stripe, Razorpay, PayPal (Java/Python/TypeScript) |
| `migration` | Framework/DB migrations - Spring Boot upgrades, rollback strategies |

### Frontend (3 skills)

| Skill | Description |
|-------|-------------|
| `css-core` | CSS/SCSS - Flexbox, Grid, Bootstrap, Tailwind, responsive design |
| `animations-core` | Animations - CSS, GSAP, scroll, Angular animations |
| `seo-keyword-research-core` | SEO keywords - research, meta tags, search ranking |

### DevOps (3 skills)

| Skill | Description |
|-------|-------------|
| `docker` | Docker - Dockerfiles, multi-stage builds, Compose, security |
| `kubernetes` | Kubernetes - deployments, Helm, HPA, network policies |
| `jenkins-pipeline` | Jenkins CI/CD - declarative pipelines, Docker/K8s integration |

### Desktop (1 skill)

| Skill | Description |
|-------|-------------|
| `javafx-ide-designer` | JavaFX desktop apps - RichTextFX, dark themes, IDE patterns |

### System (1 skill)

| Skill | Description |
|-------|-------------|
| `python-system-scripting` | Python scripts for Claude memory system - hooks, sessions, Windows-safe |

### Meta (6 skills - auto-applied)

| Skill | Description |
|-------|-------------|
| `adaptive-skill-intelligence` | On-the-fly skill creation when no match exists |
| `context-management-core` | Context window optimization and smart cleanup |
| `model-selection-core` | Claude model selection (Haiku/Sonnet/Opus) |
| `phased-execution-intelligence` | Phase breakdown and parallel agent coordination |
| `task-planning-intelligence` | Task planning and complexity scoring |
| `memory-enforcer` | Memory system enforcement rules |

---

## Agents

| Agent | Domain | Use Case |
|-------|--------|----------|
| `orchestrator-agent` | Multi-domain | Coordinates multiple agents for complex tasks |
| `spring-boot-microservices` | Backend | Spring Boot service implementation |
| `devops-engineer` | DevOps | Docker, K8s, Jenkins, infrastructure |
| `qa-testing-agent` | Testing | Test strategy, regression detection, QA |
| `angular-engineer` | Frontend | Angular components, routing, state management |
| `ui-ux-designer` | Design | Web UI/UX layout, dashboards, interactions |
| `android-ui-designer` | Mobile | Android XML layouts, Material Design |
| `android-backend-engineer` | Mobile | Kotlin backend, Coroutines, REST integration |
| `swiftui-designer` | Mobile | SwiftUI views, iOS layout design |
| `swift-backend-engineer` | Mobile | Swift server-side, Vapor, REST APIs |
| `static-seo-agent` | SEO | Static site SEO, meta tags, JSON-LD |
| `dynamic-seo-agent` | SEO | Angular/React SPA SEO, route-level meta |

---

## Project Structure

```text
claude-global-library/
├── README.md
├── CLAUDE.md
├── .gitignore
│
├── skills/
│   ├── INDEX.md                              # Complete skill index
│   ├── README.md                             # Skills overview
│   │
│   ├── backend/                              # Backend skills
│   │   ├── java-spring-boot-microservices/
│   │   ├── java-design-patterns-core/
│   │   ├── spring-boot-design-patterns-core/
│   │   ├── rdbms-core/
│   │   ├── nosql-core/
│   │   └── payment-integration/
│   │
│   ├── frontend/                             # Frontend skills
│   │   ├── animations-core/
│   │   ├── css-core/
│   │   └── seo-keyword-research-core/
│   │
│   ├── devops/                               # DevOps skills
│   │   ├── docker/
│   │   ├── kubernetes/
│   │   └── jenkins-pipeline/
│   │
│   ├── desktop/                              # Desktop skills
│   │   └── javafx-ide-designer/
│   │
│   ├── system/                               # System skills
│   │   └── python-system-scripting/
│   │
│   ├── adaptive-skill-intelligence/          # Meta skills
│   ├── context-management-core/
│   ├── model-selection-core/
│   ├── phased-execution-intelligence/
│   ├── task-planning-intelligence/
│   ├── memory-enforcer/
│   └── migration/
│
└── agents/
    ├── README.md                             # Agents overview
    ├── orchestrator-agent/
    ├── spring-boot-microservices/
    ├── devops-engineer/
    ├── qa-testing-agent/
    ├── angular-engineer/
    ├── ui-ux-designer/
    ├── android-ui-designer/
    ├── android-backend-engineer/
    ├── swiftui-designer/
    ├── swift-backend-engineer/
    ├── static-seo-agent/
    └── dynamic-seo-agent/
```

---

## How to Use

### Skills
Skills are knowledge modules loaded via Claude Code's Skill tool. Each skill contains patterns, rules, and coding guidance for its domain.

```
# In Claude Code, skills are invoked automatically based on task detection
# Or manually via: Skill tool with skill="skill-name"
```

### Agents
Agents are autonomous workers launched via Claude Code's Task tool for complex multi-step workflows.

```
# Agents are launched via: Task(subagent_type="agent-name")
# Example: Task(subagent_type="devops-engineer", prompt="Create K8s deployment")
```

### Installation
Copy skills and agents to your Claude configuration:

```bash
# Copy all skills
cp -r skills/* ~/.claude/skills/

# Copy all agents
cp -r agents/* ~/.claude/agents/
```

---

## Contributing

1. Skills go in `skills/<category>/<skill-name>/`
2. Agents go in `agents/<agent-name>/`
3. Each skill must have a `skill.md` or `SKILL.md`
4. Each agent must have a `<agent-name>.md`
5. No project-specific content (business logic, credentials, internal paths)

---

**Version:** 2.0.0
**Last Updated:** 2026-02-23
**Repository:** https://github.com/piyushmakhija28/claude-global-library
