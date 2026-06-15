### Code Review Best Practices

Code review is the systematic examination of software source code by developers other than the original author prior to merging the code into a shared repository. Its primary functions are to identify defects, ensure adherence to established coding standards, and distribute technical knowledge across an engineering team.

Below is a breakdown of the standard practices required to execute effective and efficient code reviews.

---

### Author Responsibilities

The quality of a code review is heavily dependent on how the original author prepares and submits the code.

* **Scope Limitation:** Submissions must be small and address a single, specific issue. A review containing hundreds of changed files reduces the reviewer's ability to identify logical errors and increases the probability of introducing defects.
* **Contextual Documentation:** The author must provide a written summary attached to the submission. This summary must explain the exact problem being solved, the reasoning behind the chosen technical approach, and specific instructions for testing the changes.
* **Pre-submission Verification:** Before requesting a peer review, the author must perform a preliminary self-review. The code must compile without errors, pass all automated test suites, and clear any configured static analysis tools.

### Reviewer Focus Areas

Manual code review time is an expensive engineering resource and must be directed toward issues that automated systems cannot detect.

* **Logic and Architecture:** The reviewer must evaluate whether the code solves the intended problem correctly, operates efficiently, and integrates cleanly with the existing system architecture.
* **Automated Testing Validation:** The reviewer must verify that the author included sufficient unit and integration tests for the new code, and that existing tests were accurately updated to reflect modified logic.
* **Security and Performance:** The code must be inspected for potential security vulnerabilities, such as improper data sanitization, and performance degradations, such as unoptimized database queries or blocking operations on a main execution thread.
* **Avoid Style Debates:** Reviewers must not use manual review time to enforce formatting rules. Syntax spacing, indentation, and strict variable naming conventions must be enforced automatically by formatting tools and continuous integration pipelines prior to the human review step.

### Communication Standards

The language used during a review dictates how quickly an issue is resolved and directly impacts team operational efficiency.

* **Actionable Feedback:** When identifying a defect, the reviewer must state exactly what is incorrect and provide a specific, technical recommendation or code example demonstrating how to resolve it.
* **Categorized Comments:** The reviewer must clearly distinguish between critical requirements (errors that must be fixed before approval) and minor suggestions (alternative approaches the author can choose to implement or ignore).
* **Objective Language:** Feedback must strictly address the technical behavior of the code, not the individual who wrote it.

### Operational Efficiency

A pending code review acts as a strict blocker to the software delivery pipeline.

* **Prompt Responses:** Reviewers must prioritize evaluating pending code submissions to prevent integration delays and minimize the risk of merge conflicts caused by divergent code branches.
* **Time Allocation:** Human ability to detect software defects drops significantly after prolonged periods of reading code. Active reviewing should be limited to discrete time blocks, typically under 60 minutes per session, to maintain a high defect-detection rate.