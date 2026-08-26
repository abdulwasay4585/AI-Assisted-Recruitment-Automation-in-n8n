# AI Recruitment Workflow

An automated recruitment workflow built with n8n, Supabase, Google Gemini, Gmail, and a human approval step.

## Workflow

                 AI Analysis
                      |
                      v
              AI Recommendation
                      |
                      v
                Human Review
                      |
              +-------+-------+
              |               |
              v               v
          shortlist         reject
              |               |
              v               v
        Shortlisted         Rejected
              |               |
              v               v
       Supabase Update   Supabase Update
              |               |
              v               v
        Gmail Email        Gmail Email

        
## Features

### AI Candidate Analysis
Google Gemini analyzes the candidate against the Junior Machine Learning Engineer requirements and generates:

• Skills  
• AI score  
• AI recommendation  
• Reason  
• Matched requirements  
• Missing requirements  

### Human Approval Mechanism
AI recommendations are advisory only. A human reviewer makes the final decision through the approval workflow.

The final decision must be:

```text
shortlist
reject
````

The decision is then sent back to n8n and used by the IF node to select the correct workflow branch.

### Scoring

The final score combines AI and rule based evaluation:

```text
Combined Score = AI Score × 0.4 + Rule Score × 0.6
```

The rule based score evaluates required skills and minimum experience.

### Error Handling

The workflow validates the human decision before continuing.

Invalid values such as:

```text
pending
approve
declined
```

are rejected instead of being treated as a valid hiring decision.

Database updates use the candidate ID to ensure that the correct candidate record is modified.

### Database

Supabase stores:

• Candidate information
• AI analysis
• Scores
• Human decision
• Final status
• Review timestamp

Example final states:

```text
human_decision = shortlist
status = Shortlisted
```

or

```text
human_decision = reject
status = Rejected
```

### Email Notification

After the database is updated, Gmail sends the candidate an appropriate notification.

Shortlisted candidates receive a progression email.

Rejected candidates receive a rejection email.

## Requirements

• n8n
• Supabase
• Google Gemini API
• Gmail account
• Postman for testing

## Testing

Submit a candidate through the n8n form, review the AI analysis, send a human decision through the approval webhook, and verify:

```text
Human Decision → Database Update → Correct Email
```
