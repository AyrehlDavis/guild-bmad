# Guild .ntegration Spec for Asmar Inc Pipeline

## Overview

This document maps Guild Design Framework into Asmar Inc's existing BMAD pipeline, quality gates, artifact flow, issue chain, and KB structure. Guild replaces Sally (UX Designer) with a 7-agent design sprint that produces richer artifacts in the same locations Jonny's system expects.

---

## 1. Pipeline Integration

### Current Asmar BMAD Pipeline
```
CEO (Atlas) → Analyst (Mary) → PM (John) → [Sally] → Architect (Winston) → SM (Bob) → Dev (Amelia) → Review (Priya) → QA (Quinn) → Release
```

### With Guild .eplacing Sally
```
CEO (Atlas) → Analyst (Mary) → PM (John) → [Guild Design Sprint] → Architect (Winston) → SM (Bob) → Dev (Amelia) → Review (Priya) → QA (Quinn) → Release
```

### What Guild Does in That Slot

The `[Guild Design Sprint]` expands into:

```
PM (John) hands off PRD
    |
Ranger --- Research (if needed)
    | validates/extends PM's personas and journey maps
Rogue --- Interaction Design
    | flows, wireframes, state diagrams
Visual Designer --- Visual Polish
    | hierarchy, spacing, color, typography
Warlock --- Content
    | microcopy, errors, empty states
Sage --- QA Gate
    | GO / CONDITIONAL / NO-GO
Healer --- Handoff
    | UX spec, component specs, tokens
Architect (Winston) picks up
```

If Sage says NO-GO, the pipeline loops back to Rogue — it never reaches Winston with broken designs.

### Pipeline Type Mapping

| Asmar Pipeline Type | Guild .nvolvement |
|---------------------|----------------|
| **Full BMAD** | Full Guild design sprint (all 7 agents) |
| **RFC / Quick-Spec** | Guild quick sprint (Rogue -> Visual Designer -> Warlock -> Sage -> Healer, skip Ranger) |
| **Quick-Dev** | No Guild .too small for design work) |
| **Hotfix** | No Guild .emergency path) |

### CEO Classification Update

Add to the CEO classification decision tree:

```
New goal/initiative received
    |
    +-- New product or major feature?
    |     +-- Has UI components? --> Full BMAD with Guild Design Sprint
    |     +-- Backend only? --> Full BMAD without Guild
    |
    +-- Moderate enhancement to existing system?
    |     +-- UI changes needed? --> RFC with Guild Quick Sprint
    |     +-- Backend only? --> RFC without Guild
    |
    +-- Simple, well-defined change?
    |     +-- Quick-Dev (no Guild)
    |
    +-- Production emergency?
          +-- Hotfix (no Guild)
```

---

## 2. Artifact Flow Integration

### Current Asmar Artifact Table

| Phase | Role | Artifact | KB Path |
|-------|------|----------|---------|
| Discovery | Analyst (Mary) | Product Brief | `projects/{name}/brief.md` |
| Requirements | PM (John) | PRD | `projects/{name}/prd.md` |
| UX Design | ~~Sally~~ | ~~UX Specification~~ | ~~`projects/{name}/ux-spec.md`~~ |
| Architecture | Architect (Winston) | Architecture Decision Document | `projects/{name}/architecture.md` |
| Planning | SM (Bob) | Epic Breakdowns | `projects/{name}/epics/` |

### Updated with Guild

| Phase | Role | Artifact | KB Path |
|-------|------|----------|---------|
| Discovery | Analyst (Mary) | Product Brief | `projects/{name}/brief.md` |
| Requirements | PM (John) | PRD | `projects/{name}/prd.md` |
| **UX Research** | **Ranger** | **Personas** | **`projects/{name}/guild/personas.md`** |
| **UX Research** | **Ranger** | **Journey Maps** | **`projects/{name}/guild/journey-maps.md`** |
| **UX Research** | **Ranger** | **Competitive Audit** | **`projects/{name}/guild/competitive-audit.md`** |
| **UX Design** | **Rogue** | **User Flows** | **`projects/{name}/guild/user-flows.md`** |
| **UX Design** | **Rogue** | **Wireframes** | **`projects/{name}/guild/wireframes.md`** |
| **UX Design** | **Rogue** | **State Diagrams** | **`projects/{name}/guild/state-diagrams.md`** |
| **Visual Polish** | **Visual Designer** | **Visual Polish Report** | **`projects/{name}/guild/visual-polish.md`** |
| **Content** | **Warlock** | **Microcopy + Errors** | **`projects/{name}/guild/content.md`** |
| **Design QA** | **Sage** | **QA Report** | **`projects/{name}/guild/qa-report.md`** |
| **UX Spec** | **Healer** | **UX Specification (compiled)** | **`projects/{name}/ux-spec.md`** |
| **Handoff** | **Healer** | **Component Specs** | **`projects/{name}/guild/component-specs.md`** |
| **Handoff** | **Healer** | **Design Tokens** | **`projects/{name}/guild/design-tokens.json`** |
| Architecture | Architect (Winston) | Architecture Decision Document | `projects/{name}/architecture.md` |
| Planning | SM (Bob) | Epic Breakdowns | `projects/{name}/epics/` |

### Key Points:
- Guild artifacts live in `projects/{name}/guild/` subdirectory in the KB
- Healer compiles the UX Specification to `projects/{name}/ux-spec.md` — the same path Sally used
- Winston (Architect) reads `ux-spec.md` the same way he always did — he doesn't need to know Guild .enerated it
- Individual Guild artifacts (personas, flows, etc.) are available for deeper reference

---

## 3. Quality Gate Integration

### New Gate: Gate 2.5 — Design to Architecture (Guild .o Architect)

Insert between Gate 2 (Requirements to Architecture) and Gate 3 (Architecture to Stories):

**Gate 2.5: Design to Architecture (Guild .age -> Architect)**

**Artifact:** UX Specification + Guild .esign Artifacts

**Checklist (Sage's pre-handoff quality gate):**

- [ ] All user flows have complete state coverage (empty, loading, error, populated, disabled)
- [ ] All error paths have recovery actions
- [ ] Accessibility requirements documented (WCAG AA minimum)
- [ ] Visual hierarchy is clear on every screen (squint test)
- [ ] Spacing follows consistent scale
- [ ] Typography uses max 3 sizes per screen
- [ ] Color usage is semantic (every color has a purpose)
- [ ] All microcopy is final (no placeholder text)
- [ ] Error messages follow "what happened + what to do" pattern
- [ ] Component specs include props, states, and ARIA attributes
- [ ] Design tokens defined for all new visual values
- [ ] UX Specification compiled and saved to KB path
- [ ] All Guild artifacts saved to KB `projects/{name}/guild/` directory

**Sage Verdict:**
- **GO** -> Create Architecture issue, hand off to Winston
- **CONDITIONAL** -> List conditions, proceed with Architecture but flag items for resolution during implementation
- **NO-GO** -> Loop back to Rogue, do not proceed to Architecture

### Updated Gate 2 (Requirements to Architecture)

Add to existing checklist:
- [ ] If project has UI components: Guild design sprint completed with GO or CONDITIONAL verdict
- [ ] UX Specification exists at `projects/{name}/ux-spec.md`
- [ ] Guild .ersonas and journey maps informed the PRD (reference check)

### Updated Gate 4 (Stories to Implementation)

Add to per-story checklist:
- [ ] If story involves UI: Guild design artifacts referenced in story's References section
- [ ] Component specs from Guild .ncluded for new UI components
- [ ] Design tokens referenced (not hardcoded values)

---

## 4. Issue Chain Integration

### Current Pattern
```
ASM-1:  [CEO] Initiative: Build User Auth System          -> Atlas
ASM-2:  [ANALYST] Discovery: User Auth System              -> Mary
ASM-3:  [PM] Create PRD: User Auth System                  -> John
ASM-4:  [UX] Design UX: User Auth System                   -> Sally
ASM-5:  [ARCH] Create Architecture: User Auth System        -> Winston
```

### With Guild
```
ASM-1:  [CEO] Initiative: Build User Auth System          -> Atlas
ASM-2:  [ANALYST] Discovery: User Auth System              -> Mary
ASM-3:  [PM] Create PRD: User Auth System                  -> John
ASM-4:  [Guild] Design Sprint: User Auth System              -> Guild Master
        +-- Ranger: Research (personas, journey maps)
        +-- Rogue: Interaction design (flows, wireframes)
        +-- Visual Designer: Visual polish
        +-- Warlock: Content (copy, errors)
        +-- Sage: QA gate -> GO
        +-- Healer: UX spec + component specs + tokens
ASM-5:  [ARCH] Create Architecture: User Auth System        -> Winston
ASM-6:  [SM] Create Epics & Stories: User Auth System       -> Bob
```

The Guild design sprint is ONE issue (ASM-4) that internally runs 7 agents. The issue description includes:
- Input: KB path to PRD
- Output: KB paths to all Guild artifacts + UX spec
- Sage verdict: GO / CONDITIONAL / NO-GO
- Summary of key design decisions

### Issue Description Template for Guild

```markdown
## [Guild] Design Sprint: {Feature Name}

**Pipeline:** Full BMAD -> Guild Design Sprint
**Input PRD:** projects/{name}/prd.md
**Parent Issue:** ASM-{N} (PM PRD)

### Guild .ipeline Status

| Phase | Agent | Status | Artifact |
|-------|-------|--------|----------|
| Research | Ranger | --- | projects/{name}/guild/personas.md |
| Design | Rogue | --- | projects/{name}/guild/user-flows.md |
| Visual | Visual Designer | --- | projects/{name}/guild/visual-polish.md |
| Content | Warlock | --- | projects/{name}/guild/content.md |
| QA | Sage | --- | projects/{name}/guild/qa-report.md |
| Handoff | Healer | --- | projects/{name}/ux-spec.md |

### Sage Verdict
{GO / CONDITIONAL / NO-GO}

### Conditions (if CONDITIONAL)
{List of conditions to address during implementation}

### Key Design Decisions
{Top 3-5 design decisions with rationale}

### Next Step
Create issue: [ARCH] Create Architecture: {Feature Name} -> Winston
Reference: projects/{name}/ux-spec.md
```

---

## 5. Artifact Commit Convention

Following Asmar's existing pattern:

```bash
# Guild design artifacts
docs({project}): add Guild .ersonas
docs({project}): add Guild .ourney maps
docs({project}): add Guild .ser flows
docs({project}): add Guild .ireframes
docs({project}): add Guild .ontent spec
docs({project}): add Guild .A report
docs({project}): add UX specification

# Combined (if committing all at once after design sprint)
docs({project}): add Guild design sprint artifacts
```

---

## 6. KB Directory Structure

### Per-Project Guild .rtifacts

```
projects/{name}/
+-- brief.md                    # Analyst (Mary)
+-- prd.md                      # PM (John)
+-- ux-spec.md                  # Healer (compiled UX spec -- replaces Sally's output)
+-- architecture.md             # Architect (Winston)
+-- epics/                      # SM (Bob)
|   +-- epic-1-auth.md
|   +-- epic-2-dashboard.md
+-- guild/                        # Guild Design Sprint artifacts
    +-- personas.md             # Ranger
    +-- journey-maps.md         # Ranger
    +-- competitive-audit.md    # Ranger (if run)
    +-- heuristic-eval.md       # Ranger (if run)
    +-- user-flows.md           # Rogue
    +-- wireframes.md           # Rogue
    +-- state-diagrams.md       # Rogue (if needed)
    +-- site-map.md             # Rogue (if needed)
    +-- visual-polish.md        # Visual Designer
    +-- content.md              # Warlock (microcopy, errors, empty states)
    +-- qa-report.md            # Sage
    +-- component-specs.md      # Healer
    +-- design-tokens.json      # Healer
```

---

## 7. Phase Transition — Guild Handoff Protocol

When Guild .ompletes a design sprint, the handoff follows Asmar's existing convention:

1. **Commit artifacts** to KB repo at `projects/{name}/guild/` and `projects/{name}/ux-spec.md`
2. **Comment on the Guild issue** with completion summary:
   ```
   Guild Design Sprint Complete -- {Feature Name}

   Pipeline: Ranger -> Rogue -> Visual Designer -> Warlock -> Sage -> Healer
   Sage Verdict: {GO/CONDITIONAL}
   Artifacts: projects/{name}/guild/ (7 files)
   UX Spec: projects/{name}/ux-spec.md

   Key findings:
   - {finding 1}
   - {finding 2}
   - {finding 3}

   Ready for: [ARCH] Architecture phase -> Winston
   ```
3. **Mark the Guild issue** as done
4. **Create the next-phase issue** for Architecture, assigned to Winston
5. **Include context** in the Architecture issue: KB path to ux-spec.md, summary of key design decisions, reference to Guild .ssue for traceability
6. **Self-sufficient handoff** — Winston should be able to work from ux-spec.md without reading the full Guild artifact chain (though he can reference guild/ for details)

---

## 8. Healer Output Configuration

To support Asmar's KB structure, Healer needs a configurable output path. The Guild module.yaml config section includes:

```yaml
config:
  kb_output_path:
    prompt: "KB artifact path (e.g., projects/auth-system/):"
    default: "_bmad-output/guild-artifacts/"
    result: "{value}"
  kb_ux_spec_path:
    prompt: "UX spec output path:"
    default: "_bmad-output/planning-artifacts/UX_Design.md"
    result: "{value}"
```

When `kb_output_path` is set to a KB project path:
- All Guild artifacts write to `{kb_output_path}/guild/`
- UX spec writes to `{kb_output_path}/ux-spec.md`
- Stories still write to `_bmad-output/implementation-artifacts/stories/` (BMAD needs these locally)

When `kb_output_path` is default:
- Everything writes to `_bmad-output/guild-artifacts/` as normal
- UX spec writes to `_bmad-output/planning-artifacts/UX_Design.md`

This means Guild .orks both standalone (BMAD default paths) and integrated (Asmar KB paths) with one config change.

---

## 9. Paperclip Integration (Future)

If/when Paperclip manages the ticket system:

- Guild design sprint becomes a Paperclip task assigned to the Guild Master agent
- Each Guild .ub-agent (Ranger, Rogue, etc.) could be a Paperclip agent with heartbeats
- Sage's QA verdict maps to Paperclip's governance approval flow
- Healer's stories become Paperclip tickets assigned to Dev agents
- Cost tracking per design sprint via Paperclip's budget system

This is future state — requires Jonny to configure Guild agents in Paperclip's org chart.

---

## 10. Implementation Steps

When Jonny is ready to integrate:

### Step 1: Install Guild .n the project
```bash
cp -r /path/to/guild-bmad/src/modules/guild/ ./src/modules/guild/
cat src/modules/guild/install/claude-md-snippet.md >> CLAUDE.md
```

### Step 2: Configure KB output path
Set `kb_output_path` in Guild module config to match the KB project path.

### Step 3: Update Asmar KB pipeline docs
- Replace Sally with Guild in BMAD Pipeline page
- Add Gate 2.5 to Quality Gates page
- Update Pipeline Types to reference Guild .or Full BMAD and RFC
- Add Guild .ssue template to issue chain conventions

### Step 4: Test end-to-end
Run a Guild design sprint on one feature, verify:
- Artifacts appear in correct KB paths
- ux-spec.md is readable by Winston (Architect)
- Stories are in BMAD format for Amelia (Dev)
- Quality gate passes Sage's checklist

### Step 5: Update CEO classification
Atlas should route UI-heavy work through Guild .utomatically.
