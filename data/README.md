# data/

Synthetic dataset for the Smart University Course Advisor. It is **fake but realistic**: no real
student, instructor or course information is used. State clearly in the report that the data is synthetic.

```
data/
├── raw/         # the 7 CSV files below (ready to use, already clean)
└── processed/   # outputs of your own cleaning / feature scripts
```

## Files in `raw/`

| File | Rows | What it contains |
|---|---|---|
| `courses.csv` | 52 | Course catalog (43 core + 9 electives) across 4 levels |
| `prerequisites.csv` | 53 | Which course must be passed before another one |
| `instructors.csv` | 20 | Instructors (anonymous, numbered) |
| `sections.csv` | 70 | Timetable of the **target semester, Spring-2027**: days, times, room, capacity |
| `students.csv` | 1,400 | One row per student (4 admission years) |
| `grades.csv` | 27,018 | Academic history: every course attempt from Fall-2023 to Fall-2026 |
| `preferences.csv` | 1,400 | Each student's scheduling preferences for Spring-2027 |

Full column descriptions: [`../docs/DATA_SCHEMA.md`](../docs/DATA_SCHEMA.md).

## The story behind the data

- The advisor plans the **next semester: Spring-2027**. History is complete through Fall-2026.
- Students come from four cohorts. `current_level` is the level they are entering in Spring-2027:

| Admission year | Students | Terms completed | Level in Spring-2027 |
|---|---|---|---|
| 2023 | 280 | 7 | 4 |
| 2024 | 340 | 5 | 3 |
| 2025 | 400 | 3 | 2 |
| 2026 | 380 | 1 | 1 |

- Students follow the study plan term by term. Some **fail and retake** courses (661 retake attempts),
  so some students are behind their nominal level. That makes the eligibility problem more interesting.
- A course is **eligible** for a student in Spring-2027 when: it has sections in `sections.csv`, the student has not
  passed it yet, and the student has passed all its prerequisites. On average a student has about 10 eligible
  courses (between 1 and 17).

## Conventions

- **Days:** `Sun;Mon;Tue;Wed;Thu` (semicolon-separated when a section meets on several days).
- **Times:** 24-hour `HH:MM`. A 3-credit course meets twice a week (1.5 h each); a 2-credit course meets once (2 h).
  Time slots are not perfectly aligned between 1.5 h and 2 h courses, so partial overlaps happen (good for testing conflict detection).
- **Semesters:** `Fall-2023`, `Spring-2024`, ... (Fall term starts in September).
- **Grades:** `score` is 0-100; `grade_points` uses the scale below; a course is **passed** when `grade_points >= 1.0`.

| Score | Letter | Grade points |
|---|---|---|
| 93-100 | A | 4.0 |
| 90-92 | A- | 3.7 |
| 87-89 | B+ | 3.3 |
| 83-86 | B | 3.0 |
| 80-82 | B- | 2.7 |
| 77-79 | C+ | 2.3 |
| 73-76 | C | 2.0 |
| 70-72 | C- | 1.7 |
| 67-69 | D+ | 1.3 |
| 60-66 | D | 1.0 |
| below 60 | F | 0.0 |

- **Preferences:** `avoid_early_morning = True` means the student wants to avoid classes that **start before 10:00**;
  `avoid_late_evening = True` means avoid classes that **end after 16:00**.

## How the data was generated (for the report)

A statistical simulation, not real records:
1. Each student gets hidden abilities (general, math, programming) plus a study-effort level.
2. Each course has a difficulty and a mix of the skills it needs (for example Statistics leans on math, Machine Learning on math and programming).
3. A student's score in a course = a base level + their relevant ability + effort - course difficulty + random noise.
4. Students register term by term following the curriculum, only after passing prerequisites, and retake courses they failed.
5. `high_school_score` and `study_hours_per_week` are noisy hints about ability and effort (they are *not* the hidden values themselves).

So the Learning component can find real patterns (for example prerequisite grades, high school score and study
hours help predict a grade), but those patterns come from the simulation, not from real students.

## Checks already done
- No missing values and no duplicate attempts.
- Every foreign key matches (grades -> students/courses, sections -> courses/instructors, ...).
- No student took a course before passing its prerequisites.
- No instructor or room is double-booked in `sections.csv`.
- Overall pass rate about 95%, average score about 78, average GPA about 2.3.

## Tips
- **Avoid data leakage** when predicting a grade: build features only from terms *before* the course you predict.
  `gpa_so_far` in `students.csv` includes all history up to Fall-2026.
- Start with a small slice (one student, a few courses) to make the pipeline work, then scale up.
- Quick peek (Python):
  ```python
  import pandas as pd
  courses = pd.read_csv("data/raw/courses.csv")
  print(courses.head())
  ```
