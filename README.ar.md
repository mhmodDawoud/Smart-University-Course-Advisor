# Smart University Course Advisor: Project Explanation

[Arabic version](README.ar.md)

## What is the project about?

Every semester, a student has to choose courses, and the choice comes with constraints:

- The student must have passed the **prerequisites** (the courses required before a given course).
- Courses must not clash in time.
- The student must respect the allowed number of **credit hours**.
- Ideally, the choice should also fit the student's level and preferences.

This project is a **smart registration assistant** that does all of this automatically. It recommends suitable courses to the student and builds the best possible schedule.

## The project uses 3 AI components (a requirement of the course)

| Component        | What does it do?                                                                                                   |
| ---------------- | ------------------------------------------------------------------------------------------------------------------ |
| **Searching**    | Searches for valid course combinations based on prerequisites and timetable availability.                          |
| **Learning**     | Predicts the student's performance (expected grade) from historical academic data.                                 |
| **Optimization** | Picks the best conflict-free schedule based on credit hours, predicted performance, and the student's preferences. |

## A simple example

1. A Level 2 student comes to register. The system looks at the courses the student has already passed.
2. **Search**: it lists the courses the student is allowed to take and groups them into combinations with no time conflicts.
3. **Learning**: it predicts roughly what grade the student will get in each course (for example, it may find that a certain course will be hard for this student).
4. **Optimization**: it picks the best combination: a suitable number of credit hours, good predicted performance, and times close to what the student prefers.
5. The interface shows the recommended schedule and the predicted grades.

## Expected outputs

- Recommended courses the student is eligible for.
- An optimized schedule with no conflicts.
- A prediction of academic performance.

## The pipeline in short

```
Student data + preferences ──┐
Course and timetable data ───┴─► Search ──┐
Historical academic data ──────► Learning ─┴─► Optimization ─► Recommended schedule + predicted grades
```

## Algorithms

The team chooses the algorithms (for example CSP / Backtracking for Search, a Regression or Classification model for Learning, and a Genetic Algorithm for Optimization) and records its choices in the `docs/` folder.

## Repository structure

- `data/`: the data (raw and processed).
- `notebooks/`: experiments and exploration.
- `src/`: the code, split into `data`, `search`, `learning`, `optimization`, `evaluation`, and `app`.
- `tests/`: the tests.
- `results/`: the figures and tables that go into the report.
- `docs/`: the plan, data schema, team workflow, report, and presentation.

Every folder contains a `README.md` explaining what should go in it.

## Grading (out of 20)

- Presentation: 5
- Code running + Results and conclusion + Challenges and proposed solutions: 10
- Questions and Discussion (individual): 5

**The discussion is individual**, so every team member must understand the whole project, not just their own part, and be able to explain their contribution.

## Workflow

See `docs/TEAM_WORKFLOW.md`: one branch per task, a Pull Request, and a review from a teammate before merging into `main`.
