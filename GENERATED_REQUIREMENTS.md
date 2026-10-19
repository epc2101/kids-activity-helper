# Kids Activity Helper — Application Requirements & Creation Plan

## 1. Vision & Overview
Mommy Helper automates the management of school and after-school extracurricular communications. By auto-forwarding emails (including PDFs and flyers) from specific senders to a dedicated address, parents receive a clean, aggregated list of actionable items with deadlines and calendar events, complete with optional weekly email summaries. 

I do not want to give AI access to read through all of my email so this is an alterantive. 

This doc was generated based on a requirements doc that I fed to my AI assistant. 

## 2. Core Target Audience
* Parents of school-aged children (K-12).
* Caregivers helping with daily/weekly scheduling.

## 3. Key User Stories
1. **Gmail Rule Generation**: Get guided assistance creating custom Gmail search queries & auto-forwarding rules.
2. **Dedicated Email Ingestion**: Forward emails from teachers, principals, and programs to a dedicated forwarding address.
3. **Action Item Analysis**: Instant determination of whether an email requires action (e.g., "Wear PJs on Tuesday", "Bring field trip form").
4. **Deadline & Event Reminders**: Automatically capture signup windows and deadlines.
5. **Calendar Integration**:
   * Option A: Manually add individual items to Google Calendar.
   * Option B: Automatically sync all parsed events with dedicated color coding.
6. **Multi-Child Filtering**: Parse and categorize activities by child/grade level when forwarding emails for multiple kids.
7. **Weekly Digest**: Receive an optional weekly email summary highlighting upcoming actions for the week.

## 4. Functional Specifications

### A. Onboarding & Privacy
* **Account Setup**: Simple registration with step-by-step setup guides for Gmail forwarding and Calendar connection.
* **Child Profiles**: Setup nickname/first name and grade level only. *Strict privacy guideline: No full names, school addresses, or PII stored.*
* **Email Verification**: Required email verification on account creation.

### B. Email Ingestion & Rules
* **Query Generator**: User inputs target senders/keywords -> App generates exact Gmail filter query string.
* **Inbound Webhook**: Dedicated inbound email parser address.
* **Attachment Processing**: Extracts text and images/PDFs from email bodies and attachments.
* **Sender Verification**: Verifies incoming forwarder matches registered parent email.
* **Data Retention**: Emails are purged after parsing; only structured action items map to first names.

### C. Gemini AI Parser Architecture
The parser uses **Google Gemini 1.5** with Structured JSON Output mode to categorize emails into:
1. `NO_ACTION`: Informational only.
2. `EVENT`: Sports games, school plays, parent meetings (filtered/tagged by grade).
3. `REGISTRATION`: Signups for camp, sports, activities with hard deadlines.
4. `VOLUNTEER`: Volunteer signups and form requests.

**Output Schema Extracted:**
* Action Required (Boolean)
* Summary / Action Title
* Action Type / Category Label
* Specific Deadline Date & Event Dates
* Applicable Child (mapped by Grade / First Name)
* Key Details & Link back to original message

### D. Action List & Calendar Sync
* **Dashboard Action List**: Filterable view by child, deadline, or category.
* **Google Calendar Sync**:
  * One-click manual add or background auto-sync.
  * Dedicated color identifier for "Parent Reminders".