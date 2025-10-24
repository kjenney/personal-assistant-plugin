---
description: Find available time slots for scheduling
params:
  - name: duration
    description: Duration needed (e.g., "30min", "1hour", "2hours")
    required: false
  - name: timeframe
    description: When to look (e.g., "today", "tomorrow", "this week")
    required: false
  - name: preferences
    description: Preferences like "morning", "afternoon", "avoid lunch"
    required: false
thinking: enabled
---

Help the user find available time slots in their calendar.

**Default behavior** (no parameters): Find 1-hour slots in the next 3 days

**Parameters**:
- **duration**: Length of time needed (default: 1 hour)
- **timeframe**: When to search (default: next 3 days)
- **preferences**: Time preferences or constraints

**Analysis should include**:

1. **Available Slots**:
   - List free time blocks matching criteria
   - Group by day
   - Indicate slot duration
   - Note any adjacent meetings (to avoid context switching)

2. **Optimal Times**:
   - Highlight slots with buffer time before/after
   - Prefer slots that create longer focus blocks
   - Consider typical meeting patterns (avoid fragmenting time)

3. **Alternatives**:
   - If no perfect matches, suggest close alternatives
   - Offer to check different timeframes
   - Suggest moving non-critical meetings if needed

4. **Smart Suggestions**:
   - Best times for focused work (longer blocks)
   - Best times for quick meetings (between other meetings)
   - Times to avoid (lunch, end of day, etc.)

Present results in order of recommendation, with clear reasoning for each suggestion.
