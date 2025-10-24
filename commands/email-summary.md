---
description: Summarize and prioritize emails
params:
  - name: timeframe
    description: Timeframe to check (today/yesterday/week)
    required: false
  - name: filter
    description: Filter by sender, subject, or keywords
    required: false
thinking: enabled
---

Provide an intelligent email summary for the user.

**Default behavior** (no parameters): Summarize unread emails from the last 24 hours

**With timeframe**:
- "today": Emails received today
- "yesterday": Emails from yesterday
- "week": Emails from the past 7 days

**With filter**: Apply the specified filter to narrow down emails

**For each summary**:
1. **Priority Classification**:
   - HIGH: Urgent emails, from VIPs, or with deadline keywords
   - MEDIUM: Normal work emails requiring response
   - LOW: FYI emails, newsletters, automated notifications

2. **Email Details**:
   - From/Subject
   - Brief summary (2-3 sentences)
   - Action required (if any)
   - Suggested response approach

3. **Overall Insights**:
   - Total unread count
   - Number requiring action
   - Any patterns or themes
   - Recommended next steps

Present this in a scannable format that helps the user quickly triage their inbox.
