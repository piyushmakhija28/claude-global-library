# Skills Index

**Location:** `~/.claude/skills/`
**Last Updated:** 2026-02-18
**Total Skills:** 25+

---

## Backend Skills

**Location:** `~/.claude/skills/backend/`

| Skill | Description | Symlink |
|-------|-------------|---------|
| `java-spring-boot-microservices` | Spring Boot microservices, REST APIs, security, DB integration | Yes |
| `java-design-patterns-core` | Design patterns in Java applications | Yes |
| `spring-boot-design-patterns-core` | Spring Boot-specific design patterns | Yes |
| `rdbms-core` | PostgreSQL/MySQL schema design, queries, optimization | Yes |
| `nosql-core` | MongoDB, Elasticsearch patterns and best practices | Yes |
| `payment-integration` | **NEW** Payment gateway integration (Stripe, Razorpay, PayPal) | No |
| `json-core` | JSON handling and processing | No |
| `swift-backend-core` | Swift backend development | No |

---

## Frontend Skills

**Location:** `~/.claude/skills/frontend/`

| Skill | Description | Symlink |
|-------|-------------|---------|
| `css-core` | CSS, responsive design, modern layouts, frameworks | Yes |
| `animations-core` | CSS/JS animations, 2D/3D, performance | Yes |
| `typescript-core` | TypeScript patterns and best practices | No |
| `javascript-core` | JavaScript fundamentals and patterns | No |
| `ui-ux-core` | UI/UX design principles | No |
| `seo-keyword-research-core` | SEO keyword intelligence and research | Yes |

---

## Mobile Skills

**Location:** `~/.claude/skills/mobile/`

| Skill | Description |
|-------|-------------|
| `kotlin-core` | Kotlin Android development |
| `android-xml-ui` | Android XML UI layouts |
| `swiftui-core` | SwiftUI iOS development |

---

## DevOps Skills

**Location:** `~/.claude/skills/devops/`

| Skill | Description | Symlink |
|-------|-------------|---------|
| `docker` | Dockerfiles, Compose, optimization, security | Yes |
| `kubernetes` | K8s deployments, services, ingress, network policies | Yes |
| `jenkins-pipeline` | CI/CD pipelines, Docker/K8s integration | Yes |

---

## Desktop Skills

**Location:** `~/.claude/skills/desktop/`

| Skill | Description |
|-------|-------------|
| `javafx-ide-designer` | **NEW** JavaFX IDE interface design and implementation |

---

## System Skills

**Location:** `~/.claude/skills/system/`

| Skill | Description | Symlink |
|-------|-------------|---------|
| `python-system-scripting` | Python hooks, session mgmt, automation, enforcement (Windows-safe) | Yes |

---

## Intelligence/Meta Skills

**Location:** `~/.claude/skills/` (root)

| Skill | Description |
|-------|-------------|
| `adaptive-skill-intelligence` | Auto-detects and creates skills on-the-fly |
| `context-management-core` | Context validation, optimization, caching |
| `model-selection-core` | Haiku/Sonnet/Opus model selection rules |
| `task-planning-intelligence` | Task breakdown and planning |
| `phased-execution-intelligence` | Phased execution management |
| `memory-enforcer` | Memory system enforcement |
| `migration` | Framework/database migration expert |

---

## Symlink Quick Reference

These symlinks exist in the root `~/.claude/skills/` for quick access:

```
animations-core      -> frontend/animations-core/
css-core             -> frontend/css-core/
docker               -> devops/docker/
java-design-patterns-core -> backend/java-design-patterns-core/
java-spring-boot-microservices -> backend/java-spring-boot-microservices/
jenkins-pipeline     -> devops/jenkins-pipeline/
kubernetes           -> devops/kubernetes/
nosql-core           -> backend/nosql-core/
rdbms-core           -> backend/rdbms-core/
seo-keyword-research-core -> frontend/seo-keyword-research-core/
spring-boot-design-patterns-core -> backend/spring-boot-design-patterns-core/
python-system-scripting -> system/python-system-scripting/
```

---

## Skill Structure Convention

Each skill folder should contain:

```
skill-name/
├── skill.md       # Main skill file (required)
├── README.md      # Skill documentation
└── *.md           # Additional resources (optional)
```

---

## Adding New Skills

1. Determine category (backend/frontend/mobile/devops/desktop)
2. Create folder: `~/.claude/skills/{category}/{skill-name}/`
3. Add `skill.md` (main file) and `README.md`
4. Optionally create symlink in root for quick access
5. Update this INDEX.md

---

## Version History

- 2026-02-18: Created INDEX, added desktop category, organized payment-integration and javafx-ide-designer
- 2026-01-26: Initial skill structure
