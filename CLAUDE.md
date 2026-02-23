# claude-global-library - Claude Code Instructions

**Project:** Claude Global Library
**Type:** Skills and Agents Repository
**Version:** 2.0.0
**Status:** Active Development

---

## PROJECT OVERVIEW

Curated collection of reusable skills (knowledge modules) and agents (autonomous task runners) for Claude Code. Organized by domain: backend, frontend, DevOps, desktop, system, and meta.

**Important:** This file provides **ADDITIONAL** project-specific context. It does **NOT** override global CLAUDE.md policies.

---

## PROJECT STRUCTURE

```text
claude-global-library/
├── README.md
├── CLAUDE.md
├── .gitignore
├── skills/
│   ├── INDEX.md
│   ├── README.md
│   ├── backend/                    # 7 skills (Spring Boot, DB, patterns, payments)
│   │   ├── java-spring-boot-microservices/
│   │   ├── java-design-patterns-core/
│   │   ├── spring-boot-design-patterns-core/
│   │   ├── rdbms-core/
│   │   ├── nosql-core/
│   │   └── payment-integration/
│   ├── frontend/                   # 3 skills (CSS, animations, SEO)
│   │   ├── animations-core/
│   │   ├── css-core/
│   │   └── seo-keyword-research-core/
│   ├── devops/                     # 3 skills (Docker, K8s, Jenkins)
│   │   ├── docker/
│   │   ├── kubernetes/
│   │   └── jenkins-pipeline/
│   ├── desktop/                    # 1 skill (JavaFX)
│   │   └── javafx-ide-designer/
│   ├── system/                     # 1 skill (Python system scripting)
│   │   └── python-system-scripting/
│   ├── adaptive-skill-intelligence/  # Meta skills (auto-applied)
│   ├── context-management-core/
│   ├── model-selection-core/
│   ├── phased-execution-intelligence/
│   ├── task-planning-intelligence/
│   ├── memory-enforcer/
│   └── migration/
└── agents/                         # 12 agents
    ├── README.md
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

## PROJECT-SPECIFIC RULES

1. **No project-specific content** - No business logic, credentials, internal paths
2. **Organized by domain** - backend/, frontend/, devops/, desktop/, system/ for skills
3. **One folder per skill** - Each skill has its own directory with skill.md/SKILL.md
4. **One folder per agent** - Each agent has its own directory with agent-name.md
5. **No flat duplicates** - Skills ONLY in their organized folder (not duplicated at root)
6. **INDEX.md** - skills/INDEX.md must be updated when adding/removing skills
7. **README.md per category** - Each category (skills/, agents/) has its own README

---

## SYNC RULES

| Source | Destination | What |
|--------|-------------|------|
| `~/.claude/skills/` | `skills/` | All reusable skills |
| `~/.claude/agents/` | `agents/` | All 12 agents |

**Exclude from sync:**
- Project-specific skills (test-surgricalswale-order, etc.)
- Private paths, credentials, business logic
- Symlinks (resolve to actual files before copying)

---

## SUMMARY

**This CLAUDE.md provides:**
- Project-specific context for skills/agents library
- Directory organization rules
- Sync rules between local system and repo

**Global policies:** Apply automatically (NOT overridden)

---

**Project:** claude-global-library
**Version:** 2.0.0
**Last Updated:** 2026-02-23
**Status:** Active Development
