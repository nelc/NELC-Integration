# Auto Validation Checklist

## 1. Statement Coverage and Sequence
1. Ensure a **registration statement** is sent for every learning journey.
2. Ensure **all required statements are received**, covering the full lifecycle (from *registered* through *earned*).
3. Ensure **no duplicate statements** are sent within the same course.
4. Ensure **progress statements exist** and their values **increase monotonically over time**.
5. Ensure an **attempt statement exists for each exam attempt**.
6. Ensure **attempt IDs increment sequentially by 1** for each exam attempt.
7. Ensure All object IDs for course, lesson, video, module, etc must follow the naming convention below:
`Ex: http://www.lmsname.com/course/CR001/module/MD003/lesson/123423`


## 2. Course, Object, and Relationship Integrity
7. Ensure the **course ID is consistent** across all statements and matches all `parent.id` references.
8. Ensure the **parent object type is always the course**, where applicable.
9. Ensure the **object type is correct** for each verb:
   - `watch` → `video`
   - `complete` → `lesson`
   - `attempt` → `unit-test`
   - etc.
10. Ensure **course, lesson, video, and virtual classroom objects include a valid duration**.
11. Ensure **every course includes a non-empty description** in the *registered* statement.
12. Ensure **course name and description do not contain HTML tags**.

## 3. Context and Extension Rules
13. Ensure **course description appears only in the registration statement**.
14. Ensure **context.extensions appear only in the registration statement**.
15. Ensure **all mandatory extensions are present and populated** in the registration statement.
16. Ensure **`lms_url` is included in the registration statement**.
17. Ensure **`program_url` is included in the registration statement**.
18. Ensure the **platform key is present in all statements** and matches the agreed value.
19. Ensure the **full platform official name is provided**, not an abbreviation. It is under:
     - `context.extensions.https://nelc.gov.sa/extensions/platform.name.ar-SA`
     - `context.extensions.https://nelc.gov.sa/extensions/platform.name.en-US`
21. Ensure `"context.language"` matches the object language (e.g., `ar-SA`, `en-US`).
22. Ensure **all localized values are placed under the correct language keys** (`en-US`, `ar-SA`).

## 4. Learner and Identity Validation
22. Ensure the **learner’s National ID is valid and correctly formatted**.

## 5. Assessment, Scoring, and Results
23. Ensure **attempt statements include `result.score.min`**.
24. Ensure `result.success = true` **when `result.score.raw` is greater than or equal to `result.score.min`**.
25. Ensure **rating statements have a score between 0 and 1**.
26. Ensure **rating statements include a textual review response**.
27. During the staging integration test, lms must send multiple attemps for a specific learner attempting the same test/quiz to allow data validation.

## 6. Certification Rules
27. Ensure **certificate ID and certificate URL are distinct fields**.
28. Ensure the **learner certificate represents an instance copy**, not the master certificate object.
29. Certificate url link must be publicly accessible link. (without the need for lms login)
