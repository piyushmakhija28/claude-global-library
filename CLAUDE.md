# Claude Global Library - Project Instructions

**Project:** Claude Global Library - Reusable Skills, Agents & Patterns
**Version:** 1.0.0
**Type:** Public GitHub Repository
**Status:** 🟢 Active

---

## 📖 Project Overview

**Claude Global Library** is a public repository of reusable skills, agents, and optimization patterns for Claude Code. It provides:
- Ready-to-use skills (Docker, Kubernetes, Java, etc.)
- Specialized agents (DevOps, Spring Boot, QA, etc.)
- Design patterns and best practices
- Optimization strategies

**Repository:** https://github.com/piyushmakhija28/claude-global-library

---

## 🎯 What This Project Contains

**✅ INCLUDED (Reusable Skills/Agents ONLY):**

1. **Skills:**
   - `docker` - Docker expert skill
   - `kubernetes` - K8s deployment skill
   - `java-spring-boot-microservices` - Spring Boot development
   - `jenkins-pipeline` - CI/CD pipeline skill
   - `rdbms-core` - Database design skill
   - `nosql-core` - NoSQL database skill
   - And more...

2. **Agents:**
   - `devops-engineer` - DevOps automation agent
   - `spring-boot-microservices` - Java microservices agent
   - `qa-testing-agent` - Testing automation agent
   - `orchestrator-agent` - Multi-service coordination
   - And more...

3. **Patterns & Documentation:**
   - Design patterns (Java, Spring Boot)
   - Optimization patterns
   - Best practices guides
   - Integration examples

**❌ NOT INCLUDED (These Go to Claude Insight):**
- Core memory system files
- Policy enforcement scripts
- Monitoring dashboard
- Session management system

**❌ NEVER INCLUDED (Keep Private):**
- Personal configuration files
- Project-specific skills (surgricalswale-*, techdeveloper-*)
- Business logic for specific companies
- Proprietary code or secrets

---

## 🏗️ Project Structure

```
claude-global-library/
├── README.md                      # Public project documentation
├── CLAUDE.md                      # THIS FILE - Project-specific instructions
├── skills/                        # Reusable skills
│   ├── docker/
│   │   ├── skill.md
│   │   ├── prompts/
│   │   └── examples/
│   ├── kubernetes/
│   ├── java-spring-boot-microservices/
│   └── ...
├── agents/                        # Specialized agents
│   ├── devops-engineer/
│   │   ├── agent.md
│   │   ├── prompts/
│   │   └── tools/
│   ├── spring-boot-microservices/
│   └── ...
├── patterns/                      # Design patterns
│   ├── java/
│   ├── spring-boot/
│   └── optimization/
└── docs/                          # Documentation
    ├── skill-development.md
    ├── agent-development.md
    └── best-practices.md
```

---

## 🤖 Working with This Project

**When asked to work on Claude Global Library:**

1. **Focus:** Reusable, generic skills and agents ONLY
2. **No Personal Info:** All content must be public-friendly
3. **No Project-Specific:** No business logic for specific companies
4. **Documentation:** Each skill/agent needs complete documentation

**Skill/Agent Checklist:**

✅ **Before Adding New Skill/Agent:**
- [ ] Is it reusable across different projects?
- [ ] Is it generic (not company/project-specific)?
- [ ] Is documentation complete?
- [ ] Are examples provided?
- [ ] Is it tested?

❌ **DO NOT Add:**
- Skills named like `surgricalswale-*` or `techdeveloper-*`
- Agents with hardcoded company URLs
- Business logic specific to one company
- Anything with secrets or credentials

---

## 📋 Coding Standards

**For Skills:**
```markdown
# Skill Name

## Description
Clear, concise description

## When to Use
Specific use cases

## Requirements
Prerequisites

## Usage
/skill-name [args]

## Examples
Real examples with output

## Integration
How to integrate with other skills
```

**For Agents:**
```markdown
# Agent Name

## Description
What this agent does

## Capabilities
List of capabilities

## Tools Available
Which tools agent can use

## Invocation
How to call the agent

## Examples
Usage examples
```

---

## 🚀 Development Workflow

**Adding New Skill:**
1. Create skill in `~/.claude/skills/skill-name/` (local)
2. Test thoroughly
3. Ensure it's generic and reusable
4. Copy to `claude-global-library/skills/`
5. Add documentation
6. Commit and push

**Adding New Agent:**
1. Create agent in `~/.claude/agents/agent-name/` (local)
2. Test thoroughly
3. Ensure it's generic and reusable
4. Copy to `claude-global-library/agents/`
5. Add documentation
6. Commit and push

**Syncing:**
```bash
# From personal system to public library
cd ~/.claude/skills/docker
# Test and verify it's generic
cp -r . "C:\Users\techd\Documents\workspace-spring-tool-suite-4-4.27.0-new\claude-global-library\skills\docker\"
```

---

## 🔍 Detection Rules

**A skill/agent is eligible for Claude Global Library if:**
- ✅ Name is generic (docker, kubernetes, java)
- ✅ Content is reusable across projects
- ✅ No company-specific URLs or paths
- ✅ No hardcoded secrets or credentials
- ✅ Complete documentation provided

**Block if:**
- ❌ Name contains: surgricalswale, techdeveloper, piyush
- ❌ Content has project-specific business logic
- ❌ Contains hardcoded private URLs
- ❌ Missing documentation

---

## 🔗 Related Projects

- **Claude Insight:** https://github.com/piyushmakhija28/claude-insight
  - Monitoring dashboard and core memory system
  - Separate public repository

---

## 🛠️ For Contributors

**If you're using Claude Code to work on this project:**

1. This CLAUDE.md provides project-specific context
2. Focus on creating generic, reusable content
3. Follow naming conventions
4. Document everything thoroughly
5. Test before publishing

**Quality Standards:**
- Code must be production-ready
- Documentation must be complete
- Examples must be tested
- No placeholders or TODOs in main branch

**Questions?**
- GitHub Issues: https://github.com/piyushmakhija28/claude-global-library/issues
- Documentation: See README.md

---

**VERSION:** 1.0.0 (Project-Specific)
**LAST UPDATED:** 2026-02-17
**TYPE:** Public Project Instructions
**LOCATION:** `claude-global-library/CLAUDE.md`
