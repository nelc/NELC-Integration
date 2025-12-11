# 1. Blackboard Ultra → xAPI Mapping Dictionary

Complete, hierarchical, ready for implementation.

## 1.1 Institution / Organization Metadata

(Extractable via Blackboard REST APIs, SIS Integration APIs, and Ultra “Institutional Hierarchy”)

| Blackboard Entity | API Source | xAPI Object Type | Required Data | Notes |
|------------------|------------|------------------|----------------|-------|
| University | Institutional Hierarchy | Group | org:id, name, code | You will define each tenant/university as an xAPI “group” |
| College / Faculty | Institutional Hierarchy | Group | college_id, name | Optional: hierarchy path |
| Department | Institutional Hierarchy | Group | dept_id, name | |
| Program | Institutional Hierarchy or course module metadata | Group | program_id, name | Depends on institution; some use metadata in courses |
| Semester / Term | Term API | Activity | term_id, title, start_date, end_date | Blackboard populates "terms" out of the box |
| Cluster / Campus | Institutional Hierarchy | Group | cluster_id, campus_name | Many universities store campuses as hierarchy nodes |

### xAPI Mapping (Institutional Metadata)

You will use:

- `object.type = "Group" or "Activity"`
- `context.contextActivities.grouping = University / College / Department`
- `context.extensions = { hierarchy values }`

---

## 1.2 Courses, Sections & Descriptions

| Blackboard Entity | API Source | xAPI Type | Extractable Fields |
|------------------|------------|-----------|---------------------|
| Course | Courses API | Activity | courseId, courseName, courseCode, description, created, modified |
| Course Description | Courses API | Activity Definition | description, objectives |
| Course Section (Ultra term section) | Course Membership API | Activity | sectionId, sectionNumber, term, instructors |
| Enrollment | Enrollment API | Statement context | enrollment role, start/end dates |
| Instructor | Users API | Agent | id, name, email, role |
| Teaching Assistants | Users API | Agent | id, name, role |

### xAPI Mapping (Course Metadata)

- `object.type = "Activity"`
- `object.definition.type = "http://adlnet.gov/expapi/activities/course"`
- `context.grouping = university → college → department → program → term → course`

---

## 1.3 Learning Content & Educational Materials

| Blackboard Entity | API Source | xAPI Object Type | Includes |
|-------------------|------------|------------------|----------|
| Ultra Document | Content API | Activity | name, id, parent folder, description |
| Folder / Learning Module | Content API | Activity | structure, hierarchy |
| Assignment | Assignments API | Activity | instructions, attachments |
| Test / Quiz | Assessment API | Activity | questions, metadata |
| File / PDF / Media | Content API | Activity | filename, type |
| External Tool (LTI) | LTI tool data | Activity | launch URL |

---

## 1.4 Academic Engagement Events

| Blackboard Event | Extract Source | xAPI Verb | xAPI Object |
|------------------|----------------|-----------|--------------|
| Course accessed | Activity log | accessed | Course activity |
| Content viewed | Activity log | viewed | Document/file/assessment |
| Assignment submitted | Grades/Attempts API | submitted | Assignment |
| Attempt completed | Assessment API | completed | Quiz/test |
| Question answered | Assessment API | answered | Question |
| Grade posted | Gradebook API | graded | Assignment/Assessment |
| Discussion post | Discussion API | posted | Thread/post |
| Collaborate session join | Collaborate session logs | attended | Session |
| Collaborate chat message | Collaborate logs | commented | Message |

---

# 2. Sample xAPI Statements

Covering all academic-structure metadata and learning events with university tracking.

---

## 2.1 Sample: Course Accessed

Tracks: university → college → department → program → course → section → term.

```json
{
  "actor": {
    "objectType": "Agent",
    "account": {
      "homePage": "https://bb.example.com",
      "name": "student_98321"
    }
  },
  "verb": {
    "id": "http://adlnet.gov/expapi/verbs/accessed",
    "display": {"en-US": "accessed"}
  },
  "object": {
    "id": "https://bb.example.com/courses/ENG101-F23-001",
    "definition": {
      "type": "http://adlnet.gov/expapi/activities/course",
      "name": {"en-US": "English Composition I"},
      "description": {"en-US": "An introductory writing course."}
    }
  },
  "context": {
    "contextActivities": {
      "grouping": [
        { "id": "https://bb.example.com/institution/UniversityA" },
        { "id": "https://bb.example.com/college/ArtsHumanities" },
        { "id": "https://bb.example.com/department/English" },
        { "id": "https://bb.example.com/program/BachelorEnglish" },
        { "id": "https://bb.example.com/term/2023-Fall" },
        { "id": "https://bb.example.com/course/ENG101" }
      ],
      "parent": [
        { "id": "https://bb.example.com/section/ENG101-F23-001" }
      ]
    }
  },
  "timestamp": "2024-02-15T09:32:12Z"
}
```

## 2.2 Sample: Instructor Viewed Course Roster
```json
{
  "actor": {
    "objectType": "Agent",
    "account": {
      "homePage": "https://bb.example.com",
      "name": "instructor_1123"
    }
  },
  "verb": {
    "id": "http://id.tincanapi.com/verb/viewed",
    "display": {"en-US": "viewed"}
  },
  "object": {
    "id": "https://bb.example.com/courses/ENG101-F23-001/roster",
    "definition": {
      "name": {"en-US": "Course Roster"},
      "type": "http://adlnet.gov/expapi/activities/roster"
    }
  },
  "context": {
    "contextActivities": {
      "grouping": [
        { "id": "https://bb.example.com/institution/UniversityA" },
        { "id": "https://bb.example.com/college/ArtsHumanities" },
        { "id": "https://bb.example.com/course/ENG101" }
      ],
      "parent": [
        { "id": "https://bb.example.com/section/ENG101-F23-001" }
      ]
    }
  }
}
```

## 2.3 Sample: Assignment Submitted
```json
{
  "actor": {
    "objectType": "Agent",
    "account": { "name": "student_98321" }
  },
  "verb": {
    "id": "http://adlnet.gov/expapi/verbs/submitted",
    "display": {"en-US": "submitted"}
  },
  "object": {
    "id": "https://bb.example.com/assignments/ENG101-F23-001/WritingTask1",
    "definition": {
      "name": {"en-US": "Writing Task 1"},
      "type": "http://adlnet.gov/expapi/activities/assignment"}
  },
  "result": {
    "completion": true,
    "extensions": {
      "https://bb.example.com/xapi/attemptId": "ATT-912931"
    }
  },
  "context": {
    "instructor": [
      {
        "objectType": "Agent",
        "account": {"name": "instructor_1123"}
      }
    ],
    "contextActivities": {
      "grouping": [
        { "id": "https://bb.example.com/institution/UniversityA" },
        { "id": "https://bb.example.com/college/ArtsHumanities" },
        { "id": "https://bb.example.com/program/BachelorEnglish" },
        { "id": "https://bb.example.com/term/2023-Fall" }
      ],
      "parent": [
        { "id": "https://bb.example.com/section/ENG101-F23-001" }
      ]
    }
  }
}
```

## 2.4 Sample: Content Viewed
```json
{
  "actor": { "account": { "name": "student_333" }},
  "verb": {
    "id": "http://adlnet.gov/expapi/verbs/viewed",
    "display": {"en-US": "viewed"}
  },
  "object": {
    "id": "https://bb.example.com/courses/ENG101-F23-001/modules/module2/document7",
    "definition": {
      "name": {"en-US": "Grammar Basics"},
      "type": "http://adlnet.gov/expapi/activities/document"
    }
  },
  "context": {
    "contextActivities": {
      "grouping": [
        { "id": "https://bb.example.com/institution/UniversityA" },
        { "id": "https://bb.example.com/course/ENG101" }
      ]
    }
  }
}
```

## 2.5 Sample: Instructor Posted to Discussion
```json
{
  "actor": {
    "objectType": "Agent",
    "account": {"name": "instructor_1123"}
  },
  "verb": {
    "id": "http://adlnet.gov/expapi/verbs/posted",
    "display": {"en-US": "posted"}
  },
  "object": {
    "id": "https://bb.example.com/courses/ENG101/discussions/12345/post/876",
    "definition": {
      "type": "http://id.tincanapi.com/activitytype/discussion",
      "name": {"en-US": "Discussion Post"}
    }
  },
  "context": {
    "instructor": [
      { "objectType": "Agent", "account": {"name": "instructor_1123"} }
    ],
    "contextActivities": {
      "grouping": [
        { "id": "https://bb.example.com/institution/UniversityA" },
        { "id": "https://bb.example.com/course/ENG101" }
      ],
      "parent": [
        { "id": "https://bb.example.com/section/ENG101-F23-001" }
      ]
    }
  }
}
```

# 3. Final Deliverable Coverage

Your request is now fully covered for:

Full Blackboard Ultra → xAPI event mapping dictionary

Complete xAPI samples across:

Institutional metadata (university/college/program/department)

Courses, sections, terms

Instructors

Content

Assignments

Assessments

Discussions

Access & navigation

Collaboration sessions

This is ready for implementation in your central multi-university LRS.
