# 🤖 Claude Global Library

**Version:** 1.0.0
**Purpose:** Reusable skills and agents for Claude AI projects
**License:** MIT

---

## 🎯 What is This?

**Claude Global Library** is a collection of **pre-built skills and agents** that you can use in your Claude AI projects.

### What are Skills?
Skills are knowledge modules that teach Claude about specific technologies or patterns:
- 🐳 Docker - Container best practices
- ☸️ Kubernetes - K8s deployment patterns
- ☕ Java Spring Boot - Microservices architecture
- 🗄️ RDBMS - Database design and SQL
- 🎨 CSS - Modern web styling

### What are Agents?
Agents are specialized assistants that autonomously handle complex tasks:
- 🚀 DevOps Engineer - CI/CD, deployment, infrastructure
- 🧪 QA Testing Agent - Test design and validation
- ☕ Spring Boot Microservices - Backend development
- 🎨 UI/UX Designer - Interface design

---

## 📦 What's Included?

### Skills (27 total)
```
✅ Backend Development
   - java-spring-boot-microservices
   - spring-boot-design-patterns-core
   - rdbms-core
   - nosql-core

✅ DevOps & Infrastructure
   - docker
   - kubernetes
   - jenkins-pipeline

✅ Frontend Development
   - css-core
   - animations-core
   - frontend

✅ Mobile Development
   - mobile
   - android (backend/UI)
   - swift (backend/UI)

✅ AI & Automation
   - adaptive-skill-intelligence
   - context-management-core
   - model-selection-core
   - task-planning-intelligence

✅ SEO & Marketing
   - seo-keyword-research-core
   - dynamic-seo-agent
   - static-seo-agent

... and more!
```

### Agents (12 total)
```
✅ Backend Development
   - spring-boot-microservices
   - android-backend-engineer
   - swift-backend-engineer

✅ Frontend/UI Development
   - angular-engineer
   - ui-ux-designer
   - android-ui-designer
   - swiftui-designer

✅ DevOps & Quality
   - devops-engineer
   - qa-testing-agent

✅ SEO
   - dynamic-seo-agent
   - static-seo-agent

✅ Orchestration
   - orchestrator-agent
```

---

## 🚀 QUICK START

### 1. Download Skills/Agents

```bash
# Clone this repository
git clone https://github.com/piyushmakhija28/claude-global-library.git

# Or download specific skills/agents you need
```

### 2. Install to Your Project

```bash
# Copy skills you need
cp -r claude-global-library/skills/docker ~/.claude/skills/
cp -r claude-global-library/skills/kubernetes ~/.claude/skills/

# Copy agents you need
cp -r claude-global-library/agents/devops-engineer ~/.claude/agents/
cp -r claude-global-library/agents/qa-testing-agent ~/.claude/agents/
```

### 3. Use in Conversations

**Using Skills:**
```
User: "I need help with Docker"
Claude: [Automatically loads docker skill knowledge]
```

**Using Agents:**
```
User: "Deploy this app using DevOps agent"
Claude: [Launches devops-engineer agent to handle deployment]
```

---

## 📖 SKILL CATEGORIES

### 🏗️ Backend Development
| Skill | Description | Use When |
|-------|-------------|----------|
| `java-spring-boot-microservices` | Spring Boot patterns, REST APIs, microservices | Building Java backends |
| `spring-boot-design-patterns-core` | Design patterns in Spring Boot | Architecting scalable systems |
| `rdbms-core` | PostgreSQL/MySQL schema design, queries | Working with relational databases |
| `nosql-core` | MongoDB/Elasticsearch patterns | Working with NoSQL databases |

### 🐳 DevOps & Infrastructure
| Skill | Description | Use When |
|-------|-------------|----------|
| `docker` | Dockerfile best practices, multi-stage builds | Containerizing applications |
| `kubernetes` | K8s deployments, services, ingress | Deploying to Kubernetes |
| `jenkins-pipeline` | CI/CD pipelines, Jenkinsfile | Setting up automation |

### 🎨 Frontend Development
| Skill | Description | Use When |
|-------|-------------|----------|
| `css-core` | Modern CSS, Flexbox, Grid, responsive design | Styling web applications |
| `animations-core` | CSS/JS animations, performance | Adding UI animations |
| `angular-engineer` | Angular patterns, components, routing | Building Angular apps |

### 📱 Mobile Development
| Skill | Description | Use When |
|-------|-------------|----------|
| `mobile` | General mobile patterns | Cross-platform development |
| `android-backend-engineer` | Android + backend integration | Building Android apps |
| `swiftui-designer` | SwiftUI patterns and layouts | Building iOS apps |

---

## 🤖 AGENT CATEGORIES

### 🚀 Infrastructure & Operations
| Agent | Specialization | Use For |
|-------|----------------|---------|
| `devops-engineer` | CI/CD, Docker, K8s, deployment | Full DevOps workflows |
| `qa-testing-agent` | Test design, validation, regression | Quality assurance |

### ☕ Backend Development
| Agent | Specialization | Use For |
|-------|----------------|---------|
| `spring-boot-microservices` | Java Spring Boot backends | Complex microservices |
| `android-backend-engineer` | Android + API integration | Mobile backend logic |
| `swift-backend-engineer` | Swift backend systems | iOS backend logic |

### 🎨 UI/UX Design
| Agent | Specialization | Use For |
|-------|----------------|---------|
| `ui-ux-designer` | Web UI/UX design | Modern web interfaces |
| `angular-engineer` | Angular apps | Frontend implementation |
| `android-ui-designer` | Android XML layouts | Android UI design |
| `swiftui-designer` | SwiftUI interfaces | iOS UI design |

### 🔗 Orchestration
| Agent | Specialization | Use For |
|-------|----------------|---------|
| `orchestrator-agent` | Multi-agent coordination | Complex multi-step workflows |

---

## 💡 USAGE EXAMPLES

### Example 1: Docker + DevOps

**Setup:**
```bash
cp -r skills/docker ~/.claude/skills/
cp -r agents/devops-engineer ~/.claude/agents/
```

**Use:**
```
User: "Create a Dockerfile for my Spring Boot app and set up CI/CD"

Claude:
1. Uses 'docker' skill for Dockerfile best practices
2. Launches 'devops-engineer' agent for CI/CD setup
3. Creates Dockerfile + Jenkinsfile
4. Configures deployment pipeline
```

### Example 2: Full Stack Development

**Setup:**
```bash
cp -r skills/{java-spring-boot,rdbms-core,css-core} ~/.claude/skills/
cp -r agents/{spring-boot-microservices,ui-ux-designer} ~/.claude/agents/
```

**Use:**
```
User: "Build a user management system with Spring Boot backend and modern UI"

Claude:
1. Launches 'spring-boot-microservices' agent for backend
2. Launches 'ui-ux-designer' agent for frontend
3. Uses 'rdbms-core' skill for database design
4. Uses 'css-core' skill for styling
5. Delivers complete full-stack solution
```

---

## 🎯 WHEN TO USE WHAT?

### Use Skills When:
- ✅ You need knowledge about a technology
- ✅ You want best practices and patterns
- ✅ You're asking questions or learning
- ✅ You need quick guidance

**Example:** "What's the best way to structure a Spring Boot project?" → Use `java-spring-boot-microservices` skill

### Use Agents When:
- ✅ You need autonomous task execution
- ✅ Complex, multi-step workflows
- ✅ Specialized implementation work
- ✅ You want hands-off automation

**Example:** "Deploy my app to Kubernetes" → Use `devops-engineer` agent

---

## 🛠️ CUSTOMIZATION

### Create Your Own Skills

```bash
# 1. Create skill directory
mkdir ~/.claude/skills/my-custom-skill

# 2. Create skill.md
cat > ~/.claude/skills/my-custom-skill/skill.md <<EOF
# My Custom Skill

Specialized knowledge about [your topic]

## When to Use
- Use when user asks about [topic]
- Use when implementing [feature]

## Knowledge
[Your expertise here]
EOF

# 3. Use in conversations
```

### Create Your Own Agents

```bash
# 1. Create agent directory
mkdir ~/.claude/agents/my-custom-agent

# 2. Create agent.md
cat > ~/.claude/agents/my-custom-agent/agent.md <<EOF
# My Custom Agent

Autonomous agent for [specific task]

## Capabilities
- [Task 1]
- [Task 2]

## Tools Available
- Read, Write, Edit, Bash, etc.
EOF

# 3. Launch via Task tool
```

---

## 📚 DOCUMENTATION

### Skill Documentation
- **Docker:** `skills/docker/README.md`
- **Kubernetes:** `skills/kubernetes/README.md`
- **Spring Boot:** `skills/java-spring-boot-microservices/README.md`
- [View all skill docs](docs/skills/)

### Agent Documentation
- **DevOps Engineer:** `agents/devops-engineer/README.md`
- **QA Testing:** `agents/qa-testing-agent/README.md`
- **Spring Boot:** `agents/spring-boot-microservices/README.md`
- [View all agent docs](docs/agents/)

---

## 🤝 CONTRIBUTING

Want to add your own skills/agents to this library?

1. Fork the repository
2. Add your skill/agent with documentation
3. Test thoroughly
4. Submit a pull request

**Guidelines:**
- Skills/agents must be **general-purpose** (not project-specific)
- Include clear documentation and examples
- No proprietary or sensitive information
- Follow naming conventions

---

## 📦 RELATED PROJECTS

### Claude Insight (Monitoring Dashboard)
- **Purpose:** Monitor Claude Memory System performance
- **Repo:** https://github.com/piyushmakhija28/claude-insight
- **Use:** Track costs, sessions, policy enforcement

### Claude Memory System
- **Purpose:** Core automation framework
- **Docs:** Included in Claude Insight
- **Use:** Session management, policy enforcement

---

## 🎯 FAQ

**Q: Do I need to download all skills/agents?**
A: No! Download only what you need for your projects.

**Q: Can I modify skills/agents?**
A: Yes! Customize them for your specific use cases.

**Q: Are skills/agents required for Claude?**
A: No, they're optional enhancements that make Claude more specialized.

**Q: How do I update skills/agents?**
A: Pull latest changes from this repo and copy updated files.

---

## 📄 LICENSE

MIT License - Free to use, modify, and distribute.

---

## 👨‍💻 MAINTAINED BY

**TechDeveloper**
Website: https://www.techdeveloper.in
GitHub: https://github.com/piyushmakhija28

---

**Download skills/agents you need and enhance your Claude AI projects!** 🚀
