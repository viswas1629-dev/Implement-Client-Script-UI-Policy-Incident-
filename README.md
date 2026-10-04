# Implement Client Script & UI Policy (Incident)

A ServiceNow project that improves the **Incident Management** process by using **UI Policies** and **Client Scripts** to control how Incident records are created, updated and saved.

## Team Details

| | |
|---|---|
| **Team ID** | SWTID-2026-4732 |
| **Team Size** | 4 |
| **Team Leader** | Karuppa Sami Kasi Visvanathan S |
| **Team Members** | Bala M, Saifur Rahuman Z, Karthik T |
| **Date** | 30 September 2026 |

## Overview

In the existing Incident process, users enter information manually. This can lead to incomplete or inconsistent data and affect incident triage, routing, reporting accuracy and service quality.

This project adds automated form-level business rules on the **Incident** table so that High Impact Incidents follow the defined rules, invalid or unauthorized updates are prevented, and users get immediate feedback.

## Project Phases

| Phase | Activity | Component |
|---|---|---|
| 1 | Implementing High Impact Controls | UI Policy: **High Impact Control** |
| 2 | Controlling Urgency | UI Policy Action for High Impact Incidents |
| 3 | Automating Urgency Updates | onChange Client Script: **Auto set urgency for high impact** |
| 4 | Preventing Invalid Submissions | onSubmit Client Script: **Prevent save if Assigned To missing** |
| 5 | Protecting Incident State | onCellEdit Client Script: **Prevent state change via list edit** |
| 6 | Validating the Solution | Testing of all UI Policies and Client Scripts |

## Features

- **Mandatory field control:** Assignment group becomes mandatory when Impact is `1 - High`. The *Reverse if false* option undoes this when the condition no longer applies.
- **Urgency control:** A UI Policy Action manages the Urgency field for High Impact Incidents.
- **Automatic Urgency update:** Changing Impact to `1 - High` sets Urgency to `1 - High` and shows an informational message.
- **Save-time validation:** A High Impact Incident cannot be saved while **Assigned To** is empty. An error message is shown.
- **List edit protection:** The State field cannot be changed directly from the Incident list. An alert asks the user to open the Incident instead, and the callback returns `false`.

## Implementation Summary

All components are created on the **Incident** table (`incident`).

### UI Policy: High Impact Control
- **Condition:** Impact is `1 - High`
- **Action:** Assignment group set to mandatory
- **Reverse if false:** enabled

### Client Scripts

| Name | Type | Field / Trigger | Behavior |
|---|---|---|---|
| Auto set urgency for high impact | onChange | Impact | Sets Urgency to `1 - High` and shows an info message |
| Prevent save if Assigned To missing | onSubmit | Form save | Shows an error and stops the save if Impact is High and Assigned To is empty |
| Prevent state change via list edit | onCellEdit | State | Shows an alert and returns `false` to block the list edit |

## Testing

Phase 6 validates the full configuration with these scenarios:

| # | Scenario | Expected Result |
|---|---|---|
| 1 | Create a High Impact Incident with **Assigned To** empty | Save is blocked and an error message is shown |
| 2 | Populate **Assigned To** and save | Incident saves successfully |
| 3 | Change Impact from High to Medium (reverse condition) | Assigned To is no longer mandatory and Urgency is editable again |
| 4 | Edit **State** directly from the Incident list | Change is blocked and an alert is shown |
| 5 | Change **State** from the Incident form | Update is allowed |

## How to Reproduce

1. Open a ServiceNow instance and go to **Incident > All**.
2. Create the UI Policy **High Impact Control** and its actions (Phases 1 and 2).
3. Create the three Client Scripts from the table above (Phases 3, 4 and 5).
4. Run the five test scenarios (Phase 6) and confirm the expected results.

## Documentation

Each phase has its own document (`Phase_1.docx` to `Phase_6_.docx`) with the problem statement, user story, proposed solution and screenshots.

## Outcome

- Mandatory fields are enforced for High Impact Incidents
- Related field updates are automated
- Invalid and unauthorized changes are prevented
- Less manual checking and better data consistency, integrity and Incident handling quality
