# BMAD Integration Rules

## PM → Guild Handoff
When BMAD's PM agent creates a PRD, it may also generate Guild-format artifacts:
- Persona cards (saved to _bmad-output/guild-artifacts/)
- Journey maps (saved to _bmad-output/guild-artifacts/)
- Research summaries embedded in the PRD

These are valid Guild artifacts. Guild agents should:
- READ them as context
- REFERENCE them in downstream artifacts
- NOT recreate them unless explicitly asked
- EXTEND them if the scope has changed

## Guild . BMAD Dev Handoff
When Guild .ompletes a design sprint:
- Stories go to _bmad-output/implementation-artifacts/stories/
- sprint-status.yaml is updated with new stories
- UX_Design.md is generated at _bmad-output/planning-artifacts/
- BMAD's dev agent picks up stories via /dev-story
- BMAD's SM agent manages sprint planning

## Sally Replacement
Guild replaces BMAD's built-in UX Designer (Sally). When Guild .s installed:
- The PM should route to Guild .fter PRD, not to Sally
- If the PM offers "UX design" as a next step, it means "run Guild design sprint"
- The architecture phase should reference Guild artifacts, not Sally's UX_Design.md
- After architecture, Guild Master handles the rest

## Artifact Ownership
| Artifact | Owner | Location |
|----------|-------|----------|
| Product Brief | BMAD Analyst | _bmad-output/planning-artifacts/ |
| PRD | BMAD PM | _bmad-output/planning-artifacts/ |
| Architecture | BMAD Architect | _bmad-output/planning-artifacts/ |
| Personas | Guild .ova (or PM) | _bmad-output/guild-artifacts/ |
| Journey Maps | Guild .ova (or PM) | _bmad-output/guild-artifacts/ |
| User Flows | Guild .ai | _bmad-output/guild-artifacts/ |
| Wireframes | Guild .ai | _bmad-output/guild-artifacts/ |
| Microcopy | Guild .cho | _bmad-output/guild-artifacts/ |
| Error Messages | Guild .cho | _bmad-output/guild-artifacts/ |
| QA Reports | Guild .age | _bmad-output/guild-artifacts/ |
| Component Specs | Guild .elay | _bmad-output/guild-artifacts/ |
| Design Tokens | Guild .elay | _bmad-output/guild-artifacts/ |
| UX_Design.md | Guild .elay (compiled) | _bmad-output/planning-artifacts/ |
| Stories | Guild .elay | _bmad-output/implementation-artifacts/stories/ |
| Sprint Status | BMAD SM | _bmad-output/implementation-artifacts/ |

## Flow for Existing Projects (Brownfield)

The typical brownfield flow when PM creates a PRD with Guild artifacts:

1. PM creates PRD → generates personas + journey maps as Guild artifacts
2. User runs @guild-master /design-sprint
3. Orchestrator detects: brownfield + existing personas + existing journey maps
4. Ranger: skips persona/journey, runs only what's missing (heuristic eval, competitive audit)
5. Rogue → Warlock → Sage → Healer → PM review → SM → Dev

## Flow for New Projects (Greenfield)

1. Analyst creates product brief
2. PM creates PRD → may generate initial personas
3. @guild-master /design-sprint detects greenfield
4. Ranger: validates/extends PM's personas, runs full research suite
5. Rogue → Warlock → Architect → Sage → Healer → PM review → SM → Dev
