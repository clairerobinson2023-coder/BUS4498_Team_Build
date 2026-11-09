# Verify Budget and Schedule Task Specification

## Basic Information

- **Task ID:** T6
- **Task name:** Verify Budget and Schedule
- **Task type:** Verify
- **Task owner:** TripSync system

## 1. Task Description

The system uses defined calculations to total estimated costs and detect obvious conflicts, such as overlapping events, activities outside travel dates, or activities that exceed available daily time blocks. It reports warnings rather than silently changing the itinerary.

## 2. Inputs

### Input 1

- **Input name:** Draft itinerary
- **Contents and format:** Day-by-day itinerary with time blocks and estimated costs.
- **Source:** T5 Compose Draft Itinerary.

### Input 2

- **Input name:** Validated travel requirements
- **Contents and format:** Traveler budget, travel dates, and scheduling constraints.
- **Source:** T2 Validate Trip Requirements.

- **If a required input is missing or invalid:** Return the case to T5 Compose Draft Itinerary or route it to T3 Resolve Requirement Gaps.

## 3. Outputs

### Output 1

- **Output name:** Budget and schedule check
- **Contents and format:** Estimated total, budget variance, conflict list, and pass-with-warnings or needs-revision status.
- **Next task or recipient:** T7 Review and Approve Itinerary.
- **Complete when:** Every selected item has been included in the cost total and evaluated against the stated dates and time constraints.

## 4. Planned Tools

### Tool 1

- **Tool name:** check_budget_and_schedule
- **Input:** Draft itinerary and validated travel requirements
- **Output:** Budget and schedule check
- **Implementation Route:** Functions/scripts.
- **Integration approach:** Direct integration.
- **Role in this task:** Performs cost totals, variance calculation, date comparisons, and overlap checks using stated rules.
- **Task timeout:** 45 seconds.
- **Maximum retries:** 1.
- **Retry only when:** A temporary itinerary-record read fails; retry once after five seconds.
- **On timeout, exhausted retries, or an error that cannot be retried:** Mark checks unavailable and send the itinerary to the TripSync support reviewer for manual verification.
