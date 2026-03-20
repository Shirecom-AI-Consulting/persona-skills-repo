---
name: persona-creator
description: >
  Generate complete QuadCode System persona definitions from a job description, aligned to the
  exact fields in the QuadCode persona edit form. Use this skill whenever the user wants to:
  create a new Shirecom AI Consulting persona, onboard a new AI team member from a job description,
  populate persona fields for the QuadCode multi-agent platform, or define a new role in the
  virtual company. Trigger on phrases like "create a persona", "new persona", "add a team member",
  "onboard", "generate persona from JD", or any request to add a new AI persona to the QuadCode
  system. Always use this skill — never attempt persona creation from memory alone.
---

# Persona Creator Skill

**Version:** 2.0
**Author:** Shirecom AI Consulting
**Domain:** QuadCode System — Persona Management
**Tags:** persona, quadcode, job-description, onboarding, multi-agent

---

## Purpose

Transform a job description (JD) into a fully-populated QuadCode persona payload that maps
exactly to the fields in the **Edit Persona** form. Output is delivered as both a human-readable
summary and a structured JSON file ready for form entry or API ingestion.

---

## Workflow Overview

1. **Receive** the job description (pasted text, file, or URL)
2. **Extract** role signals from the JD
3. **Generate** a realistic persona name and email
4. **Populate** every Edit Persona form field
5. **Output** summary card + JSON file + form entry cheat sheet
6. **Validate** against the field checklist

---

## Phase 1: Job Description Intake

### Accepted Input Formats
- Pasted text (most common)
- Uploaded `.txt`, `.pdf`, or `.docx` file
- A URL to a job posting (use web_fetch if available)

### Extraction Targets

| Signal | Where to Look in JD |
|--------|---------------------|
| Job title / role | Title, header, first paragraph |
| Domain / functional area | Reporting structure, team name, responsibilities |
| Core responsibilities | Bullet list of duties |
| Required skills | "Requirements" / "Qualifications" sections |
| Soft skills / personality | "We're looking for..." / culture sections |
| Seniority level | Title prefix (Sr., Principal, Lead, Manager, etc.) |
| Communication style | Role type (client-facing, internal, technical, executive) |

---

## Phase 2: Persona Name Generation

Generate a **realistic, culturally diverse professional name** — no real people.

- Format: `Firstname Lastname`
- Email: `firstname.lastname@shirecom.com`
- Owner: Default to `Wesley` unless specified otherwise

**Naming guidance by role type:**
- Engineering / Technical → grounded, precise-feeling names
- Creative / Content → names with a bit more flair
- Executive / Leadership → authoritative-sounding names
- Research / Analytical → measured, professional names

---

## Phase 3: Populate the Edit Persona Form Fields

Generate a value for **every field** in the form. Never leave a required field empty.
Use inference and professional archetype reasoning for any field not explicitly covered by the JD.

---

### SECTION: Basic Information

| Field | Type | Generation Rules |
|-------|------|-----------------|
| **Name** *(required)* | Text | Generated in Phase 2 |
| **Email** | Text | `firstname.lastname@shirecom.com` |
| **Role** *(required)* | Text | Exact job title from JD (or closest professional equivalent) |
| **Persona Type** *(required)* | Select | See Persona Type table below |
| **Domain** *(required)* | Text | Functional domain inferred from JD (e.g., `Engineering`, `Marketing`, `Design`, `Operations`, `Finance`, `Legal`, `Sales`, `Research`) |
| **Owner** | Text | `Wesley` (default) |
| **Description** | Text | 1–2 sentence expert-framing statement. Format: "Expert in [core competency], [key activity], and [key output]." |
| **Tags** | Comma-separated | 4–6 lowercase tags drawn from role domain, tools, and methods |

#### Persona Type Selection

| Persona Type | Use When |
|---|---|
| `Specialist` | Deep domain expertise, individual contributor (most common) |
| `Manager` | Leads a team or function, coordinates other personas |
| `Executive` | C-suite or VP-level strategic leadership |
| `Analyst` | Primary output is research, data, or insight |
| `Consultant` | Client-facing advisory role |
| `Coordinator` | Workflow, operations, or project coordination |

---

### SECTION: Personality Traits

| Field | Type | Generation Rules |
|-------|------|-----------------|
| **Communication Style** | Text | Choose one: `Direct` / `Collaborative` / `Analytical` / `Empathetic` / `Assertive` / `Diplomatic` |
| **Decision Style** | Text | Choose one: `Data-driven` / `Intuitive` / `Consensus-based` / `Risk-averse` / `Bold` / `Methodical` |
| **Collaboration Style** | Text | Choose one: `Independent` / `Team-oriented` / `Cross-functional` / `Facilitative` / `Supportive` |
| **Leadership Traits** | Comma-separated | 3–5 observable leadership behaviors specific to this role. Use action-oriented phrases. |

#### Leadership Traits Generation Guide

Write traits as observable behaviors, not generic virtues:
- ✅ "Sets explicit evaluation criteria before beginning any build"
- ✅ "Surfaces tradeoffs to stakeholders before making architectural decisions"
- ✅ "Shares research findings proactively before synthesis is complete"
- ❌ "Team player" (too generic)
- ❌ "Hard worker" (not observable)

Tailor traits to role domain:
- Engineering → precision, documentation, fallback design, peer review habits
- Research → rigor, question-framing, early sharing, methodology transparency
- Creative → iteration mindset, feedback-seeking, audience empathy
- Leadership → delegation patterns, escalation habits, vision-setting
- Ops/PM → milestone tracking, blocker escalation, stakeholder alignment

---

### SECTION: Resume

| Field | Type | Generation Rules |
|-------|------|-----------------|
| **Professional Summary** | Text | 2–3 sentences in third person. Focus: role identity, primary expertise, and value delivered. |
| **Career Highlights** | One per line | 3–5 lines. Each = one concrete achievement with a result. Action verb + what + measurable outcome. |
| **Skills** | Comma-separated | 6–10 skills from JD requirements plus inferred domain standards. Mix hard skills and methods. |

#### Professional Summary Formula

```
[Name] is a [seniority + role title] with expertise in [primary domain].
[He/She/They] specializes in [core capability 1] and [core capability 2],
delivering [value outcome] for [audience/team/organization].
```

#### Career Highlights Formula

Each line: `[Action verb] + [what was done] + [result or scale]`

Examples:
- `Designed RAG pipeline serving 2M+ daily queries with sub-800ms p95 latency`
- `Built content calendar driving 40% increase in organic traffic over 6 months`
- `Reduced model hallucination rate by 34% through simulation-based training framework`
- `Led usability study that reshaped product navigation for 500K active users`

Write 3–5 highlights. At least 2 should include a quantified result.

---

## Phase 4: Output Format

Always produce **three deliverables**:

### 1. Summary Card (inline in conversation)

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
QUADCODE PERSONA — SUMMARY CARD
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Name:               [Full Name]
Role:               [Job Title]
Domain:             [Domain]
Persona Type:       [Type]
Email:              [email]
Owner:              Wesley
Tags:               [tag1, tag2, tag3, tag4]

PERSONALITY
  Communication:    [style]
  Decision:         [style]
  Collaboration:    [style]
  Leadership:       [trait1] | [trait2] | [trait3]

RESUME
  Summary:          [first sentence only]
  Top Highlight:    [strongest career highlight]
  Key Skills:       [3 most relevant skills]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### 2. Full Persona JSON File

Save to: `/mnt/user-data/outputs/persona-[firstname-lastname].json`

```json
{
  "basic_information": {
    "name": "[Full Name]",
    "email": "[firstname.lastname@shirecom.com]",
    "role": "[Job Title]",
    "persona_type": "[Type]",
    "domain": "[Domain]",
    "owner": "Wesley",
    "description": "[1-2 sentence expert framing]",
    "tags": ["[tag1]", "[tag2]", "[tag3]", "[tag4]"]
  },
  "personality_traits": {
    "communication_style": "[style]",
    "decision_style": "[style]",
    "collaboration_style": "[style]",
    "leadership_traits": ["[trait1]", "[trait2]", "[trait3]", "[trait4]"]
  },
  "resume": {
    "professional_summary": "[2-3 sentence third-person summary]",
    "career_highlights": [
      "[highlight 1]",
      "[highlight 2]",
      "[highlight 3]",
      "[highlight 4]"
    ],
    "skills": "[skill1, skill2, skill3, skill4, skill5, skill6]"
  }
}
```

### 3. Form Entry Cheat Sheet (inline in conversation)

Plain-text copy of every field value, ready to paste into the Edit Persona form:

```
--- BASIC INFORMATION ---
Name:           [Full Name]
Email:          [email]
Role:           [role]
Persona Type:   [type]
Domain:         [domain]
Owner:          Wesley
Description:    [description]
Tags:           [tag1, tag2, tag3, tag4]

--- PERSONALITY TRAITS ---
Communication Style:  [style]
Decision Style:       [style]
Collaboration Style:  [style]
Leadership Traits:    [trait1], [trait2], [trait3], [trait4]

--- RESUME ---
Professional Summary:
[full summary text]

Career Highlights:
[highlight 1]
[highlight 2]
[highlight 3]
[highlight 4]

Skills:
[skill1, skill2, skill3, skill4, skill5, skill6]
```

---

## Phase 5: Validation Checklist

Before delivering output, verify every item:

**Basic Information**
- [ ] Name is a realistic, non-real person's full name
- [ ] Email follows `firstname.lastname@shirecom.com`
- [ ] Role matches or closely reflects the JD title
- [ ] Persona Type selected from the approved list
- [ ] Domain is a single functional area (not a job title)
- [ ] Owner is set to `Wesley`
- [ ] Description is 1–2 sentences in expert-framing format
- [ ] Tags are lowercase, comma-separated, 4–6 items

**Personality Traits**
- [ ] Communication Style is one of the approved values
- [ ] Decision Style is one of the approved values
- [ ] Collaboration Style is one of the approved values
- [ ] Leadership Traits are 3–5 observable, role-specific behaviors (not generic)

**Resume**
- [ ] Professional Summary is 2–3 sentences, written in third person
- [ ] Career Highlights are one per line, action-verb format
- [ ] At least 2 highlights include a quantified result
- [ ] Skills are comma-separated, 6–10 items, mix of hard skills and methods

---

## Edge Cases

| Situation | Handling |
|-----------|----------|
| Very short or vague JD | Ask 3 clarifying questions: primary output, seniority, and key tools |
| JD for a real company | Adapt to Shirecom AI Consulting context; re-frame responsibilities accordingly |
| Role already exists in roster | Flag the potential duplicate; ask if this is a variant or replacement |
| Unusual / niche role | Use closest Persona Type; add a `_note` field in the JSON explaining assumptions |
| Title only, no JD | Generate based on standard industry profile; add `"inferred": true` to JSON |

---

## Reference: Approved Field Values

### Communication Style
`Direct` | `Collaborative` | `Analytical` | `Empathetic` | `Assertive` | `Diplomatic`

### Decision Style
`Data-driven` | `Intuitive` | `Consensus-based` | `Risk-averse` | `Bold` | `Methodical`

### Collaboration Style
`Independent` | `Team-oriented` | `Cross-functional` | `Facilitative` | `Supportive`

### Persona Type
`Specialist` | `Manager` | `Executive` | `Analyst` | `Consultant` | `Coordinator`

### Domain Examples
`Engineering` | `Marketing` | `Design` | `Operations` | `Finance` | `Legal` | `Sales` |
`Research` | `Product` | `AI/ML` | `Security` | `Data` | `Content` | `Customer Success`

---

## Example Invocation

**User:** "Create a persona for a Content Marketing Manager from this JD..."

**Skill output:**
1. Extract: role=Content Marketing Manager, domain=Marketing, seniority=mid-level
2. Generate: Maya Osei → `maya.osei@shirecom.com`
3. Persona Type: `Manager` (manages content function)
4. Description: "Expert in developing content strategies, creating blog posts and whitepapers, and managing multi-channel content distribution."
5. Leadership Traits: "Builds editorial calendars 2 sprints ahead", "Reviews all content against SEO brief before publication", "Syncs with product team weekly on launch content needs"
6. Career Highlights: 4 lines, at least 2 with quantified outcomes
7. Skills: 8 comma-separated items from JD + domain standards
8. Output: Summary card + JSON file + Form entry cheat sheet

---

## Version History

- **v1.0** — Initial skill with extended Neo4j schema (prompt sets, relationships, neo4j_meta, MBTI)
- **v2.0** — Rebuilt to match exact QuadCode Edit Persona form fields:
  Basic Information (8 fields), Personality Traits (4 fields), Resume (3 fields)
