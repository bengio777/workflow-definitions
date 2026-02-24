# AI Building Block Spec: Workflow Naming

## Execution Pattern
**Recommended Pattern:** Skill-Powered Prompt
**Reasoning:**
- Conversational coaching for name/description generation with Notion write at the end
- Domain pattern matching and name generation are LLM tasks, not multi-agent orchestration
- Notion MCP used for two operations: reading Business Processes and writing the Workflow entry
- Linear flow with two human checkpoints — no branching, routing, or parallelization
- All naming rules, anti-patterns, and domain patterns fit in a single SKILL.md

---

## Scenario Summary
| Field | Value |
|-------|-------|
| **Workflow Name** | Workflow Naming |
| **Description** | Generate consistent, outcome-focused names and descriptions for business workflows using domain-specific naming patterns, then create entries in the Notion Workflows database with full property mapping. |
| **Process Outcome** | Named workflow entry in Notion Workflows database |
| **Trigger** | User needs to name a workflow, write a description, or add a workflow to Notion |
| **Type** | Augmented |
| **Business Process** | Operations |

---

## Step-by-Step Decomposition
| Step | Name | Autonomy Level | Building Block(s) | Tools / Connectors | Skill Candidate | HITL Gate |
|------|------|----------------|--------------------|--------------------|-----------------|-----------|
| 1 | Describe workflow | Human | — | — | — | User provides workflow description |
| 2 | Identify domain and pattern | AI-Deterministic | Classification, Pattern Matching | references/examples-by-process.md | `naming-workflows` | None |
| 3 | Generate name options | AI-Deterministic | LLM Generation | — | `naming-workflows` | None |
| 4 | Write description + process outcome | AI-Deterministic | LLM Generation | — | `naming-workflows` | None |
| 5 | Select and confirm | Human | — | — | — | User picks name, description, outcome |
| 6 | Link to business process | AI + Human | Data Retrieval, Sequence Assignment | Notion MCP (Business Processes DB) | `naming-workflows` | User confirms process linkage |
| 7 | Create entry in Notion | Automation | Data Write | Notion MCP (Workflows DB) | `naming-workflows` | None |
| 8 | Confirm creation | AI-Semi-Autonomous | LLM Generation | — | `naming-workflows` | None |

## Autonomy Spectrum Summary
```
|--Human------AI-Deterministic----Human------AI+Human---Auto---AI--|
   Step 1     Steps 2, 3, 4      Step 5     Step 6     Step 7 Step 8
```

---

## Skill Candidates
### `naming-workflows`
**Purpose:** Generate domain-appropriate workflow names and descriptions following naming conventions, then register the workflow in Notion with full property mapping.

**Inputs:**
| Input | Source | Required |
|-------|--------|----------|
| Workflow description (rough or formal) | Human (conversation) | Yes |
| Name selection | Human (Step 5) | Yes |
| Business process confirmation | Human (Step 6) | Yes |
| Business Processes database (live) | Notion MCP | Yes |
| Existing workflows under selected process (live) | Notion MCP | For sequence assignment |

**Outputs:**
| Output | Format | Destination |
|--------|--------|-------------|
| 2-3 name options with descriptions | Structured text | Claude response |
| Workflow entry | Notion page with full properties | Notion Workflows DB |
| Confirmation with Notion link | Text | Claude response |

**Decision Logic:**
- Domain detection maps workflow to one of 7 business domains (Sales, Marketing, Product, Education, Consulting, Operations, Finance)
- Each domain has a naming pattern (e.g., Sales: [Prospect Type] [Action])
- Names must pass 4 rules: 2-4 words, noun phrase, title case, self-explanatory
- 5 anti-patterns checked: verb phrases, too generic, too long, tool-focused, jargon
- Sequence number: query existing workflows under business process, assign next multiple of 10
- If no matching business process exists: ask whether to create one or leave unlinked

**Failure Modes:**
| Failure | Detection | Recovery |
|---------|-----------|----------|
| Name exceeds 4 words | Word count check on generated names | Simplify to core noun phrase; if irreducible, may be two workflows |
| No business process match | Notion query returns no matching processes | Ask user: create new process or leave unlinked |
| Duplicate workflow name | Notion query finds existing entry with same name | Ask user: update existing entry or differentiate new one |
| User can't articulate workflow | Description is vague or missing | Ask three grounding questions: "What triggers this? What's the output? Who does it?" |
| Notion MCP failure | Connection error on read or write | Report error; suggest checking MCP configuration |

---

## Step Sequence and Dependencies
```
[1] Describe workflow (Human)
 └──► [2] Identify domain and pattern (depends on: workflow description)
       └──► [3] Generate name options (depends on: domain + pattern)
             └──► [4] Write description + process outcome (depends on: name options)
                   └──► [5] Select and confirm (Human — depends on: presented options)
                         └──► [6] Link to business process (depends on: confirmed name)
                               └──► [7] Create entry in Notion (depends on: confirmed process link)
                                     └──► [8] Confirm creation (depends on: successful write)
```

**Dependency Map:**
- Steps 2-4 are a fast AI burst (domain detection, name generation, description writing)
- Step 5 is the primary human gate — nothing writes to Notion until user confirms
- Step 6 requires Notion MCP read (Business Processes DB) and human confirmation
- Step 7 requires Notion MCP write (Workflows DB) with all properties populated
- Step 8 depends on successful Step 7 write to report the Notion link

---

## Prerequisites
- Claude Code installed and configured
- `naming-workflows` skill installed at `~/.claude/skills/naming-workflows/`
- Notion MCP configured with access to Workflows and Business Processes databases
- Reference file present: `references/examples-by-process.md`

## Context Inventory
| Context | Source | When Loaded |
|---------|--------|-------------|
| Domain naming patterns (7 domains) | SKILL.md | Skill activation |
| Naming rules and anti-patterns | SKILL.md | Skill activation |
| Examples by business process | references/examples-by-process.md | Step 2 (domain identification) |
| Notion property schema (Name, Description, Process Outcome, etc.) | SKILL.md | Step 7 (entry creation) |
| Business Processes database (live) | Notion MCP | Step 6 |
| Existing workflows under process (live) | Notion MCP | Step 6 (sequence assignment) |

## Tools and Connectors
| Tool | Purpose | Required |
|------|---------|----------|
| Notion MCP | Read Business Processes DB; write to Workflows DB | Yes |

## Recommended Implementation Order
1. Write SKILL.md with naming conventions, domain patterns, anti-patterns, and Notion property schema
2. Build references/examples-by-process.md with real examples per domain
3. Verify Notion MCP connectivity to both databases
4. Test name generation for workflows in 3+ different domains
5. Test end-to-end Notion write with full property mapping
6. Test edge cases: duplicate name, no matching process, vague description

## Where to Run
- **Platform:** Claude Code
- **Execution:** Interactive conversation via `naming-workflows` skill
- **GitHub:** bengio777/agent-skills
- **Skill Location:** `~/.claude/skills/naming-workflows/`
