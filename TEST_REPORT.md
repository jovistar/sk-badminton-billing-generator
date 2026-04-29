# Test Report

## Scope

Test target: `SKILL.md`

This project is a Codex skill definition, not an executable application. Tests were performed as rule-level simulations against the billing calculation and output rules.

## Functional Tests

| ID | Scenario | Input Summary | Expected Result | Status |
| --- | --- | --- | --- | --- |
| F-01 | Default court duration, court price, ball type, and ball price | 18 attendees, 3 courts, 30 balls | Total 1102.50, attendee fee 61.25, receivable total 1041.25 | Pass |
| F-02 | Multiple court groups | 3 courts x 3h, 1 court x 1h, 21 attendees, 42 balls | Total 1387.50, attendee fee 66.08, receivable total 1321.60 | Pass |
| F-03 | Absence cost split | 10 attendees, 2 absentees, 2 courts x 3h, 20 balls | Absent fee 30.00, attendee fee 67.50, receivable total 667.50 | Pass |
| F-04 | Proxy registration groups | Attending 1-person groups: 5, attending 2-person groups: 3, 2 courts x 2h, 10 balls | Attendees 11, attendee fee 38.87, receivable total 388.70 | Pass |
| F-05 | Multiple ball types | 8 attendees, 1 court x 3h, red 10 balls, Ya3 6 balls | Total 452.52, attendee fee 56.57, receivable total 395.99 | Pass |
| F-06 | Organizer-only edge case | 1 attendee, 1 court x 3h, 1 ball | Total 198.75, attendee fee 198.75, receivable total 0.00, non-empty payment blocks | Pass |

## Exception Tests

| ID | Scenario | Expected Behavior | Status |
| --- | --- | --- | --- |
| E-01 | Missing required court count | List missing required field and do not calculate | Pass |
| E-02 | Missing required ball count | List missing required field and do not calculate | Pass |
| E-03 | Unknown ball type without unit price | Ask for that ball type's unit price and do not calculate | Pass |
| E-04 | Attendance count is 0 | Reject calculation because attendee count must be greater than 0 | Pass |
| E-05 | Summary count conflicts with registration groups | Use registration groups for calculation, summary only for validation, ask user to fix mismatch | Pass |
| E-06 | Organizer attends but attending 1-person group is 0 | Ask user to fix grouping conflict and do not move organizer into another group | Pass |
| E-07 | Non-positive court count, duration, price, ball count, or ball price | Reject invalid value and ask user to correct it | Pass |

## Delivery Tests

| ID | Check | Expected Behavior | Status |
| --- | --- | --- | --- |
| D-01 | Output block count | Always output three plain-text code blocks | Pass |
| D-02 | Output block order | Bill text first, collection text second, total payment text third | Pass |
| D-03 | Scene labels | Do not output labels such as "bill text", "collection text", or "total payment text" | Pass |
| D-04 | Money format | All output money values keep 2 decimal places | Pass |
| D-05 | Total payment | Third block uses `总付款额：金额元` and equals the sum of all receivable amounts in the second block | Pass |
| D-06 | Empty receivable edge case | Output `无应收款：0.00元` in the second block and `总付款额：0.00元` in the third block | Pass |
| D-07 | Line ending style | In each block, every non-final line ends with ` \|` | Pass |

## Fixes Applied During Testing

- Added a global output-money rule: all output amounts must keep 2 decimal places.
- Added a no-receivable fallback: if every registration type has receivable amount 0, output `无应收款：0.00元`.
- Fixed the total-payment block format as `总付款额：金额元`.

## Result

Status: Pass after fixes.
