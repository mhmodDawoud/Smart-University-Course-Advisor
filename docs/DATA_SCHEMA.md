# Data schema

This is the **actual** schema of the files in `data/raw/` (it replaces the earlier proposal).
Update this file if the team changes the data.

## Tables

### courses.csv
| Column | Type | Meaning |
|---|---|---|
| `course_id` | text | e.g. `CS201` (primary key) |
| `name` | text | Course title |
| `credit_hours` | int | 2 or 3 |
| `level` | int | 1-4 (year in which the course is normally taken; electives are level 3 or 4) |
| `department` | text | `CS`, `AI`, `MATH`, `STAT`, `IS`, `ELE`, ... |
| `semester_offered` | text | `Fall`, `Spring` or `Both` |
| `course_type` | text | `core` or `elective` |

### prerequisites.csv
| Column | Type | Meaning |
|---|---|---|
| `course_id` | text | The course that has a prerequisite |
| `prerequisite_course_id` | text | The course that must be **passed** first |

A course may have several prerequisites (several rows); all of them are required.

### instructors.csv
`instructor_id` (e.g. `INS05`), `name` (anonymous), `department`

### sections.csv (timetable of Spring-2027)
| Column | Type | Meaning |
|---|---|---|
| `section_id` | text | e.g. `CS201-02` (primary key) |
| `course_id` | text | Links to `courses.csv` |
| `semester` | text | Always `Spring-2027` |
| `instructor_id` | text | Links to `instructors.csv` |
| `days` | text | e.g. `Sun;Tue` (the section meets on each of these days) |
| `start_time`, `end_time` | text | `HH:MM`, same for every meeting day |
| `room` | text | e.g. `H105` (lecture hall) or `L203` (lab) |
| `capacity` | int | Seats available |

### students.csv
| Column | Type | Meaning |
|---|---|---|
| `student_id` | text | Anonymous id, e.g. `ST0412` (primary key) |
| `admission_year` | int | 2023-2026 |
| `current_level` | int | Level in Spring-2027 (1-4) by admission year; some students are behind in credits |
| `semesters_completed` | int | Terms with grades (1-7) |
| `high_school_score` | float | Admission score (%) |
| `study_hours_per_week` | int | Self-reported |
| `gpa_so_far` | float | Credit-weighted GPA of the latest attempt of each course, through Fall-2026 |
| `completed_credit_hours` | int | Credit hours of passed courses |

### grades.csv
| Column | Type | Meaning |
|---|---|---|
| `student_id`, `course_id` | text | Links to students / courses |
| `semester` | text | e.g. `Fall-2024` |
| `score` | float | 0-100 |
| `grade_points` | float | 0.0-4.0 (**the main target for the Learning component**) |
| `letter_grade` | text | A ... F |
| `attempt` | int | 1 for the first try, 2+ for retakes |

A course is passed when `grade_points >= 1.0`.

### preferences.csv
| Column | Type | Meaning |
|---|---|---|
| `student_id` | text | Links to students |
| `min_credit_hours`, `max_credit_hours` | int | Wanted credit-hour range for the semester |
| `preferred_days` | text | Days the student prefers to attend, e.g. `Sun;Mon;Wed` |
| `avoid_early_morning` | bool | Avoid classes starting before 10:00 |
| `avoid_late_evening` | bool | Avoid classes ending after 16:00 |

## Relationships

```
students 1--* grades *--1 courses 1--* prerequisites (self reference)
courses  1--* sections *--1 instructors
students 1--1 preferences
```

## Interfaces between components

| From | To | What is passed |
|---|---|---|
| Data | Search | courses, prerequisites, sections, the student's passed courses |
| Data | Learning | grades, students, courses |
| Search | Optimization | list of valid, conflict-free combinations of sections |
| Learning | Optimization | predicted `grade_points` per student and course |
| Optimization | App | best schedule + score + predicted grades |

Write the exact formats (data structures, function signatures) here before coding so everyone builds against the same contract.

## Notes
- The data is **synthetic** (see `data/README.md`); say so in the report.
- Never add real names or national IDs.
