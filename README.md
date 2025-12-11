# NELC-Integration
# NELC — xAPI Profiles Guidelines  
---

## Table of Contents
- [Overview](#overview)
- [Concepts, Verbs and Activity](#concepts-verbs-and-activity)
- [Guidelines](#guidelines)
- [Learner Engagement Scenario](#learner-engagement-scenario)
- [Sample Statements](#sample-statements)
- [Notes to Consider and Common Mistakes](#notes-to-consider-and-common-mistakes)
- [Resources](#resources)
- [Contact Us](#contact-us)

---

## Overview
This file explains how to seamlessly integrate an Education Provider with FutureX through xAPI.  
It describes how to author an xAPI Profile using JSON-based semantic triples (subject–predicate–object).

---

## Concepts, Verbs and Activity

### Table 1: Concepts, Verbs and Activity

| Concept Name | Description | Concept Type | IRI |
|--------------|-------------|--------------|-----|
| registered | Indicates the actor is officially enrolled or inducted in an activity. | Verb | http://adlnet.gov/expapi/verbs/registered |
| initialized | Indicates the actor successfully started an activity. | Verb | http://adlnet.gov/expapi/verbs/initialized |
| watched | Actor watched dynamic content; more specific than *experience* or *consume*. | Verb | https://w3id.org/xapi/acrossx/verbs/watched |
| completed | Actor finished the activity normally. | Verb | http://adlnet.gov/expapi/verbs/completed |
| attended | Actor was present at a virtual or physical activity. | Verb | http://adlnet.gov/expapi/verbs/attended |
| progressed | Indicates how much the actor advanced in an activity. | Verb | http://adlnet.gov/expapi/verbs/progressed |
| rated | Actor rated an object. Rating should be in Result.Score. | Verb | http://id.tincanapi.com/verb/rated |
| earned | Actor earned or was awarded the object. | Verb | http://id.tincanapi.com/verb/earned |
| attempted | Actor attempted to access the object. | Verb | http://adlnet.gov/expapi/verbs/attempted |
| attempt-id | Distinguishes between attempts. | Extension | http://id.tincanapi.com/extension/attempt-id |
| browser information | Browser metadata (same as user-agent). | Extension | http://id.tincanapi.com/extension/browser-info |
| jws certificate location | URL of public certificate to verify signature. | Extension | http://id.tincanapi.com/extension/jws-certificate-location |
| unit test | A unit test inside a software project. | ActivityType | http://id.tincanapi.com/activitytype/unit-test |
| lesson | Learning content that may be standalone or course-related. | ActivityType | http://adlnet.gov/expapi/activities/lesson |
| course | Learning content published with a completion goal. | ActivityType | https://w3id.org/xapi/cmi5/activitytype/course |
| video | Recording of visual and audio content. | ActivityType | https://w3id.org/xapi/video/activity-type/video |
| module | Content aggregation level below course. | ActivityType | http://adlnet.gov/expapi/activities/module |
| certificate | Proof a user completed a course. | ActivityType | https://www.opigno.org/en/tincan_registry/activity_type/certificate |
| assessment | Generic competence assessment. | ActivityType | https://w3id.org/xapi/tla/activity-types/assessment |
| school assignment | Student task. | ActivityType | http://id.tincanapi.com/activitytype/school-assignment |
| virtual classroom | Online live classroom session. | ActivityType | https://w3id.org/xapi/virtual-classroom/activity-types/virtual-classroom |
| LMS URL | Learning platform URL. | Extension | https://nelc.gov.sa/extensions/lms_url |
| program URL | Program/course URL. | Extension | https://nelc.gov.sa/extensions/program_url |

---

## Guidelines
- Backend API authentication is mandatory — frontend API authentication not accepted.
- NELC provides endpoint credentials upon formal request.
- Course completion types:
  - Passing unit quiz
  - Passing final course exam
  - Watching course videos (90% consumption = watched)
- "actor.name" **must be learner’s unique ID** (National ID, Iqama, etc.).
- Course **description must be full and clean (no HTML)** in the registration statement.
- Durations must follow **ISO 8601**: `PT1H22M17S`.
- Timestamp format: `2022-01-31T07:18:32.829Z`.
- **Platform format:** PROV-NUM (e.g., GELS-001).
- Language must use **ISO 639-1** codes (e.g., `ar-SA`, `en-US`).
- Certificate link must be inside:  
  `http://id.tincanapi.com/extension/jws-certificate-location`
- Videos are considered watched when 90% of duration is consumed.
- Progress can be explicitly sent using the **progressed** verb.
- All object IDs for course, lesson, video, module, etc must follow the naming convention below:
  - Ex: http://www.lmsname.com/course/CR001/module/MD003/lesson/123423


---

## Learner Engagement Scenario

### Table 2: Learner Engagement Scenario

(A full table is preserved in your file. GitHub renders wide tables horizontally — still readable.)

---

## Sample Statements

All JSON statements are converted into valid Markdown fenced code blocks:

### Statement #1 — **registered**
```jsonc
{
  "actor": {
    "mbox": "mailto:1234567890@gmail.com",
    "name": "1234567890",
    "objectType": "Agent"
  },
  "verb": {
    "id": "http://adlnet.gov/expapi/verbs/registered",
    "display": { "en-US": "registered" }
  },
  "object": {
    "id": "http://www.UDEMY.com/course/CR001",
    "definition": {
      "name": { "en-US": "مادة الرياضيات" },
      "description": { "en-US": "Math-sixth class-First semester" },
      "type": "https://w3id.org/xapi/cmi5/activitytype/course"
    },
    "objectType": "Activity"
  },
  "context": {
    "instructor": {
      "name": "Ibrahim Khalid",
      "mbox": "mailto:ik@example.com"
    },
    "platform": "MGMT-003",
    "language": "ar-SA",
    "extensions": {
      "https://nelc.gov.sa/extensions/duration": "PT30H00M00S",
      "https://nelc.gov.sa/extensions/education_level": "[primary|intermediate|secondary|...]",  // For schools LMSs only
      "https://nelc.gov.sa/extensions/education_year": "[1|2|3|...]",  // For schools LMSs only
      "https://nelc.gov.sa/extensions/semester": "[1|2|3]",  // For schools LMSs only
      "https://nelc.gov.sa/extensions/lms_url": "http://www.lmsname.com",
      "https://nelc.gov.sa/extensions/program_url": "http://www.lmsname.com/course/1234",
      "https://nelc.gov.sa/extensions/learner_mobile_no": "+96655995959559",
      "https://nelc.gov.sa/extensions/learner_full_name": "عبدالرحمن محمد",
      "https://nelc.gov.sa/extensions/learner_nationality": "Saudi Arabia",
      "https://nelc.gov.sa/extensions/date_of_birth": "18/06/1998",
      "https://nelc.gov.sa/extensions/platform": {
        "name": {
          "ar-SA": "التقنيات المتقدمة للتدريب",
          "en-US": "Leading Tech for Training"
        }
      }
    }
  },
  "timestamp": "2022-01-31T07:18:32.829Z"
}
```
### Statement #2 — **initialized**
```json
{
  "actor": {
    "mbox": "mailto:1234567890@gmail.com",
    "name": "1234567890",
    "objectType": "Agent"
  },
  "verb": {
    "id": "http://adlnet.gov/expapi/verbs/initialized",
    "display": { "en-US": "initialized" }
  },
  "object": {
    "id": "http://www.lmsname.com/course/CR001",
    "definition": {
      "name": { "en-US": "Java for Beginners" },
      "description": { "en-US": "" },
      "type": "https://w3id.org/xapi/cmi5/activitytype/course"
    },
    "objectType": "Activity"
  },
  "context": {
    "instructor": {
      "name": "Ibrahim Khalid",
      "mbox": "mailto:ik@example.com"
    },
    "platform": "MGMT-003",
    "language": "ar-SA",
    "extensions": {
      "https://nelc.gov.sa/extensions/platform": {
        "name": {
          "ar-SA": "التقنيات المتقدمة للتدريب",
          "en-US": "Leading Tech for Training"
        }
      }
    }
  },
  "timestamp": "2022-01-31T07:18:32.829Z"
}
```

### Statement #3 — **watched**
```json
{
  "actor": {
    "mbox": "mailto:1234567890@gmail.com",
    "name": "1234567890",
    "objectType": "Agent"
  },
  "verb": {
    "id": "https://w3id.org/xapi/acrossx/verbs/watched",
    "display": { "en-US": "watched" }
  },
  "object": {
    "id": "http://www.lmsname.com/course/CR001/module/MDL002/video/VD003",
    "definition": {
      "name": { "en-US": "Java for Beginners - Unit 2 Video3 Intro to Java" },
      "description": { "en-US": "concepts and skills" },
      "type": "https://w3id.org/xapi/video/activity-type/video"
    },
    "objectType": "Activity"
  },
  "context": {
    "instructor": {
      "name": "Ibrahim Khalid",
      "mbox": "mailto:ik@example.com"
    },
    "platform": "MGMT-003",
    "language": "ar-SA",
    "extensions": {
      "http://id.tincanapi.com/extension/browser-info": {
        "code_name": "Mozilla",
        "name": "Firefox",
        "version": "5.0"
      },
      "https://nelc.gov.sa/extensions/platform": {
        "name": {
          "ar-SA": "التقنيات المتقدمة للتدريب",
          "en-US": "Leading Tech for Training"
        }
      }
    },
    "contextActivities": {
      "parent": [
        {
          "id": "http://www.lmsname.com/course/CR001",
          "definition": {
            "name": { "en-US": "Java for Beginners" },
            "description": { "en-US": "" },
            "type": "https://w3id.org/xapi/cmi5/activitytype/course"
          },
          "objectType": "Activity"
        }
      ]
    }
  },
  "result": {
    "completion": true,
    "duration": "PT00H10M22S"
  },
  "timestamp": "2022-01-31T07:18:32.829Z"
}
```

### Statement #4 — **completed lesson**
```json
{
  "actor": {
    "mbox": "mailto:1234567890@gmail.com",
    "name": "1234567890",
    "objectType": "Agent"
  },
  "verb": {
    "id": "http://adlnet.gov/expapi/verbs/completed",
    "display": { "en-US": "completed" }
  },
  "object": {
    "id": "http://www.lmsname.com/course/CR001/module/MDL002/lesson/LSN001",
    "definition": {
      "name": { "en-US": "Java for Beginners - Unit 1 Video1 Introduction to Java" },
      "description": { "en-US": "lesson objectives and concepts" },
      "type": "http://adlnet.gov/expapi/activities/lesson"
    },
    "objectType": "Activity"
  },
  "context": {
    "instructor": {
      "name": " Ibrahim Khalid ",
      "mbox": "mailto:ik@example.com"
    },
    "platform": "MGMT-003",
    "language": "ar-SA",
    "extensions": {
      "http://id.tincanapi.com/extension/browser-info": {
        "code_name": "Mozilla",
        "name": "Firefox",
        "version": "5.0"
      },
      "https://nelc.gov.sa/extensions/platform": {
        "name": {
          "ar-SA": "التقنيات المتقدمة للتدريب",
          "en-US": "Leading Tech for Training"
        }
      }
    },
    "contextActivities": {
      "parent": [
        {
          "id": "http://www.lmsname.com/course/CR001",
          "definition": {
            "name": { "en-US": "Java for Beginners" },
            "description": { "en-US": "" },
            "type": "https://w3id.org/xapi/cmi5/activitytype/course"
          },
          "objectType": "Activity"
        }
      ]
    }
  },
  "result": {
    "duration": "PT00H05M00S"
  },
  "timestamp": "2022-01-31T07:18:32.829Z"
}
```
### Statement #5 — **attended**
```json
{
  "actor": {
    "mbox": "mailto:1234567890@gmail.com",
    "name": "1234567890",
    "objectType": "Agent"
  },
  "verb": {
    "id": "http://adlnet.gov/expapi/verbs/attended",
    "display": { "en-US": "attended" }
  },
  "object": {
    "id": "http://www.lmsname.com/course/CR001/module/MDL002/virtual-classroom/VD003",
    "definition": {
      "name": { "en-US": "Java for Beginners - Unit 2 - Practical lesson" },
      "description": { "en-US": "concepts and skills" },
      "type": "https://w3id.org/xapi/virtual-classroom/activity-types/virtual-classroom"
    },
    "objectType": "Activity"
  },
  "context": {
    "instructor": {
      "name": " Ibrahim Khalid ",
      "mbox": "mailto:ik@example.com"
    },
    "platform": "MGMT-003",
    "language": "ar-SA",
    "extensions": {
      "http://id.tincanapi.com/extension/browser-info": {
        "code_name": "Mozilla",
        "name": "Firefox",
        "version": "5.0"
      },
      "https://nelc.gov.sa/extensions/platform": {
        "name": {
          "ar-SA": "التقنيات المتقدمة للتدريب",
          "en-US": "Leading Tech for Training"
        }
      }
    },
    "contextActivities": {
      "parent": [
        {
          "id": "http://www.lmsname.com/course/CR001",
          "definition": {
            "name": { "en-US": "Java for Beginners" },
            "description": { "en-US": "" },
            "type": "https://w3id.org/xapi/cmi5/activitytype/course"
          },
          "objectType": "Activity"
        }
      ]
    }
  },
  "result": {
    "completion": true,
    "duration": "PT01H30M00S"
  },
  "timestamp": "2022-01-31T07:18:32.829Z"
}
```

### Statement #6 — **attempted**
```json
{
  "actor": {
    "mbox": "mailto:1234567890@gmail.com",
    "name": "1234567890",
    "objectType": "Agent"
  },
  "verb": {
    "id": "http://adlnet.gov/expapi/verbs/attempted",
    "display": { "en-US": "attempted" }
  },
  "object": {
    "id": "http://www.lmsname.com/course/CR001/module/MDL001/quiz/QZ001",
    "definition": {
      "name": { "en-US": "Java Crash Course unit - 1 Quiz -1" },
      "description": { "en-US": "Java Crash Course unit - 1 Quiz -1" },
      "type": "http://id.tincanapi.com/activitytype/unit-test"
    },
    "objectType": "Activity"
  },
  "context": {
    "instructor": {
      "name": " Ibrahim Khalid",
      "mbox": "mailto:ik@example.com"
    },
    "platform": "MGMT-003",
    "language": "ar-SA",
    "extensions": {
      "http://id.tincanapi.com/extension/attempt-id": 1,
      "http://id.tincanapi.com/extension/browser-info": {
        "code_name": "Mozilla",
        "name": "Firefox",
        "version": "5.0"
      },
      "https://nelc.gov.sa/extensions/platform": {
        "name": {
          "ar-SA": "التقنيات المتقدمة للتدريب",
          "en-US": "Leading Tech for Training"
        }
      }
    },
    "contextActivities": {
      "parent": [
        {
          "id": "http://www.lmsname.com/course/CR001",
          "definition": {
            "name": { "en-US": "Java for Beginners" },
            "description": { "en-US": "" },
            "type": "https://w3id.org/xapi/cmi5/activitytype/course"
          },
          "objectType": "Activity"
        }
      ]
    }
  },
  "result": {
    "score": {
      "scaled": 0.60,
      "raw": 30,
      "min": 20,
      "max": 50
    },
    "success": true,
    "completion": false
  },
  "timestamp": "2022-01-31T07:18:32.829Z"
}
```

### Statement #7 — **completed module**
```json
{
  "actor": {
    "mbox": "mailto:1234567890@gmail.com",
    "name": "1234567890",
    "objectType": "Agent"
  },
  "verb": {
    "id": "http://adlnet.gov/expapi/verbs/completed",
    "display": { "en-US": "completed" }
  },
  "object": {
    "id": "http://www.lmsname.com/course/CR001/module/MDL002",
    "definition": {
      "name": { "en-US": "Java for Beginners - Unit 1 Basics of Java" },
      "description": { "en-US": "Unit Objectives" },
      "type": "http://adlnet.gov/expapi/activities/module"
    },
    "objectType": "Activity"
  },
  "context": {
    "instructor": {
      "name": " Ibrahim Khalid ",
      "mbox": "mailto:ik@example.com"
    },
    "platform": "MGMT-003",
    "language": "ar-SA",
    "extensions": {
      "http://id.tincanapi.com/extension/browser-info": {
        "code_name": "Mozilla",
        "name": "Firefox",
        "version": "5.0"
      },
      "https://nelc.gov.sa/extensions/platform": {
        "name": {
          "ar-SA": "التقنيات المتقدمة للتدريب",
          "en-US": "Leading Tech for Training"
        }
      }
    },
    "contextActivities": {
      "parent": [
        {
          "id": "http://www.lmsname.com/course/CR001",
          "definition": {
            "name": { "en-US": "Java for Beginners" },
            "description": { "en-US": "" },
            "type": "https://w3id.org/xapi/cmi5/activitytype/course"
          },
          "objectType": "Activity"
        }
      ]
    }
  },
  "timestamp": "2022-01-31T07:18:32.829Z"
}
```

### Statement #8 — **progressed**
```json
{
  "actor": {
    "mbox": "mailto:1234567890@gmail.com",
    "name": "1234567890",
    "objectType": "Agent"
  },
  "verb": {
    "id": "http://adlnet.gov/expapi/verbs/progressed",
    "display": { "en-US": "progressed" }
  },
  "object": {
    "id": "http://www.lmsname.com/course/CR001",
    "definition": {
      "name": { "en-US": "Java for Beginners" },
      "description": { "en-US": "" },
      "type": "https://w3id.org/xapi/cmi5/activitytype/course"
    },
    "objectType": "Activity"
  },
  "context": {
    "instructor": {
      "name": "Ibrahim Khalid",
      "mbox": "mailto:ik@example.com"
    },
    "platform": "KSUX",
    "language": "ar-SA",
    "extensions": {
      "https://nelc.gov.sa/extensions/platform": {
        "name": {
          "ar-SA": "التقنيات المتقدمة للتدريب",
          "en-US": "Leading Tech for Training"
        }
      }
    }
  },
  "result": {
    "score": {
      "scaled": 0.15
    },
    "completion": true
  },
  "timestamp": "2022-01-31T07:18:32.829Z"
}
```

### Statement #9 — **completed course**
```json
{
  "actor": {
    "mbox": "mailto:1234567890@gmail.com",
    "name": "1234567890",
    "objectType": "Agent"
  },
  "verb": {
    "id": "http://adlnet.gov/expapi/verbs/completed",
    "display": { "en-US": "completed" }
  },
  "object": {
    "id": "http://www.lmsname.com/course/CR001",
    "definition": {
      "name": { "en-US": "Java for Beginners" },
      "description": { "en-US": "" },
      "type": "https://w3id.org/xapi/cmi5/activitytype/course"
    },
    "objectType": "Activity"
  },
  "context": {
    "instructor": {
      "name": "Ibrahim Khalid",
      "mbox": "mailto:ik@example.com"
    },
    "platform": "KSUX",
    "language": "ar-SA",
    "extensions": {
      "https://nelc.gov.sa/extensions/platform": {
        "name": {
          "ar-SA": "التقنيات المتقدمة للتدريب",
          "en-US": "Leading Tech for Training"
        }
      }
    }
  },
  "timestamp": "2022-01-31T07:18:32.829Z"
}
```

### Statement #10 — **rated**
```json
{
  "actor": {
    "mbox": "mailto:1234567890@gmail.com",
    "name": "1234567890",
    "objectType": "Agent"
  },
  "verb": {
    "id": "http://id.tincanapi.com/verb/rated",
    "display": { "en-US": "rated" }
  },
  "object": {
    "id": "http://www.lmsname.com/course/CR001",
    "definition": {
      "name": { "en-US": "Java for Beginners" },
      "description": { "en-US": "" },
      "type": "https://w3id.org/xapi/cmi5/activitytype/course"
    },
    "objectType": "Activity"
  },
  "context": {
    "instructor": {
      "name": "Ibrahim Khalid",
      "mbox": "mailto:ik@example.com"
    },
    "platform": "KSUX",
    "language": "ar-SA",
    "extensions": {
      "https://nelc.gov.sa/extensions/platform": {
        "name": {
          "ar-SA": "التقنيات المتقدمة للتدريب",
          "en-US": "Leading Tech for Training"
        }
      }
    }
  },
  "result": {
    "score": {
      "scaled": 0.8,
      "raw": 4,
      "min": 0,
      "max": 5
    },
    "response": "Text review written by the evaluator"
  },
  "timestamp": "2022-01-31T07:18:32.829Z"
}
```

### Statement #11 — **earned**
```json
{
  "actor": {
    "mbox": "mailto:1234567890@gmail.com",
    "name": "1234567890",
    "objectType": "Agent"
  },
  "verb": {
    "id": "http://id.tincanapi.com/verb/earned",
    "display": { "en-US": "earned" }
  },
  "object": {
    "id": "http://www.lmsname.com/course/CR001/certificate/CFT001",
    "definition": {
      "name": { "en-US": "Java level-1 Certificate" },
      "type": "https://www.opigno.org/en/tincan_registry/activity_type/certificate"
    },
    "objectType": "Activity"
  },
  "context": {
    "extensions": {
      "http://id.tincanapi.com/extension/jws-certificate-location": "http://www.abc.com/cert/12341231",
      "https://nelc.gov.sa/extensions/platform": {
        "name": {
          "ar-SA": "التقنيات المتقدمة للتدريب",
          "en-US": "Leading Tech for Training"
        }
      }
    },
    "platform": "KSUX",
    "language": "ar-SA",
    "contextActivities": {
      "parent": [
        {
          "id": "http://www.lmsname.com/course/CR001",
          "definition": {
            "name": { "en-US": "Java for Beginners" },
            "description": { "en-US": "" },
            "type": "https://w3id.org/xapi/cmi5/activitytype/course"
          },
          "objectType": "Activity"
        }
      ]
    }
  },
  "timestamp": "2022-01-31T07:18:32.829Z"
}
```

## Notes to Consider and Common Mistakes

- The first event to be sent is the **"registered"** statement.
- The **"initialized"** action is done when the learner performs the first action in the LMS after the "registered" event.
- The **"watched"** video statement must be automatically sent **when the video ends**, without requiring any learner interaction such as clicking “Mark as complete.”
- A learner activity must be **sent only once**.  
  Duplicate statements are **not allowed**.
- Course descriptions must **not contain HTML tags**.  
  The same rule applies to all object descriptions.
- Object descriptions must clearly describe the object.  
  The **course description must be the full official description** from the LMS course card.
- Instructor name must be accurate.  
  Placeholder text, dummy data, or junk values are not accepted.
- Video duration must be **exact**.
- Lesson duration is the **estimated time** required to complete the lesson.
- Lessons that are not videos or virtual classrooms—such as HTML pages or PDF reading materials—**must still be treated as lessons**.
- The **"progress"** event should be sent:
  - after lesson completion  
  - after unit completion  
  - after quiz attempts  
  - and at any reasonable progress interval
- The platform AR/EN names must be the official licensed names and must match exactly the values sent under:  
  `https://nelc.gov.sa/extensions/platform`
- There must be **minimal validation** for national ID input:  
  it must be **10 digits** and begin with **1, 2, or 4** to comply with Saudi identification rules.

---

## Resources

**Integration Service Providers:**  
https://futurex.nelc.gov.sa/ar/partnerships/platform

### Plugins (designed by NELC)

- **WordPress / Laravel**
- **PHP Laravel**  
  https://github.com/nelc/laravel-lrs-package
- **Laravel Helper**  
  https://github.com/nelc/laravel-lrs-helpers
- **LearnDash LMS**  
  https://github.com/nelc/learndash-lrs-plugin/
- **TutorLMS**  
  https://github.com/nelc/tutor-lms-lrs-plugin


**Postman collection download:**  
[MyFutureX.postman_collection](https://raw.githubusercontent.com/kalraymi/NELC-Integration/main/MyFutureX.postman_collection.json)


---

## Contact Us

If you have any questions, contact the Integration Team:

📧 integration@elc.edu.sa  
📧 k.alraymi@elc.edu.sa
