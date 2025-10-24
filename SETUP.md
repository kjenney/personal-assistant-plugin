# Personal Assistant Plugin - Setup Guide

This guide will walk you through setting up the Personal Assistant plugin for Claude Code, which integrates Gmail and Google Calendar.

## Prerequisites

- Claude Code installed and configured
- Node.js and npm installed
- A Google account
- Google Cloud Platform account (free tier is sufficient)

## Step 1: Google Cloud Setup

### 1.1 Create a Google Cloud Project

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Click "Select a project" → "New Project"
3. Name your project (e.g., "Claude Personal Assistant")
4. Note your Project ID

### 1.2 Enable Required APIs

1. In your project, go to "APIs & Services" → "Library"
2. Search for and enable:
   - **Gmail API**
   - **Google Calendar API**

### 1.3 Configure OAuth Consent Screen

1. Go to "APIs & Services" → "OAuth consent screen"
2. Select "External" user type
3. Fill in required fields:
   - App name: "Claude Personal Assistant"
   - User support email: Your email
   - Developer contact: Your email
4. Click "Save and Continue"
5. On "Scopes" page, click "Add or Remove Scopes"
6. Add these scopes:
   - `https://www.googleapis.com/auth/gmail.readonly`
   - `https://www.googleapis.com/auth/gmail.send`
   - `https://www.googleapis.com/auth/gmail.modify`
   - `https://www.googleapis.com/auth/calendar`
   - `https://www.googleapis.com/auth/calendar.events`
   - `openid`
7. Click "Save and Continue"
8. Add yourself as a test user (your email address)
9. Click "Save and Continue"

### 1.4 Create OAuth 2.0 Credentials

1. Go to "APIs & Services" → "Credentials"
2. Click "Create Credentials" → "OAuth client ID"
3. Select "Desktop app" as application type
4. Name it "Claude Desktop Client"
5. Click "Create"
6. **Important**: Download the JSON file and save it securely
7. Note your **Client ID** and **Client Secret**

## Step 2: Configure Environment Variables

You'll need to set up environment variables for the MCP servers. Create a secure location for your credentials.

### 2.1 For Gmail MCP

1. Generate an encryption key:
```bash
python3 -c "from cryptography.fernet import Fernet; print(Fernet.generate_key().decode())"
```

2. Save this key securely - you'll need it for the `TOKEN_ENCRYPTION_KEY` variable

### 2.2 For Google Calendar MCP

1. Save your downloaded OAuth credentials JSON file to a secure location
2. Note the full path to this file

### 2.3 Set Environment Variables

Add these to your shell profile (`~/.zshrc`, `~/.bashrc`, or `~/.bash_profile`):

```bash
# Google OAuth Credentials
export GOOGLE_CLIENT_ID="your-client-id.apps.googleusercontent.com"
export GOOGLE_CLIENT_SECRET="your-client-secret"
export TOKEN_ENCRYPTION_KEY="your-generated-encryption-key"
export GOOGLE_OAUTH_CREDENTIALS="/path/to/your/gcp-oauth.keys.json"
```

**Reload your shell**:
```bash
source ~/.zshrc  # or ~/.bashrc
```

## Step 3: Install the Plugin

### Option A: Install from Local Directory

1. Clone or download this repository
2. Navigate to the plugin directory
3. Install the plugin:
```bash
cd /path/to/personal-assistant-plugin
claude plugin install .
```

### Option B: Install from Git Repository

```bash
claude plugin install https://github.com/yourusername/personal-assistant-plugin
```

## Step 4: Verify Installation

1. Start Claude Code
2. Check that the plugin is loaded:
```bash
claude plugin list
```

3. You should see "personal-assistant" in the list

## Step 5: Initial Authentication

### Gmail Authentication

1. In Claude Code, try a command that uses Gmail:
```
Can you check my recent emails?
```

2. The first time, you'll be redirected to a browser for OAuth authentication
3. Sign in with your Google account
4. Grant the requested permissions
5. You'll be redirected back - authentication is complete

### Calendar Authentication

1. Try a calendar command:
```
What's on my calendar today?
```

2. Follow the OAuth flow similar to Gmail
3. Grant calendar permissions

## Step 6: Test the Slash Commands

Try out the personal assistant commands:

```
/morning-brief
```
- Gets your morning briefing with emails and calendar

```
/schedule
```
- Shows your calendar schedule

```
/email-summary
```
- Summarizes your recent emails

```
/meeting-prep
```
- Prepares you for your next meeting

```
/free-time 1hour
```
- Finds 1-hour free slots in your calendar

## Troubleshooting

### Authentication Issues

If you encounter authentication problems:

1. **Check credentials**: Verify your environment variables are set correctly
2. **Token location**: Tokens are stored in Claude Code's config directory
3. **Re-authenticate**: Remove token files and authenticate again
4. **OAuth redirect URI**: Ensure it matches your Cloud Console settings

### MCP Server Issues

If MCP servers aren't loading:

1. **Check installation**:
```bash
npx @smithery/cli doctor
```

2. **Verify environment variables**:
```bash
echo $GOOGLE_CLIENT_ID
echo $GOOGLE_OAUTH_CREDENTIALS
```

3. **Check Claude Code logs**:
```bash
claude logs
```

### Permission Errors

If you get permission errors:

1. Verify all required scopes are added in OAuth consent screen
2. Remove existing tokens and re-authenticate
3. Ensure your email is added as a test user

### Rate Limits

If you hit rate limits:

1. Ensure your OAuth credentials JSON includes `project_id`
2. Consider enabling billing in Google Cloud (still free for normal use)
3. Be mindful of API quotas

## Security Best Practices

1. **Never commit credentials**: Keep OAuth files and environment variables secure
2. **Use encryption**: The Gmail MCP uses encrypted token storage
3. **Regular rotation**: Periodically rotate your OAuth credentials
4. **Minimal scopes**: Only grant necessary permissions
5. **Monitor access**: Regularly check Google account activity

## Updating the Plugin

To update to the latest version:

```bash
claude plugin update personal-assistant
```

Or reinstall from the repository:

```bash
claude plugin uninstall personal-assistant
claude plugin install https://github.com/yourusername/personal-assistant-plugin
```

## Getting Help

If you encounter issues:

1. Check the [Claude Code documentation](https://docs.claude.com/en/docs/claude-code)
2. Review [MCP documentation](https://modelcontextprotocol.io/)
3. Check plugin logs: `claude logs`
4. File an issue in the plugin repository

## What's Next?

Now that your personal assistant is set up, you can:

- Run `/morning-brief` each morning to start your day
- Use `/meeting-prep` before important meetings
- Check `/email-summary` to triage your inbox
- Find time with `/free-time` for scheduling
- Ask natural language questions about your calendar and emails

Enjoy your AI-powered personal assistant!
