# Auto Validation Checklist

## 1. Statement Coverage and Sequence
1. Ensure a **registration statement** is sent for every learning journey.
2. Ensure **all required statements are received**, covering the full lifecycle (from *registered* through *earned*).
3. Ensure **no duplicate statements** are sent within the same course.
4. Ensure **progress statements exist** and their values **increase monotonically over time**.
5. Ensure an **attempt statement exists for each exam attempt**.
6. Ensure **attempt IDs increment sequentially by 1** for each exam attempt.

## 2. Course, Object, and Relationship Integrity
7. Ensure the **course ID is consistent** across all statements and matches all `parent.id` references.
8. Ensure the **parent object type is always the course**, where applicable.
9. Ensure the **object type is correct** for each verb:
   - `watch` → `video`
   - `attempt` → `unit-test`
   - etc.
10. Ensure **lesson, video, and virtual classroom objects include a duration**.
11. Ensure **every course includes a non-empty description** in the *registered* statement.
12. Ensure **course name and description do not contain HTML tags**.

## 3. Context and Extension Rules
13. Ensure **course description appears only in the registration statement**.
14. Ensure **context.extensions appear only in the registration statement**.
15. Ensure **all mandatory extensions are present and populated** in the registration statement.
16. Ensure **`lms_url` is included in the registration statement**.
17. Ensure **`program_url` is included in the registration statement**.
18. Ensure the **platform key is present in all statements** and matches the agreed value.
19. Ensure the **full platform name is provided**, not an abbreviation.
20. Ensure `"context.language"` matches the object language (e.g., `ar-SA`, `en-US`).
21. Ensure **all localized values are placed under the correct language keys** (`en-US`, `ar-SA`).

## 4. Learner and Identity Validation
22. Ensure the **learner’s National ID is valid and correctly formatted**.

## 5. Assessment, Scoring, and Results
23. Ensure **attempt statements include `result.score.min`**.
24. Ensure `result.success = true` **when `result.score.raw` is greater than or equal to `result.score.min`**.
25. Ensure **rating statements have a score between 0 and 1**.
26. Ensure **rating statements include a textual review response**.

## 6. Certification Rules
27. Ensure **certificate ID and certificate URL are distinct fields**.
28. Ensure the **learner certificate represents an instance copy**, not the master certificate object.
