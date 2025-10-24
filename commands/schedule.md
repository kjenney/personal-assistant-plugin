---
description: View calendar and manage scheduling
params:
  - name: action
    description: Action to perform (view/create/update/delete)
    required: false
  - name: details
    description: Event details or date range
    required: false
thinking: enabled
---

Help the user manage their calendar.

**If no parameters provided**: Show today's schedule and the next 3 days

**If action is "view"**:
- List events for the specified date range or period
- Highlight any conflicts
- Show free/busy status

**If action is "create"**:
- Parse the details to create a new calendar event
- Check for conflicts with existing events
- Suggest alternative times if conflicts exist
- Confirm details before creating

**If action is "update"**:
- Find the event to update
- Apply the requested changes
- Confirm before updating

**If action is "delete"**:
- Find the event to delete
- Confirm before deletion

**Always**:
- Use natural language processing to understand user intent
- Present information in a clear, easy-to-read format
- Proactively suggest improvements to scheduling
