---
name: weekly-status-update
description: "Generate a weekly development status update for the T6IR project by pulling the latest sprint data from Jira and publishing a formatted report to Confluence. Covers completed work, in-progress items, blockers, metrics, and next week outlook."
allowed-tools: Read, Grep, Glob, Bash
---

# Weekly Development Status Update

Generate a polished weekly status update for the T6IR (Talosix IR) project by pulling live data from Jira and publishing to the Product Management Confluence space.

## When to Use

- End of each sprint week (typically Friday)
- When stakeholders or leadership need a project pulse
- Before sprint reviews or stakeholder meetings
- Anytime someone asks "what's the status of T6IR?"

## Workflow

### Step 1: Pull Data from Jira

Use the Atlassian MCP to query the T6IR project. Run these JQL queries:

**Completed this week:**
```
project = T6IR AND status changed to Done DURING (startOfWeek(), now()) ORDER BY resolved DESC
```

**In Progress:**
```
project = T6IR AND status = "In Progress" ORDER BY priority DESC
```

**In Review / QA:**
```
project = T6IR AND status IN ("In Review", "Code Review", "QA", "Testing") ORDER BY priority DESC
```

**Blockers and Impediments:**
```
project = T6IR AND (status = Blocked OR flagged = impediment OR priority = Blocker) ORDER BY created DESC
```

**Created this week (new scope):**
```
project = T6IR AND created >= startOfWeek() ORDER BY created DESC
```

**Bugs opened this week:**
```
project = T6IR AND type = Bug AND created >= startOfWeek() ORDER BY priority DESC
```

**Bugs resolved this week:**
```
project = T6IR AND type = Bug AND status changed to Done DURING (startOfWeek(), now())
```

**Current sprint info:**
```
project = T6IR AND sprint in openSprints()
```

### Step 2: Calculate Metrics

From the queried data, compute:

- **Velocity**: Story points completed this week
- **Completion rate**: Stories done / stories committed for sprint
- **Bug ratio**: Bugs opened vs resolved this week
- **Blocker count**: Active blockers and average age
- **Scope change**: Stories added after sprint start
- **PR throughput**: PRs merged this week (if GitHub MCP available)

### Step 3: Generate Status Report

Format the report using this template:

```markdown
# T6IR Weekly Status Update

**Week of:** [Date Range]
**Sprint:** [Sprint Name/Number]
**Sprint Progress:** [X]% complete ([days remaining] days left)
**Overall Health:** 🟢 On Track | 🟡 At Risk | 🔴 Off Track

---

## 📊 Key Metrics

| Metric | This Week | Last Week | Trend |
|--------|-----------|-----------|-------|
| Stories Completed | X | X | ↑/↓/→ |
| Story Points Delivered | X | X | ↑/↓/→ |
| Bugs Opened | X | X | ↑/↓/→ |
| Bugs Resolved | X | X | ↑/↓/→ |
| Active Blockers | X | X | ↑/↓/→ |
| PRs Merged | X | X | ↑/↓/→ |

---

## ✅ Completed This Week

| Key | Title | Type | Points |
|-----|-------|------|--------|
| T6IR-XXX | [Story title] | Story/Bug/Task | X |
| ... | ... | ... | ... |

**Highlights:**
- [1-2 sentence summary of most impactful completions]
- [Notable achievement or milestone reached]

---

## 🔄 In Progress

| Key | Title | Assignee | Status | Points |
|-----|-------|----------|--------|--------|
| T6IR-XXX | [Story title] | [Name] | In Progress / In Review | X |
| ... | ... | ... | ... | ... |

---

## 🚧 Blockers & Risks

| Key | Title | Blocker Description | Days Blocked | Owner | Action Needed |
|-----|-------|--------------------:|-------------|-------|---------------|
| T6IR-XXX | [Title] | [What's blocking] | X | [Name] | [What needs to happen] |

**Risk Items:**
- [Any risks to sprint commitment]
- [Dependencies on external teams or vendors]
- [Technical risks identified]

---

## 🐛 Bug Status

| Status | Count |
|--------|-------|
| Opened this week | X |
| Resolved this week | X |
| Open (total) | X |
| Critical/Blocker open | X |

---

## 📋 Sprint Burndown

- **Committed:** X stories / X points
- **Completed:** X stories / X points
- **Remaining:** X stories / X points
- **Added after start:** X stories (scope change)
- **Forecast:** On track to complete by sprint end? Yes/No

---

## 🔮 Next Week Outlook

**Planned Focus:**
- [Top priority items for next week]
- [Key stories expected to complete]
- [Any planned releases or deployments]

**Upcoming Milestones:**
- [Next milestone and target date]
- [Any demos, reviews, or stakeholder meetings]

**Dependencies:**
- [Items waiting on other teams]
- [External dependencies or decisions needed]

---

## 💬 Team Notes

- [Any team capacity changes - PTO, new joiners]
- [Process improvements or retrospective actions]
- [Shout-outs or notable contributions]

---

*Report generated: [timestamp]*
*Data source: Jira project T6IR, sprint [sprint name]*
*Published to: [Confluence page link]*
```

### Step 4: Determine Health Status

Apply these rules for the health indicator:

- **🟢 On Track**: Sprint burndown on pace, 0-1 blockers, no critical bugs unresolved > 2 days
- **🟡 At Risk**: Sprint burndown slightly behind (>10% off pace), 2+ blockers, or scope added mid-sprint
- **🔴 Off Track**: Sprint burndown significantly behind (>25% off pace), critical blocker > 3 days, or key stories at risk of missing sprint

### Step 5: Publish to Confluence

Use the Atlassian MCP to publish the report:

1. **Find or create the Status Update hub page** in the Product Management space
   - Search for existing page: `space = PM AND title = "T6IR Weekly Status Updates"`
   - If it exists, create a child page under it
   - If not, create the hub page first, then add the update as a child

2. **Page title format**: `T6IR Status Update - Week of [Month Day, Year]`

3. **Labels**: `status-update`, `t6ir`, `weekly-report`, `[sprint-name]`

4. **Confluence formatting**: Convert the markdown to Confluence storage format:
   - Use Confluence macros for tables, status badges, and panels
   - Add an info panel at the top with the health status
   - Use the expand macro for detailed sections if the report is long

### Step 6: Notify Stakeholders

Offer to:
- Post a summary to Slack with a link to the Confluence page
- Tag specific stakeholders if there are blockers requiring attention
- Create Jira comments on blocked items noting they were flagged in the status report

## Customization Options

When invoked, ask the user:
- "Want the standard T6IR report or should I focus on a specific epic/area?"
- "Any additional context to include (decisions made, strategy changes)?"
- "Should I publish directly to Confluence or generate a draft first?"

## Historical Tracking

Each published report should include a link to the previous week's report at the bottom, creating a chain of weekly updates for easy historical browsing.
