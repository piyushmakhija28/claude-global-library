# Skills Index

Comprehensive directory of all available skills organized by domain. Each skill contains specialized knowledge for specific task types.

## Structure

Skills are organized by domain:
- **backend/** - Backend frameworks, databases, patterns (5 skills)
- **frontend/** - CSS, animations, SEO (3 skills)
- **devops/** - Docker, Kubernetes, Jenkins (3 skills)
- **desktop/** - JavaFX and desktop applications (1 skill)
- **system/** - System scripting and automation (3 skills)
- **mobile/** - Mobile app development (1 skill)
- **meta/** - Meta-skills for system integration (7 skills)

## Backend Skills

| Skill | Purpose |
|-------|---------|
| **java-spring-boot-microservices** | Spring Boot REST APIs, microservices architecture, business logic |
| **java-design-patterns-core** | Design patterns in Java systems (singleton, factory, observer, etc.) |
| **spring-boot-design-patterns-core** | Design patterns specific to Spring Boot applications |
| **rdbms-core** | SQL design, PostgreSQL, MySQL, query optimization, indexing |
| **nosql-core** | MongoDB, Elasticsearch, document stores, schema design |
| **payment-integration** | Payment processing, Stripe, PayPal, transactions |
| **json-core** | JSON schema, validation, transformation |
| **swift-backend-core** | Swift backend systems, REST APIs (new) |

## Frontend Skills

| Skill | Purpose |
|-------|---------|
| **css-core** | CSS layout, responsive design, modern systems, frameworks |
| **animations-core** | CSS/JS animations, 2D/3D, performance, libraries |
| **seo-keyword-research-core** | Keyword research, SEO optimization, content strategy |

## DevOps Skills

| Skill | Purpose |
|-------|---------|
| **docker** | Dockerfiles, optimization, security, multi-stage builds |
| **kubernetes** | K8s deployment, scaling, security, helm charts |
| **jenkins-pipeline** | Jenkins pipelines, CI/CD, Docker/K8s integration |

## Desktop Skills

| Skill | Purpose |
|-------|---------|
| **javafx-ide-designer** | JavaFX UI design, components, layout management |

## System Skills

| Skill | Purpose |
|-------|---------|
| **python-system-scripting** | System automation, hooks, daemon scripts, Windows-safe code |
| **linux-shell-scripting** | Bash/sh scripting, file processing, process management, cron, networking |
| **powershell-scripting** | PowerShell automation, cmdlets, registry, services, remote management |

## Mobile Skills

| Skill | Purpose |
|-------|---------|
| **android-ui-designer** | Android XML layouts, Material Design, components |
| **android-backend-engineer** | Android API integration, Kotlin, business logic |
| **swiftui-designer** | iOS SwiftUI layouts, components, theming |
| **swift-backend-engineer** | iOS backend, REST APIs, data persistence |

## Meta Skills (System Integration)

| Skill | Purpose |
|-------|---------|
| **adaptive-skill-intelligence** | Auto-detects required skills, creates/manages lifecycle |
| **context-management-core** | Context identification, validation, window optimization |
| **model-selection-core** | Selects appropriate Claude model (Haiku/Sonnet/Opus) |
| **phased-execution-intelligence** | Breaks complex tasks into execution phases |
| **task-planning-intelligence** | Plans implementation steps, identifies dependencies |
| **memory-enforcer** | Manages long-term memory across sessions |
| **migration** | Handles code migration between frameworks/versions |

## Total Count

- **23 skills** across 7 categories
- **13 agents** for autonomous task execution
- **All standardized** with lowercase `skill.md` naming

## Usage

### Via Skill Tool

```python
# Load a skill
docker = ImportManager.get_skill('docker')
kubernetes = ImportManager.get_skill('kubernetes')
```

### Direct URLs

```
Skills:  https://raw.githubusercontent.com/piyushmakhija28/claude-global-library/main/skills/{domain}/{name}/skill.md
Agents:  https://raw.githubusercontent.com/piyushmakhija28/claude-global-library/main/agents/{name}/agent.md
```

## Adding New Skills

1. Create directory: `skills/{domain}/{skill-name}/`
2. Add file: `skill.md` (lowercase)
3. Update this INDEX.md
4. Commit and push

**Example:**
```bash
mkdir -p skills/backend/my-new-skill
cat > skills/backend/my-new-skill/skill.md << 'EOF'
# My New Skill
...
EOF
git add . && git commit -m "Add my-new-skill" && git push
```

---

**Last Updated:** 2026-03-07
**Total Skills:** 23
**Total Agents:** 13
**Status:** Fully Organized & Standardized
