# Auto Validation Checklist

## 1 Statement Coverage and Sequence
1. Ensure a **registration statement** is sent for every learning journey.
2. Ensure **all required statements are received**, covering the full lifecycle (from *registered* through *earned*).
3. Ensure **no duplicate statements** are sent within the same course.
4. Ensure **progress statements exist** and their values **increase monotonically over time**.
5. Ensure an **attempt statement exists for each exam attempt**.
6. Ensure **attempt IDs increment sequentially by 1** for each exam attempt.
7. Ensure All object IDs for course, lesson, video, module, etc must follow the naming convention below:
8. Ensure **register** and **intialize** events are not sent simultaneously at the same time.
`Ex: http://www.lmsname.com/course/CR001/module/MD003/lesson/123423`



## 2. Course, Object, and Relationship Integrity
2.1 Ensure the **course ID is consistent** across all statements and matches all `parent.id` references.
2.2 Ensure the **parent object type is always the course**, where applicable.
2.3 Ensure the **object type is correct** for each verb:
   - `watch` → `video`
   - `complete` → `lesson`
   - `attempt` → `unit-test`
   - etc.
2.4 Ensure **course, lesson, video, and virtual classroom objects include a valid duration**.
2.5 Ensure **every course includes a non-empty description** in the *registered* statement.
2.6 Ensure content description is not the same string as the content name.
2.7 Ensure course level statements such as `initialize` and `progress` have empty description except for register statement. (where it is mandatory)
The same goes for the object under `parent` key.
2.8 Ensure **course name and description do not contain HTML tags**.

## 3. Context and Extension Rules
3.1 Ensure **course description appears only in the registration statement**.
3.2 Ensure **context.extensions appear only in the registration statement**.
3.3 Ensure **all mandatory extensions are present and populated** in the registration statement.
3.4 Ensure **`lms_url` is included in the registration statement**.
3.5 Ensure **`program_url` is included in the registration statement**.
3.6 Ensure the **platform key is present in all statements** and matches the agreed value.
3.7 Ensure the **full platform official name is provided**, not an abbreviation. It is under:
     - `context.extensions.https://nelc.gov.sa/extensions/platform.name.ar-SA`
     - `context.extensions.https://nelc.gov.sa/extensions/platform.name.en-US`
3.8 Ensure `"context.language"` matches the object language (e.g., `ar-SA`, `en-US`).
3.9 Ensure **all localized values are placed under the correct language keys** (`en-US`, `ar-SA`).

## 4. Learner and Identity Validation
4.1 Ensure the **learner’s National ID is valid and correctly formatted**.

## 5. Assessment, Scoring, and Results
4.1 Ensure **attempt statements include `result.score.min`**.
4.2 Ensure `result.success = true` **when `result.score.raw` is greater than or equal to `result.score.min`**.
4.3 Ensure **rating statements have a score between 0 and 1**.
4.4 Ensure **rating statements include a textual review response**.
4.5 During the staging integration test, lms must send multiple attemps for a specific learner attempting the same test/quiz to allow data validation.

## 6. Certification Rules
6.1 Ensure **certificate ID and certificate URL are distinct fields**.
6.2 Ensure the **learner certificate represents an instance copy**, not the master certificate object.
6.3 Certificate url link must be publicly accessible link. (without the need for lms login)
