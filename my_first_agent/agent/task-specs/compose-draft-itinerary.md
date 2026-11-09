# Compose Draft Itinerary Task Specification

## Basic Information

- **Task ID:** T5
- **Task name:** Compose Draft Itinerary
- **Task type:** Reason
- **Task owner:** TripSync system

## 1. Task Description

The system uses a fixed itinerary template and stated scheduling rules to place supported transportation, lodging, activities, and restaurant options into a day-by-day draft. It keeps options within the traveler’s dates and identifies any unresolved timing assumptions. It does not book, purchase, or contact providers.

## 2. Inputs

### Input 1

- **Input name:** Validated travel requirements
- **Contents and format:** Validated structured trip record including dates, budget, preferences, interests, and constraints.
- **Source:** T2 Validate Trip Requirements.

### Input 2

- **Input name:** Travel option research set
- **Contents and format:** Supported transportation, lodging, activity, and restaurant options with dates, estimated prices, locations, source links, and availability notes.
- **Source:** T4 Research Travel Options.

- **If a required input is missing or invalid:** Send the case to T3 Resolve Requirement Gaps.

## 3. Outputs

### Output 1

- **Output name:** Draft itinerary
- **Contents and format:** Day-by-day schedule with selected option identifiers, time blocks, locations, estimated costs, and notes about uncertain timing.
- **Next task or recipient:** T6 Verify Budget and Schedule.
- **Complete when:** Every trip day has a planned transportation and lodging arrangement or a clearly labeled unresolved item.

## 4. Planned Tools

### Tool 1

- **Tool name:** compose_draft_itinerary
- **Input:** Validated travel requirements and travel option research set
- **Output:** Draft itinerary
- **Implementation Route:** Language-model call.
- **Integration approach:** Direct integration.
- **Role in this task:** Applies the fixed itinerary template and explicit scheduling constraints to organize the research set into a readable draft.
- **Task timeout:** 90 seconds.
- **Maximum retries:** 1.
- **Retry only when:** A transient model-service error occurs; retry once after ten seconds with the same inputs.
- **On timeout, exhausted retries, or an error that cannot be retried:** Record draft-generation failure and route the case to the TripSync support reviewer.
