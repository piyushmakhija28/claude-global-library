# 🤖 Custom Agents

**Purpose:** Custom agent definitions and configurations

---

## 📋 What This Contains

Custom agent files that extend Claude Code with specialized capabilities:
- Migration expert agents
- Domain-specific agents
- Custom workflow agents

---

## 📁 Files in This Folder

### **Migration Agents:**
- `migration-expert-agent.md` - Expert agent for framework migrations, database migrations, and system upgrades

---

## 🎯 Usage

### **Using Migration Expert Agent:**

**Via Task Tool:**
```
Task(
    subagent_type="migration-expert",
    prompt="Migrate Spring Boot from 2.7.18 to 3.2.0"
)
```

**Features:**
- ✅ Framework migrations
- ✅ Database migrations
- ✅ API version migrations
- ✅ Dependency upgrades
- ✅ Rollback planning
- ✅ Testing strategies

---

## 📖 Agent Structure

**Each agent file contains:**
- Agent name and description
- Specialization area
- Available tools
- Usage instructions
- Example prompts
- Best practices

---

## ✅ Benefits

- **Specialized:** Domain-specific expertise
- **Reusable:** Invoke agents across projects
- **Documented:** Clear usage instructions
- **Tested:** Production-ready agents

---

**Location:** `~/.claude/memory/agents/`
