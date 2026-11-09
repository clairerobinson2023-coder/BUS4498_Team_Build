# TripSync handoff

## What works now

The TripSync runtime plans, rules, test plan, workflow, task summary, and task specifications are present in the repository. No website or Supabase Edge Function has been built yet.

## Where things live

- Runtime plans and rules: `my_first_agent/runtime/`
- Workflow and task summary: `my_first_agent/agent/workflow-of-tasks.md` and `my_first_agent/agent/task-summary.md`
- Task specifications: `my_first_agent/agent/task-specs/`
- Existing templates preserved: `workflow-of-tasks-1.md`, `task-summary-1.md`

## Secrets

No secrets are configured or stored in the repository. External-service and AI keys belong in Supabase Edge Function Secrets only.

## How to test

Use `my_first_agent/runtime/test-plan.md` as the runtime checklist. The first implementation checkpoint is the `agent` Edge Function readiness response; database and website checks come after their feature groups are built.

## What’s next

Before implementation, confirm the first feature group and Supabase project connection. Build one feature group at a time, beginning with the trip-requirements record, validation, and private-data access rules.

## How we take turns

Pull the latest repository changes before starting, tell the other teammate before working, and use GitHub Desktop to commit and push when the work is ready. Never put secrets or real personal data in the repository.
