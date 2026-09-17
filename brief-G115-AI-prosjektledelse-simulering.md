---
title: "AI-styrt simulering av prosjektledelse (byggeprosjekt)"
status: draft
created: 2026-09-17
updated: 2026-09-17
---

# Product Brief: AI-styrt simulering av prosjektledelse (byggeprosjekt)

## Executive Summary

This is a web application, built for the IBE160 course (Høgskolen i Molde), that simulates the planning and execution of a construction project. The student takes the role of project manager and makes the real decisions — planning strategy, risk response, resource allocation, change approval — while an AI advisor analyzes each situation and recommends an action. After the student decides, the simulation forks and runs both paths forward from the same state: what actually happened from the student's choice, and what would have happened had they followed the AI's recommendation. Seeing those two outcomes side by side, at every decision point through to project completion, is the core teaching mechanic — closer to a flight-simulator debrief against an expert benchmark than to a conventional PM training tool.

Scenarios are generated through a hybrid approach: a template/library layer builds a coherent skeleton (WBS, resources, cost/time baseline, milestones) from chosen parameters, while an LLM layer adds narrative flavor and occasional novel risk events, so every playthrough is different without losing structural coherence. This pairing of a genuinely AI-generated recommendation with a genuinely simulated counterfactual outcome has no clear precedent in either commercial AI-PM tools or academic construction simulators, making the comparison mechanic itself the project's real contribution. The team built it with no prior construction or PM domain expertise, on assignment terms set by the course instructor.

## The Problem

Project management education typically teaches planning tools (Gantt charts, WBS, risk registers) as static artifacts to fill in correctly, not as a decision-making practice under uncertainty. Students learn *what* a schedule or risk register looks like, but rarely *how* a plan actually breaks — how a resource conflict cascades into schedule slip, how a change request forces tradeoffs between cost and time, how a risk event that seemed minor compounds into a milestone miss. Real project management judgment is built by making decisions, watching them play out, and comparing them against a better-informed alternative — something a textbook or a static spreadsheet exercise can't provide.

At the same time, AI decision-support tools are becoming part of how real project managers work — but students have little structured exposure to what it looks like to make a decision with AI-generated analysis in front of them, weigh it against their own judgment, and see which one held up.

## The Solution

A web application where the student takes the role of project manager on a simulated construction project. Each scenario is assembled through a hybrid approach: a template/library layer provides a coherent skeleton — WBS, resources, cost/time baseline, milestones — parameterized by project size, budget, and risk profile, while an LLM layer adds narrative detail, flavor, and occasional novel risk events on top, so no two playthroughs feel identical while staying structurally sound.

The core loop repeats at each decision point (planning strategy, risk response, resource allocation, change approval), through to project completion:

1. The simulation presents a situation — a schedule state, a resource conflict, an incoming change request, a risk event.
2. An AI advisor analyzes the situation and recommends an action, showing its reasoning and the consequences it projects.
3. The student makes their own decision — which may agree with or diverge from the AI's recommendation.
4. The simulation engine runs forward twice from the same state: once along the student's choice, once along the AI's recommendation — producing two real, computed outcomes (schedule, cost forecast, risk exposure) shown side by side.

That side-by-side comparison is the teaching mechanic: the student doesn't just see the outcome of their decision — they see it against a genuinely simulated benchmark and learn from where the two diverge. This requires the simulation engine to support deterministic, replayable state — forking forward from a decision point rather than only advancing linearly.

## Who This Serves

**Primary: the student playing project manager.** Someone building intuition for PM decision-making under uncertainty — likely with limited real construction or PM experience themselves — who learns fastest by acting, seeing consequences, and comparing their judgment against a reasoned alternative rather than by reading theory.

**Secondary: the course instructor, as evaluator.** Needs the application to reliably demonstrate the intended learning mechanic (decision → consequence → comparison) end-to-end, with scenarios varied enough to show it isn't a single scripted path.

There is no broader audience for this version — it is not intended for use by other students or the public.

## What Makes This Different

This is not a market product competing for adoption — it's an assignment-scoped build — so the differentiation that matters is pedagogical honesty, not a competitive moat.

- **The comparison mechanic is a genuine gap, not just a course exercise.** Existing AI project-management tools (e.g. Microsoft Project Copilot) analyze risk and recommend actions, but they optimize the live plan going forward — they don't show the student a computed side-by-side of "what you chose" vs. "what the AI would have done," run forward from the same state.
- **There is academic precedent for the structure, not the AI layer.** The Virtual Construction Simulator (VCS3, Penn State/Reading) already does plan → simulate → compare as-built vs. as-planned. This project's contribution is replacing the fixed as-planned baseline with an AI-generated, reasoned recommendation as the comparison point — closer to a flight- or medical-simulator "debrief against an expert benchmark," generalized with an LLM as the benchmark generator.

## Success Criteria

_No formal grading rubric exists from the instructor, but the working application itself is what's graded (per course requirements) — the criteria below are the team's own best read of "done," not instructor-verified requirements._

- A student can complete a full playthrough — from scenario start to project completion — encountering all four decision types (planning strategy, risk response, resource allocation, change approval) at least once.
- At each decision point, the AI advisor produces a legible recommendation with visible reasoning, and the simulation computes and displays both the student's actual outcome and the AI's counterfactual outcome from the same forked state, without manual intervention.
- Two playthroughs with the same input parameters (size, budget, risk profile) produce recognizably different scenarios (different flavor/risk events from the LLM layer) while remaining internally coherent (budget matches scope, risks fit project type).
- Outputs — Gantt schedule, cost forecast, risk exposure, scenario results, recommended actions — are all present and update correctly after each decision.
- The application runs end-to-end as a working web app the instructor can open and play through unassisted.

## Scope

**In (first version):**
- Single-player web app; no accounts or persistent history required by default *(see open question below)*.
- One domain: construction projects only *(see open question below)*.
- Scenario generation: template/library skeleton (WBS, resources, cost/time baseline, milestones) parameterized by size/budget/risk profile, with LLM-generated narrative flavor and occasional novel risk events layered on top.
- All four decision points implemented: planning strategy, risk response, resource allocation, change approval.
- The full counterfactual loop: AI recommendation → student decision → dual-simulated outcomes (student's path and AI's path, both forward-simulated from the same forked state) → side-by-side comparison.
- All five output types: Gantt schedule, cost forecast, risk exposure, scenario results, recommended actions — kept simple enough to compute and render reliably, not enterprise-grade.
- A single playthrough runs start to finish (project completion).

**Out (first version):**
- Multiplayer or negotiation features (no multi-role negotiation play, in the style of tools like Virtual Construction Negotiation).
- Compliance with real scheduling interchange formats (MSPDI, XER, PMXML) — a custom lightweight schema, informed by PMI's WBS/RBS practice standards, is enough.
- Multiple simultaneous scenario sizes/complexity tiers as a user-facing choice beyond the parameters that drive generation.
- Analytics or comparison across multiple past playthroughs.
- Domain realism guaranteed beyond what research supports — the team has no lived construction/PM expertise, so scenario and advisor realism is bounded by research quality (PMI standards, VCS3 precedent), not judgment calls.

**Open questions (deferred, not yet decided):**
- **Save/resume:** does a playthrough need to support pausing and resuming across sessions, or is a single continuous session sufficient? A full playthrough's length is untested.
- **Domain exclusivity:** is construction confirmed as the only domain for v1, or could the instructor expect a more generic "any project type" simulator? Worth a quick check against the assignment brief or instructor before build.

## Vision

If the core loop works well for construction, the same pattern — AI recommendation, human decision, dual-simulated comparison — generalizes to other decision-training domains where judgment is learned by doing and debriefing: software delivery, healthcare operations, disaster response planning. For this project, though, the goal is a working, demonstrable proof of that pattern in one domain, built to a semester's scope, not a platform.
