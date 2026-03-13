# PDX Design Sprint Workflow

## Trigger
User invokes this workflow with a feature or improvement area:
- "Run a design sprint on the packing screen"
- "Design sprint: add social features"
- "PDX sprint: improve onboarding"

## Pre-flight
1. Read `_bmad-output/implementation-artifacts/sprint-status.yaml` — understand current project state
   - Current sprint number and capacity
   - Existing story and epic counts
   - What's TODO vs IN PROGRESS vs DONE
2. Read `_bmad-output/planning-artifacts/project-context.md` — understand the product
3. Read `_bmad-output/planning-artifacts/prd.md` — understand requirements
4. Read existing `_bmad-output/pdx-artifacts/` — don't repeat prior work
5. Ask user ONE clarifying question if scope is ambiguous
6. Confirm: "I'll run the full PDX pipeline on [scope]. This will produce research, designs, copy, QA, and dev-ready handoff. Ready?"

## Pipeline Execution

### Phase 1: Nova (Research)
Run automatically based on what's needed:
- IF no personas exist in pdx-artifacts/ → generate personas
- IF personas exist → skip (reference existing)
- ALWAYS run a targeted heuristic eval on the feature scope
- IF feature is new → run competitive audit
- IF feature exists → run journey map for the feature
- Save all to `_bmad-output/pdx-artifacts/`
- Report: "Phase 1 complete — [n] research artifacts produced"

### Phase 2: Kai (Design)
Reads Nova's output automatically:
- Generate user flow for the feature (reference personas from Phase 1)
- IF flow has complex states → generate state diagram
- IF flow has multiple actors → generate swim lane
- Generate wireframe for key screens
- Save all to `_bmad-output/pdx-artifacts/`
- Report: "Phase 2 complete — [n] design artifacts produced"

### Phase 3: Echo (Content)
Reads Kai's wireframes and Nova's personas automatically:
- Write microcopy for all screens in Kai's flow
- Write error messages for all error states Kai identified
- Write empty state copy for any empty states in the flow
- IF onboarding is involved → write onboarding copy
- Save all to `_bmad-output/pdx-artifacts/`
- Report: "Phase 3 complete — [n] content artifacts produced"

### Phase 4: Sage (QA)
Reads everything from Phases 1-3 automatically:
- Run design review on Kai's flows
- Run a11y QA check
- Check Echo's copy against voice/tone guidelines
- Run pre-handoff quality gate
- Issue GO / CONDITIONAL / NO-GO verdict
- IF NO-GO → list blockers and STOP pipeline
- IF CONDITIONAL → list conditions and continue
- IF GO → continue to Phase 5
- Save QA report to `_bmad-output/pdx-artifacts/`
- Report: "Phase 4 complete — verdict: [GO/CONDITIONAL/NO-GO]"

### Phase 5: Relay (Handoff)
Reads everything from Phases 1-4 + sprint-status.yaml:
- Generate stories with correct numbering (continue from existing highest ID)
- Assign to correct sprint (current or next based on capacity)
- Write component specs for new UI elements
- Extract design tokens
- Update sprint-status.yaml with new stories (append, never overwrite)
- Save story files to `_bmad-output/implementation-artifacts/stories/`
- Save specs and tokens to `_bmad-output/pdx-artifacts/`
- Report: "Phase 5 complete — [n] stories generated ([story range])"

### Phase 6: PM Review
Load BMAD's PM agent to review Relay's output.

The PM validates:
- Do the stories align with the PRD and product goals?
- Is the prioritization correct (P0/P1/P2)?
- Are the acceptance criteria complete and testable?
- Do the story descriptions match the intended user outcomes?
- Are there missing stories that the design artifacts imply but Relay didn't create?
- Are there stories that are out of scope and should be removed or deferred?
- Do epic groupings make sense?

PM actions:
- IF stories are aligned → approve and pass to SM
- IF stories need adjustment → modify titles, acceptance criteria, or priority, then pass to SM
- IF stories are fundamentally misaligned with PRD → flag to user with specific concerns, suggest changes, get confirmation before continuing

Output: Updated story files with PM approval status
Save PM review notes to `_bmad-output/pdx-artifacts/pm-review-[scope].md`
Report: "Phase 6 complete — PM review: [APPROVED/APPROVED WITH CHANGES/FLAGGED]"

### Phase 7: SM Sprint Planning
Load BMAD's SM agent to plan the sprint.

The SM validates and organizes:
- Read sprint-status.yaml for current sprint capacity and velocity
- Check what's already in the current sprint (don't overload)
- Validate story point estimates against team velocity
- Check dependencies between new stories and existing work
- Sequence stories within the sprint (what needs to be built first?)
- IF total points exceed sprint capacity → split across sprints, prioritize P0 first
- Assign stories to appropriate sprint(s)

SM actions:
- Update sprint-status.yaml with final sprint assignments
- Set story statuses to ready-for-dev
- Create sprint summary with:
  - Sprint goal
  - Stories included (with sequence)
  - Total points
  - Dependencies and blockers
  - Capacity check (points assigned vs team velocity)

Output: Updated sprint-status.yaml, sprint summary
Save sprint plan to `_bmad-output/implementation-artifacts/sprint-plan-[sprint-number].md`
Report: "Phase 7 complete — stories assigned to Sprint [number], [points] points planned"

## Post-Pipeline Report
After all 7 phases complete, output a summary:
```
PDX Design Sprint Complete — [Feature/Scope]

Pipeline: Nova → Kai → Echo → Sage → Relay → PM → SM

Artifacts produced: [count]
Research: [list]
Designs: [list]
Content: [list]
QA verdict: [GO/CONDITIONAL/NO-GO]
PM review: [APPROVED/APPROVED WITH CHANGES/FLAGGED]
Stories generated: [count] ([story range])
Sprint assignment: Sprint [number]
Total points: [sum]
Sprint capacity: [points assigned] / [velocity]
Conditions (if any): [list]

Ready for: @dev to pick up [first story ID] with /ds
```

## Error Handling
- If any phase fails, save what was completed and report where it stopped
- If sprint-status.yaml doesn't exist, warn user and create stories without sprint assignment
- If scope is too broad (>20 stories generated), suggest breaking into multiple sprints
- If Sage issues NO-GO, the pipeline stops — report all blockers and which phase produced them
- If PM flags stories as fundamentally misaligned, pause for user confirmation before continuing

## Quick Sprint Variant
Skip Phase 1 (Nova) when research already exists. Runs: Kai → Echo → Sage → Relay → PM → SM.
Triggered by `/quick-sprint`.

## Research Only Variant
Run only Phase 1 (Nova). Save findings for later pipeline execution.
Triggered by `/research-only`.

## Handoff Only Variant
Skip Phases 1-4. Run Relay → PM → SM against existing pdx-artifacts/.
Triggered by `/handoff-only`.
