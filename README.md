# Personal Assistant Plugin for Claude Code

Transform Claude Code into your intelligent personal assistant with seamless Gmail and Google Calendar integration. This plugin helps you manage emails, coordinate meetings, and stay organized throughout your day.

## Features

### Email Management
- **Smart Email Summaries**: Get prioritized summaries of your inbox
- **Context-Aware Responses**: Draft responses based on email thread context
- **Advanced Search**: Find emails using natural language queries
- **Priority Detection**: Automatically identifies urgent and important messages

### Calendar Management
- **Schedule Overview**: View your daily, weekly schedule at a glance
- **Smart Scheduling**: Find optimal time slots for meetings and focus work
- **Meeting Preparation**: Get context and prep materials for upcoming meetings
- **Conflict Detection**: Automatically identifies scheduling conflicts

### Personal Assistant Commands

#### `/morning-brief`
Start your day right with a comprehensive morning briefing:
- Today's calendar events and schedule
- Important email summaries
- Action items requiring attention
- Recommendations for optimal time management

#### `/schedule [action] [details]`
Manage your calendar effortlessly:
- View schedules by day, week, or date range
- Create new events with natural language
- Update or delete existing events
- Check for conflicts

#### `/email-summary [timeframe] [filter]`
Triage your inbox intelligently:
- Prioritized email summaries (HIGH/MEDIUM/LOW)
- Filter by sender, subject, or keywords
- Actionable recommendations
- Suggested response approaches

#### `/meeting-prep [meeting]`
Never go into a meeting unprepared:
- Meeting details and attendees
- Relevant email context
- Recent communication history
- Talking points and recommendations

#### `/free-time [duration] [timeframe] [preferences]`
Find the perfect time for any task:
- Discover available time slots
- Smart recommendations for focus time
- Respects your scheduling preferences
- Avoids fragmentation

## Quick Start

1. **Install the plugin**:
   ```bash
   claude plugin install /path/to/personal-assistant-plugin
   ```

2. **Set up Google Cloud credentials** (see [SETUP.md](SETUP.md))

3. **Authenticate with Google** on first use

4. **Start using**:
   ```
   /morning-brief
   ```

## Setup

See [SETUP.md](SETUP.md) for detailed installation and configuration instructions.

## Requirements

- Claude Code
- Node.js and npm
- Google account
- Google Cloud Platform account (free tier)

## MCP Servers

This plugin uses two Model Context Protocol servers:
- **Gmail MCP** (`@gongrzhe/server-gmail-autoauth-mcp`) - For email integration with automatic OAuth authentication
- **Google Calendar MCP** (`@cocal/google-calendar-mcp`) - For calendar integration

Both servers are automatically configured and managed by the plugin.

## Privacy & Security

- OAuth 2.0 authentication with encrypted token storage
- No data is stored or transmitted outside of Google and Claude
- Minimal required permissions (read/modify emails, manage calendar)
- All operations require explicit user consent

## How It Works

The Personal Assistant plugin combines:
1. **MCP Servers**: Connect to Gmail and Google Calendar APIs
2. **Slash Commands**: Provide structured workflows for common tasks
3. **Hooks**: Add contextual awareness and automation
4. **Claude's Intelligence**: Understand natural language and provide smart recommendations

## Example Usage

**Morning routine**:
```
/morning-brief
```

**During the day**:
```
What emails do I need to respond to?
/meeting-prep next
Show me my afternoon schedule
```

**Scheduling**:
```
/free-time 2hours tomorrow afternoon
Schedule a meeting with John next week for 1 hour
```

**End of day**:
```
/email-summary today
What do I have tomorrow morning?
```

## Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

## License

MIT License - See LICENSE file for details

## Support

For issues and questions:
- Check [SETUP.md](SETUP.md) for troubleshooting
- Review [Claude Code documentation](https://docs.claude.com/en/docs/claude-code)
- File an issue in this repository