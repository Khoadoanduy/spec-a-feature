## UC-TEA-nudge-non-submitters

**ID:** UC-TEA-nudge-non-submitters  
**Name:** Nudge Non-Submitters  
**Primary Actor:** Instructor  
**Trigger:** Instructor requests to view non-submitting students or trigger reminder emails for a course section, OR the system automated scheduler fires on a section's configured due day.

**Description:**  
Enables an instructor to view which students in their assigned section have not yet submitted their required weekly artifacts (Weekly Activity Report and/or Peer Evaluation) for an open submission window, and to send targeted reminder emails to those non-submitting students. The automated reminder scheduler uses the same identification logic to filter out students who have already submitted.

**Preconditions:**  
* The user is authenticated as an Instructor assigned to the target course section (BR-section-scoped-access).  
* The current week is marked as an active week for the section (BR-active-weeks).  

**Postconditions:**  
* Targeted reminder emails are dispatched exclusively to eligible non-submitting students.  
* An audit log entry is recorded for manually triggered and automated reminder dispatches.

**Main Success Scenario:**  
1. Instructor requests the list of non-submitting students for an active reporting artifact in their section.  
2. System retrieves all enrolled students assigned to teams in the section (BR-team-assignment-required).  
3. System determines non-submission status for each student according to BR-NOT-01.  
4. System displays the list of non-submitting students to the Instructor (BR-section-scoped-access, CO-ferpa).  
5. Instructor initiates a targeted reminder ("nudge") to the selected non-submitting students.  
6. System checks rate limits for manual nudges according to BR-NOT-02.  
7. System constructs and sends the reminder email to each eligible non-submitting student.  
8. System logs the number of successfully dispatched emails and reports confirmation to the Instructor.

**Extensions:**  

* **2a. A student is not assigned to a team in the course section:**  
  * 2a1. System excludes the student from non-submitter lists and nudge dispatches per BR-team-assignment-required and BR-NOT-01.  

* **3a. The submission window for the current week has closed:**  
  * 3a1. System notifies the Instructor that nudges cannot be sent for closed submission windows per BR-NOT-02.  
  * 3a2. Use case ends.  

* **3b. The current week is not an active week for the section:**  
  * 3b1. System displays an error stating no active reporting cycle exists per BR-active-weeks.  
  * 3b2. Use case ends.  

* **3c. A student previously submitted but subsequently deleted their submission:**  
  * 3c1. System identifies the absence of an active submission record and treats the student as a non-submitter per BR-NOT-01.  

* **6a. Instructor attempts to nudge the same student again within 24 hours:**  
  * 6a1. System blocks the dispatch for that student per BR-NOT-02 rate limits.  
  * 6a2. System informs the Instructor that a reminder was recently sent and indicates when the next nudge is permitted.  

* **7a. The mail server rejects a recipient's email address:**  
  * 7a1. System catches the delivery exception, logs an error with the student's email and section name, and steps over to process the next recipient.  

**Priority:** High  
**Frequency of Use:** Multiple times per week per active section  
**Business Rules:**  
* BR-section-scoped-access  
* BR-team-scoped-access  
* BR-team-assignment-required  
* BR-active-weeks  
* BR-NOT-01  
* BR-NOT-02  
* CO-ferpa  

**Associated Information:**  
* Student Email Address  
* Course Section ID  
* Submission Status / Timestamp