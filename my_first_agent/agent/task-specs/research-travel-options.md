# Research Travel Options Task Specification

```yaml
task_id: "T4"
task_name: "Research Travel Options"
task_owner: "TripSync travel-planning agent"
```

## 1. Task Goal

- **Objective:** Produce a supported and comparable set of transportation, lodging, activity, and restaurant options that fits the traveler’s validated requirements. The agent may choose which permitted category or evidence gap to investigate next, but it may not book, purchase, change reservations, or use an account requiring traveler credentials.

## 2. Inbound Inputs

### Input 1

- **Input name:** Validated travel requirements
- **What it contains:** A structured trip record with destination, departure location, travel dates, number of travelers, total budget, transportation needs, lodging preferences, interests, dietary or accessibility needs, and constraints. It has a passed validation status.
- **Source:** T2 Validate Trip Requirements.

### Input 2

- **Input name:** Research scope
- **What it contains:** Permitted public travel sources, option categories, and the maximum number of alternatives to include per category.
- **Source:** TripSync system configuration.

## 3. Tool Permissions and Boundaries

### Task-Wide Limits

- **Total task timeout:** Six minutes.
- **Maximum tool calls:** Nine total calls across all tools, including retries.

### Tool 1

- **Tool name:** retrieve_travel_options
- **Tool type:** API request or web retrieval.
- **Supports these permitted subtasks:** Identify evidence gaps; retrieve category options; verify option details.
- **Allowed use:** Read current publicly available flight, lodging, local transportation, activity, and restaurant information from permitted sources. Retrieve dates, estimated prices, locations, availability notes, restrictions, and source links for the traveler’s requested destination and dates.
- **Prohibited use:** Log in to traveler accounts, submit bookings, make purchases, alter reservations, send provider messages, access payment information, or retrieve information outside the stated trip scope.
- **Approval required:** None within the allowed use.
- **Timeout per call:** 30 seconds.
- **Maximum retries per call:** 1.
- **Retry conditions and failure response:** Retry once after five seconds only for a temporary retrieval or network error. If the second attempt fails, record the source as unavailable and choose another permitted source or hand off the evidence gap.

### Tool 2

- **Tool name:** compare_travel_options
- **Tool type:** Language-model call.
- **Supports these permitted subtasks:** Compare alternatives; prepare research set.
- **Allowed use:** Compare retrieved options against the validated dates, budget, stated preferences, constraints, and evidence gaps. Produce concise, attributable comparisons and identify tradeoffs or missing evidence.
- **Prohibited use:** Invent prices, availability, reviews, source links, or personal preferences; make bookings or commitments; send messages; override traveler requirements.
- **Approval required:** None within the allowed use.
- **Timeout per call:** 45 seconds.
- **Maximum retries per call:** 0.
- **Retry conditions and failure response:** No retry. Record the comparison as unavailable and hand the research set to the TripSync support reviewer if a usable comparison cannot be produced from the retrieved evidence.

## 4. How the Agent Should Reason

### Permitted Subtask 1

- **Subtask name:** Identify evidence gaps
- **Subtask description:** Examine the validated requirements and current research set to determine which required category, date, price, or constraint lacks enough evidence to support an option.
- **Subtask boundary:** May identify missing or conflicting evidence only. It may not add requirements, assume traveler preferences, or contact a provider.
- **Retry limits:** 0.

### Permitted Subtask 2

- **Subtask name:** Retrieve category options
- **Subtask description:** Retrieve a small set of options for the most important unresolved category, such as transportation or lodging, using permitted sources.
- **Subtask boundary:** May read only information within the traveler’s trip scope and the approved sources. It may not transact, log in, or collect payment data.
- **Retry limits:** 1 additional attempt, subject to the tool and task-wide limits.

### Permitted Subtask 3

- **Subtask name:** Verify option details
- **Subtask description:** Check that an option has usable dates, estimated price, location, and a source link before it can be included in the research set.
- **Subtask boundary:** May verify retrieved details only; it must label unavailable or conflicting information rather than infer it.
- **Retry limits:** 1 additional attempt, subject to the tool and task-wide limits.

### Permitted Subtask 4

- **Subtask name:** Compare alternatives
- **Subtask description:** Compare supported options against the traveler’s requirements and identify tradeoffs, such as lower cost versus longer travel time or preferred location versus lower availability.
- **Subtask boundary:** May rank or group alternatives based on available evidence, but may not claim an option is guaranteed, select a final plan, or replace traveler approval.
- **Retry limits:** 0.

### Permitted Subtask 5

- **Subtask name:** Prepare research set
- **Subtask description:** Assemble the supported options, source links, estimated costs, availability notes, evidence gaps, and comparison notes for the next task.
- **Subtask boundary:** May include only retrieved or clearly labeled unknown information. It may not generate a full itinerary or carry out a purchase.
- **Retry limits:** 0.

- **Decision guidance:** After each subtask, use its findings to choose the permitted subtask most likely to resolve the most important remaining uncertainty. Prioritize date-bound transportation and lodging before optional activities and restaurants. If no permitted subtask can make useful progress, stop and hand the case to a person.

## 5. When to Stop or Hand Off to a Human

- **Stop successfully when:** The research set includes supported alternatives for transportation and lodging, plus available activity and restaurant options when requested, with source links, estimated prices or an explicit unavailable-price note, and known evidence gaps.
- **Hand off early when:** Required destination or date information is unclear; no permitted source can provide supported core options; retrieved sources materially conflict; a source requires login, payment, or a booking action; or the call/time limit is reached before a usable research set is produced.
- **Hand off to:** Traveler through T3 Resolve Requirement Gaps, or the TripSync support reviewer for a technical or source-access issue.

Stop at the first applicable budget limit or handoff condition. While awaiting review, take no further autonomous action.

## 6. Outbound Deliverable

- **Status:** Completed or escalated to human.
- **Result or recommendation:** A comparable research set of travel options, or undetermined if escalated before supported options could be produced.
- **Evidence summary:** Source links, retrieval times, dates, estimated prices, availability notes, and the key tradeoffs used in the comparison.
- **Subtasks performed:** Permitted subtasks completed, including repeated retrieval attempts.
- **Unresolved issues:** Missing, conflicting, or inaccessible evidence; use none only if no unresolved issue remains.
- **Handoff note:** Reason for stopping, unresolved questions, and what the traveler or reviewer needs to decide; write Not applicable for a completed task.
- **Next task or recipient:** T5 Compose Draft Itinerary when completed; T3 Resolve Requirement Gaps or TripSync support reviewer when escalated.
