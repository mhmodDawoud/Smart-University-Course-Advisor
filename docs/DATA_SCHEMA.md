# Data schema (proposal)

This is a **starting proposal**. The team should agree on the final version in the first meeting and
update this file whenever it changes.

## Tables

**courses**
`course_id`, `name`, `credit_hours`, `level`, `semester_offered`, `department`

**prerequisites**
`course_id`, `prerequisite_course_id`

**sections** (timetable)
`section_id`, `course_id`, `instructor`, `day`, `start_time`, `end_time`, `room`, `capacity`

**students** (anonymized)
`student_id`, `program`, `level`, `gpa_so_far`

**grades** (historical academic data)
`student_id`, `course_id`, `semester`, `grade_points`

**preferences**
`student_id`, `max_credit_hours`, `preferred_days`, `avoid_early_morning`, `avoid_late_evening`

## Interfaces between components

| From | To | What is passed |
|---|---|---|
| Data | Search | courses, prerequisites, sections, student completed courses |
| Data | Learning | grades and student/course features |
| Search | Optimization | list of valid, conflict-free combinations of sections |
| Learning | Optimization | predicted performance per student and course |
| Optimization | App | best schedule + score + predicted performance |

Write the exact formats (columns, types, file names) here before coding so everyone builds against the same contract.

## Notes on data
- If real data is not available, use synthetic data and state it clearly in the report.
- Never store real names or national IDs.
