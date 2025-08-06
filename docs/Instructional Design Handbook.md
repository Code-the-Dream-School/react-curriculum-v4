# **Instructional Design Handbook**

## **Code the Dream | 2025**

*Extracted from [Instructional Design Handbook - Google Docs](https://docs.google.com/document/d/1u38QtHKfGeXwJf1yTYAyx0YK3yvit5dK6rhM0PZjx8c/edit?tab=t.0#heading=h.cxdyr2h5nma6)*
*Current as of 2025-08-06*

**Table of Contents:**

[Adapted ADDIE Process 2](#adapted-addie-process)

[Course Design Guidelines 3](#course-design-guidelines)

[Start with the End in Mind 3](#start-with-the-end-in-mind)

[Backwards Design 3](#backwards-design)

[Learning Objectives 3](#learning-objectives)

[Measure Progress with Assessments 4](#measure-progress-with-assessments)

[Formative and Summative Assessments 5](#formative-and-summative-assessments)

[Reflection Questions to Evaluate an Assessment Draft 6](#reflection-questions-to-evaluate-an-assessment-draft)

[Create Rich, Compelling, Challenging Content 7](#create-rich,-compelling,-challenging-content)

[Universal Design for Learning 7](#universal-design-for-learning)

[UDL in Brief 7](#udl-in-brief)

[Additional Tips to Support UDL 8](#additional-tips-to-support-udl)

[Best Practices for Supporting English-Language Learners 8](#best-practices-for-supporting-english-language-learners)

[Best Practices for Supporting Learners without a Formal Educational Background 9](#best-practices-for-supporting-learners-without-a-formal-educational-background)

[Reflection Questions to Evaluate a Lesson Draft 9](#reflection-questions-to-evaluate-a-lesson-draft)

[Support Students & Mentors with Effective Teaching Resources 9](#support-students-&-mentors-with-effective-teaching-resources)

[Explore and Apply Lessons 9](#explore-and-apply-lessons)

[Gradual Release of Responsibility 10](#gradual-release-of-responsibility)

[Assignment Rubrics and Sample Code 11](#assignment-rubrics-and-sample-code)

[Appendix A: Course Planning Template 12](#appendix-a:-course-planning-template)

[Appendix B: Individual Lesson Template 13](#appendix-b:-individual-lesson-template)

[Appendix C: Individual Assignment Template 16](#appendix-c:-individual-assignment-template)

[Appendix D: Group Mentor Session Template 18](#appendix-d:-group-mentor-session-template)

#

# **Adapted ADDIE Process** {#adapted-addie-process}

*Note: This is an adaptation of the classic [ADDIE](https://en.wikipedia.org/wiki/ADDIE_model) process, along with some considerations from [Backwards Design](https://www.cultofpedagogy.com/backward-design-basics/) and [Universal Design for Learning (UDL)](https://udlguidelines.cast.org/).*

| Area | Process | Deliverables |
| ----- | ----- | ----- |
| **Align**  *Career and Learning Goals* | Define target job roles and skills Identify industry standards and must-have competencies Analyze the existing course and determine the scope of needed changes.  Outline the overall learning outcomes and how they will be assessed. | List of key skills needed for the target job roles.  Overall course learning objectives and method of assessment.  |
| **Design** *Course Structure & Content Plan* | Outline learning objectives for individual units and break down topics into weekly learning goals.  Create end-of-course assessment and outline how each lesson builds toward the assessment.  Outline course structure following CTD’s course design guidelines.  | Public GitHub repo with folders for lesson content, assignments, and mentor content.  Draft of the final project and/or assessment. High-level outline of the weekly course structure.  |
| **Develop** *Create & Review Materials* | SMEs and CILs draft lessons, assessments, and mentor materials.  Team ensures that lessons meet CTD standards, especially related to UDL \- accessibility, multiple modalities, and scaffolding. SMEs, volunteers, and CTD staff provide feedback on batches of lessons to ensure accuracy, engagement, and equity. | Draft lesson content, assignments, and mentor materials for each week. Feedback (in the form of PRs or conversations) from stakeholders on accuracy, engagement, and equity.  |
| **Implement** *Pilot & Teach* | Test new curriculum with beta testers or a small pilot class.  Gather feedback from beta/pilot students and mentors and adjust content as needed.  | Feedback from students and mentors; proposed changes.  Adjusted content as needed.  |
| **Refine** *Improve & Maintain* | Teach the new curriculum in a full-size class of students.  Analyze data on student engagement.  Update lessons based on further feedback and industry trends.  | Post-class report from Data Coordinator on the data from the full-class cohort.  Small fixes to the course, or proposal for larger fixes.  |

# **Course Design Guidelines** {#course-design-guidelines}

## **Start with the End in Mind** {#start-with-the-end-in-mind}

### Backwards Design {#backwards-design}

**Backwards design** is an approach to course design that emphasizes *learning outcomes* over *topics to cover.[^1][^2]* It is “backwards” from traditional course design because instead of starting with a list of topics, the tests to be passed, even a textbook, it starts with the course goals.

Backwards design has three steps:

1. Identify what students should be able to **do** after completing the course.
2. Determine how students will be assessed on what they can do.
3. Plan the learning experiences that will guide students to the learning objectives.

These guidelines follow that structure. We’ll start with learning outcomes, discuss assessments, and only then get to content.

### Learning Objectives {#learning-objectives}

Learning objectives are statements that clearly describe what students are expected to achieve as a result of instruction. They are **not** a list of topics covered in a lesson.

Traditional course design often just lists a series of topics to cover. Consider this list for a lesson on SQL:

* Relational databases
* SQL basics  
* CRUD operations  
* SQL query operations: SELECT, constraints, ordering  
* Primary and foreign keys  
* SQL joins

This list does not specifically describe the depth of knowledge or skills students should acquire. It’s also ambiguous in how students will be assessed. How do we actually know if they understand relational databases to the level we want?

Specific, clear learning objectives are better because they help ensure that all of our assessments and content align with what students need to learn. Also, learning objectives put the **emphasis on what the students can do, not what the instructor can cover.** Here’s an example of better learning objectives:

By the end of this module, students will be able to:

1. Explain the fundamentals of relational databases and SQL, including their importance in back-end development.  
2. Implement CRUD operations and write basic SQL queries using SELECT statements with constraints and ordering.  
3. Design database schemas using primary and foreign keys, and construct queries utilizing joins.  
4. (Stretch Goal) Demonstrate proficiency in SQL by solving real-world database problems.

Note that this list also includes an optional stretch goal opportunity for high-performing students. We’ll talk more about that in the next section.

There are three steps to writing a learning objective.[^3]

1. **Define the object.** What skills, knowledge, attitude, or abilities will students gain?  
2. **Determine the mastery level.** How complex and deep will student knowledge be? Use [Bloom’s taxonomy](https://cft.vanderbilt.edu/guides-sub-pages/blooms-taxonomy/) to help think through mastery levels.[^4]  
3. **Outline the assessment.** How will student learning be measured?

#### Extension

Adding additional, more challenging opportunities for students who have mastered the core content “extends” their learning beyond the standard curriculum. In our content, optional “stretch goals” give students the opportunity to push their knowledge. This also prevents students from working ahead too quickly.

Extension can be accomplished through:

* Offering more complex or in-depth content.  
* Encouraging higher-order thinking skills (think back to [Bloom’s taxonomy](https://cft.vanderbilt.edu/guides-sub-pages/blooms-taxonomy/)).
* Provide opportunities for independent study or research.
* Introduce advanced topics related to the core lesson topic.

## **Measure Progress with Assessments** {#measure-progress-with-assessments}

Assessments gather data about student progress towards learning outcomes. They **are not** just a checkbox to indicate that a student completed a week’s content.

Potential types of assessments include code reviews, bug-fixing assignments, algorithm design challenges, project-based learning, or coding quizzes with multiple choice and short answer questions.

### Formative and Summative Assessments {#formative-and-summative-assessments}

#### Formative Assessments

The goal of **formative assessments** is to *monitor student learning* and provide feedback to students. In traditional education settings, these are not graded, are intentionally low-stakes, and allow students to fail. At CTD, most weekly assignments are similar to formative assessments.

Our goal is to include formative assessments each week (assuming there is not a summative assessment that week).

#### Summative Assessments

On the other hand, **summative assessments** *evaluate student learning* at the end of an instructional unit by comparing the assessment against expected benchmarks. Final exams are a classic example of summative assessments.

Our goal is to include summative assessments several times in a course, likely at the end of each unit, as long as that unit is not very short. For example, an Intro course with 4 weeks of JavaScript, 2 weeks of Git/GitHub, 3 weeks of HTML & CSS, and 2 weeks of final project work could have three summative assessments: one for JavaScript, one for Git/GitHub and HTML & CSS, and a final project.

#### CTD Assessment Practices

In 2026, CTD classes will follow three factors for determining whether students pass a class: assignment score, quiz score, and project score. See the table below for more details. (Note: this is an active discussion, final % of grade and other details TBD).

| Factor | % of Grade | Description |
| ----- | ----- | ----- |
| **Assignment Score** | TBD% | **Description:** Coding assignments where students apply the new material they have learned. **Frequency:** Each week **Measurement:** Reflects the number of “approved” assignments each week (8/11 \= 72%). ***Anything less than 7 is an automatic 0% for this category.*** |
| **Quiz Score** | TBD% | **Description**: Small Coderbyte (or similar tool) quizzes that assess students on chunks of knowledge. **Frequency**: \~3 times per class (at the end of a module). **Measurement**: Students need to have an average score of **TBD%** across the assessments for this factor to be successful.  |
| **Project Score** | TBD% | **Description**: ***Either*** a final project OR \~2-3 smaller projects within a course. **Frequency**: Either once at the end of the course or several times throughout the course. **Measurement**: ***All projects must be approved*** for this factor to resolve. |

### Reflection Questions to Evaluate an Assessment Draft {#reflection-questions-to-evaluate-an-assessment-draft}

Once you complete an assessment draft — whether it is a weekly coding assignment, a Coderbyte quiz, or a project score — use the following reflection questions to determine whether it is ready to be published.

1. **Alignment with Learning Objectives**  
   1. Is the assessment directly tied to this week’s or module’s learning objectives, both in terms of content and level of mastery?
   2. Can the assessment be used to measure student mastery of the learning objectives?  
   3. Does the assessment provide opportunities for extension beyond the learning objectives?  
2. **High-quality, engaging content**  
   1. Is the assessment split into clear, discrete tasks? Does it directly state what is expected of students in each task?  
   2. Is the assessment tied to real-world use cases? Does it require creative thinking from students?  
   3. Is the assessment just “repeat after me,” or does it push students to *apply* what they have learned?  
3. **Clear rubric or method of evaluation**  
   1. Could a student repeat back to you *how* they will be measured?  
   2. Could a mentor identify a few clear metrics on which students will be graded?  
4. **Example answers for assignment reviewers**  
   1. Is there a set of sample correct answers that we can provide to assignment reviewers?
   2. If multiple correct answers are possible, is there a rubric that helps mentors determine whether students are successful?

One way to be clear with students about which skills they are being evaluated on is to list the requirements for the final project *at the start or early on* in the course. Consider providing both the rubric and an example final project to students as early as possible.

## Create Rich, Compelling, Challenging Content {#create-rich,-compelling,-challenging-content}

## Universal Design for Learning {#universal-design-for-learning}

Programmers have probably heard of [Universal Design](https://www.washington.edu/doit/what-universal-design-0). Universal Design for *Learning* is a framework for creating courses that are accessible and challenging to the largest number of students from the outset.[^5]

* It’s about building the curriculum with diversity in mind, not retrofitting it to accommodate needs.[^6]  
* It’s also based in neuroscience, which indicates the brain is divided into three broad learning networks (essentially *how*, *why*, and *what*).[^7]

UDL is not about making content “easier,” it’s about making it **accessible, meaningful, and appropriately challenging** for as many learners as possible from the start. This is especially important at Code the Dream, where our learners bring diverse language backgrounds, education levels, and tech exposure.

### UDL in Brief {#udl-in-brief}

**1\. Engage the Learner (Why)**

* Start each lesson with a real-world hook or problem that the concept solves.  
* Use learner stories, career applications, or short anecdotes to help students connect.  
* Offer stretch goals and optional deeper dives to challenge advanced students.

**2\. Provide Multiple Means of Representation (What)**

* Every concept should include visual, text-based, and code-based explanations.  
* Use diagrams, pseudocode, and real code examples side by side.  
* **Link to outside resources**: MDN, W3Schools, Python docs, etc.

**3\. Provide Multiple Means of Action & Expression (How)**

* Include “Check for Understanding” questions in multiple formats: multiple choice, fill-in-the-blank, and short reflection questions.  
* Encourage optional mini-projects or experiments based on the concept.

### Additional Tips to Support UDL {#additional-tips-to-support-udl}

#### Avoid Cognitive Overload by Chunking Material

**Cognitive overload** occurs when our working memory receives too much new information to convert into long-term memory.

* [Our working memories can handle 5-9 pieces, or chunks, or information at a time](https://www.mcw.edu/-/media/MCW/Education/Academic-Affairs/OEI/Faculty-Quick-Guides/Cognitive-Load-Theory.pdf); keep lessons from spiraling beyond that scope.
* Break lessons into clear subsections (for example: “1.1: Variables”), which help students know when to pause for breaks and when to prepare for new information.

#### Point to Documentation and Supplemental Resources

Good documentation teaches students to write consistent, quality code. It helps clearly define key concepts and connects to deeper learning. Whenever possible, add links to official documentation.

Additionally, adding supplemental resources — including videos, podcasts, articles, or interactive assignments — helps students explore concepts in depth without adding too much bloat to the course.

### Best Practices for Supporting English-Language Learners {#best-practices-for-supporting-english-language-learners}

Our learners may be English-language learners (ELLs) or may have had gaps in formal education. Here’s how to write content that supports everyone:

Do:

* Use clear, plain English without dumbing things down. Avoid language that is:
  * Overly complex or difficult to read  
  * Vague
  * Weak or in the passive voice  
  * Casual or full of idioms and colloquialisms  
  * Demotivating  
* Define technical terms the first time you use them, and recap them in a glossary or "Key Terms" section  
* Break up long paragraphs and use headings, lists, and code blocks for readability.  
* Use examples with diverse names and culturally neutral contexts.  
* Pair instructions with screenshots and examples, especially for tools like VS Code, GitHub, or terminal commands.

### Best Practices for Supporting Learners without a Formal Educational Background {#best-practices-for-supporting-learners-without-a-formal-educational-background}

Many CTD students may be returning to education after years away, or may not have completed formal schooling. We aim to create a learning environment that builds confidence and emphasizes progress over perfection.

Do:

* Scaffold tasks clearly: break complex activities into steps and explain the “why.”  
* Avoid assumptions: don’t assume familiarity with tech jargon, academic expectations, or implicit cultural references.  
* Use concrete examples and plain language.  
* Offer multiple chances to revisit or revise a concept (this reduces the “fear of getting it wrong”).  
* Celebrate persistence and effort, not just accuracy.

### Reflection Questions to Evaluate a Lesson Draft {#reflection-questions-to-evaluate-a-lesson-draft}

1. **Clarity of Objectives**  

* Are this lesson’s learning objectives clearly stated and measurable?  
* Does the content align with these objectives?  

2. **Accessibility and Engagement**  

* Does the lesson use multiple modes of instruction (visual, text, code, etc.)?  
* Are key terms defined and reinforced with examples?  
* Would this be engaging and understandable to someone with limited prior academic experience?  

3. **Structure & Flow**  

* Is the lesson broken into manageable chunks with clear transitions?  
* Are there natural stopping points or check-for-understanding moments?  

4. **Mentor Support**  

* Can you anticipate where mentors may need to jump in and explain more deeply?  
* Are there notes or guidance for mentors embedded in the content or GitHub repo?

## **Support Students & Mentors with Effective Teaching Resources** {#support-students-&-mentors-with-effective-teaching-resources}

Mentor sessions are **essential** to our course content; one data analysis found that mentor session attendance is the most important variable in determining student success. A strong curriculum comes with clear, useful mentor materials to help our volunteer mentors take full advantage of time with students.

### Explore and Apply Lessons {#explore-and-apply-lessons}

We’re slowly expanding **more frequent, more structured** group mentor sessions.

* **Explore** sessions focus on the new concepts taught each week, placing special emphasis on the “why” and “how” of each new concept.

  * These sessions feature a mixture of direct instruction and group discussion.
  * Pitch to students: “Uncover the ‘why’ and ‘how’ of this week’s topic. Explore concepts through discussion and collaborative learning to gain a deep understanding.”

* **Apply** sessions let students engage in hands-on practice and assignment support.

  * Activities include guided coding, peer review, and troubleshooting.
  * Pitch to students: “Put your knowledge to work in a hands-on environment. Tackle real-world problems, work on this week’s assignment, and refine your skills with mentor guidance.”

### Gradual Release of Responsibility {#gradual-release-of-responsibility}

Both Explore and Apply sessions will follow the **Gradual Release of Responsibility model**, a framework for transferring responsibility in the learning process from the teacher to the student. In the classroom, there are three phases to this model:

1. **“I Do” (Direct Instruction)**: The teacher or instructor presents a new skill and/or models how to complete a task or solve a problem.
2. **“We Do” (Group Practice)**: The teacher and students work together to complete the same task or solve the same problem. The teacher provides support and guidance as needed, but students actively participate in the task.
3. **“You Do” (Solo Student Work)**: Students work independently to complete tasks and solve problems. The teacher offers feedback on student work.

At CTD, the “You Do” phase will be outside of mentor sessions and incorporate feedback from assignment reviewers. We’ll structure our lesson plans around this model, along with a brief “Entrance Ticket” task at the beginning of each module and a quick “Exit Ticket” at the end.

* “Entrance Tickets” are designed to introduce students to this week’s topics or promote social engagement among students.  
* “Exit Tickets” are designed to briefly gather data on where students need additional support after the session.

To keep things simple, almost all of our Entrance and Exit tickets will be multiple choice questions that mentors present on a shared screen and students can respond to via a Google poll or chat.

#### Group Session Slide Decks

CTD will provide slide deck templates for each Explore session with:

* An overview of the topic

* A few worked examples

* CFU questions and discussion prompts

Our goal is to have these ready by the start of each class. [Here is a simple template](https://docs.google.com/presentation/d/1wgSVMbzxtUmeLbnd4PXNh9SG8l79hmYnKWlJZfHB6qw/edit?usp=sharing) for slide decks – feel free to build off this\!

### Assignment Rubrics and Sample Code {#assignment-rubrics-and-sample-code}

Each assignment should come with a **rubric** that mentors can use to review content and **sample code** against which they can judge student code.

Here’s a sample rubric:

| Performance Level | Description |
| :---- | :---- |
| Does not meet expectations  | Student is unable to correctly write or explain basic SQL queries. Queries contain major syntax errors or do not follow correct logic (e.g., incorrect use of WHERE, missing semicolons, or failure to select specific columns). |
| Meets expectations | Student can successfully write basic SQL queries to perform SELECT, INSERT, UPDATE, and DELETE operations on a relational database. Queries are syntactically correct and return expected results for typical use cases.  |
| Exceeds expectations | Student demonstrates fluency with CRUD operations by incorporating more advanced SQL features such as filtering with LIKE, ordering results, using LIMIT, or performing conditional updates. Code includes thoughtful use of column selection, comments, and formatting for readability.  |

Sample code should be clear and well-commented.

#

# **Appendix A: Course Planning Template** {#appendix-a:-course-planning-template}

| Course Name |  |
| :---- | :---- |
| Prerequisites | *What classes did students take at CTD before this? What skills do they know (or need to know) to be successful?* |
| Course Objective | *What is the final product of this class? What should students be able to do?*  |
| Overall Learning Objectives | *Learning objectives Learning objectives*  |
| Modules | Module 1: TBD Lesson 1: TBD Lesson 2: TBD Lesson 3: TBD |

#

# **Appendix B: Individual Lesson Template** {#appendix-b:-individual-lesson-template}

## **\[Lesson X.X: Lesson Name\]**

## Overview

### Learning objectives

By the end of this lesson, students will be able to:

* TBD

### Topics covered

* TBD
* TBD
* TBD

### Prerequisites

* Required knowledge from previous lessons
* Required setup/installations

## Quick Review

* Brief exercise or multiple-choice check-for-understanding question covering concepts from previous lessons  
* Focus on concepts that connect to this week’s material
* Include worked examples of previous concepts

## Introduction

### Welcome Video/Podcast (5-7 minutes)

* Connect to previous learning  
* Preview this week’s content
* Explain real-world applications  
* Overview the week’s coding assignment

### Why This Matters

* Industry relevance
* Common use applications
* Career applications

## Core Content

### \[Subtopic formatted like 1.1, 1.2\]

* Concept explanation
* Code examples
* Common pitfalls

#### Subtopic check-for-understanding questions

* 2-3 quick questions
* Small code challenge

#### Subtopic Key Terms

* Term: Definition  
* Term: Definition

#### Subtopic Additional Resources

* Documentation links  
* Supplemental readings  
* Video tutorials

### \[Additional subtopics formatted like previous\]

## Guided Practice

### Mini-Project Overview

* Project overview  
* Learning goals  
* Starting code

### Step-by-Step Guide

1. Initial setup  
2. First implementation  
3. Testing and debugging  
4. Extension opportunities

### Common issues and solutions

## Review and Summary

* Key takeaways from this lesson  
* Connection to next lesson
* Quick preview of this week’s coding assignment

#

# **Appendix C: Individual Assignment Template** {#appendix-c:-individual-assignment-template}

# \[Assignment X.X: Assignment Name\]

## Overview

### Learning objectives

By the end of this lesson, students will be able to:

* TBD

### Prerequisites

* Required knowledge from previous lessons
* Required setup/installations

### Assignment rubric

Detailed list of:

* Features/Deliverables students need to pass the weekly assignment.
* The *level of mastery* students need to be successful.
* Note any extension tasks or remediation for students who are struggling.

## Setup

* List any steps students need to set up the weekly assignment, including files to download or tools to access.

## Task List

### Task 1

* TBD  
* TBD  
* TBD

### Task 2

* Etc.

## Submitting the Assignment

* Instructions on submission.

#

# **Appendix D: Group Mentor Session Template** {#appendix-d:-group-mentor-session-template}

| Phase | Time | Purpose | Mentor Tasks | Student Tasks |
| ----- | ----- | ----- | ----- | ----- |
| Entrance Ticket | 5 min | Activate prior knowledge and set the stage for the session | Mentor shares the entrance ticket with students via Zoom/Gmeet chat.  Mentor takes attendance.  | Students reflect on prior knowledge and respond to the entrance ticket.   |
| Direct Instruction | 20 min | Provide clear, foundational teaching on the topic.  | Mentor presents a slide deck with information about this week’s topic. Even though the mentor is doing most of the talking here, they will ask “check for understanding” questions to students. | Students take notes, respond to CFU questions, or ask clarifying questions.  |
| Breakout Room Discussion | 15 min | Facilitate peer-to-peer learning and collaborative exploration.  | Mentor leads a brief discussion on the topic or asks students to brainstorm a response to a problem. The mentor’s role is to ask questions and give feedback. They only answer their questions if students get stuck. (Note: If attendance is less than 5 people, no breakout room is necessary).  | Students respond to the group discussion questions. Students should do a majority of the thinking here, only relying on the mentor if they get stuck.  |
| Reflect on Breakout and Q\&A | 15 min | Synthesize insights from discussions and address any remaining questions.  | Mentor facilitates reflection & answers questions from students.  | Each discussion group reflects on a takeaway or challenge from their discussion.  |
| Exit Ticket | 5 min | Wrap up the session with a check for understanding. | Mentor shares a brief exit ticket and calls out excellent student responses.  | Students respond to the exit ticket (example, write down a new concept learned or answer a multiple-choice question).  |

[^1]:  “[Where to Start: Backward Design](https://tll.mit.edu/teaching-resources/course-design/backward-design/),” MIT Teaching and Learning Lab.

[^2]:  “[Backwards Design Basics](https://cteresources.bc.edu/documentation/backwards-design/),” Boston College.

[^3]:  “[Learning Objectives](https://cteresources.bc.edu/documentation/learning-objectives/),” Boston College.

[^4]:  “[Bloom’s Taxonomy](https://cft.vanderbilt.edu/guides-sub-pages/blooms-taxonomy/),” Vanderbilt Center for Teaching and Learning.

[^5]:  “[Universal Design for Learning,](https://cteresources.bc.edu/documentation/sample-syllabus-statements/genai/)” Boston College.

[^6]:  “[Universal Design for Learning: An Introduction](https://www.nea.org/professional-excellence/student-engagement/tools-tips/universal-design-learning-introduction),” National Education Association.

[^7]:  “[UDL and the Learning Brain](https://www.cast.org/binaries/content/assets/common/publications/articles/cast-udlandthebrain-20220228-a11y.pdf),” CAST.
