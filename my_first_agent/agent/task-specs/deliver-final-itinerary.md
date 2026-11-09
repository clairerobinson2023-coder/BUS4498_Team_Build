# Deliver Final Itinerary Task Specification

## Basic Information

- **Task ID:** T8
- **Task name:** Deliver Final Itinerary
- **Task type:** Act
- **Task owner:** TripSync system

## 1. Task Description

The system formats the traveler-approved plan into a final itinerary. It includes selected options, estimated totals, source links, warnings, and a note that prices and availability can change. The task only creates and delivers the planning document; it cannot make bookings or purchases.

## 2. Inputs

### Input 1

- **Input name:** Approved plan review packet
- **Contents and format:** Approved draft itinerary, budget and schedule check, and traveler decision status.
- **Source:** T7 Review and Approve Itinerary.

- **If a required input is missing or invalid:** Return the case to T7 Review and Approve Itinerary.

## 3. Outputs

### Output 1

- **Output name:** Final itinerary package
- **Contents and format:** Readable itinerary containing daily schedule, selected options, budget summary, relevant source links, and monitoring explanation.
- **Next task or recipient:** Traveler and T9 Configure Change Monitoring.
- **Complete when:** The package is generated from an approved plan and is available to the traveler.

## 4. Planned Tools

### Tool 1

- **Tool name:** generate_final_itinerary
- **Input:** Approved plan review packet
- **Output:** Final itinerary package
- **Implementation Route:** Language-model call and file operations.
- **Integration approach:** Direct integration.
- **Role in this task:** Fills a fixed final-itinerary template using only approved plan content and saves it for the traveler.
- **Task timeout:** 90 seconds.
- **Maximum retries:** 1.
- **Retry only when:** A temporary document-generation or storage error occurs; retry once after ten seconds.
- **On timeout, exhausted retries, or an error that cannot be retried:** Mark delivery failed and route the approved plan to TripSync support for manual delivery.
