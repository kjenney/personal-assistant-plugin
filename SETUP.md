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
3. Select **"Desktop app"** as application type (or "Web application" with redirect URI `http://localhost:3000/oauth2callback`)
4. Name it "Claude Desktop Client"
5. Click "Create"
6. **Important**: Click "Download JSON" and save the file
7. Rename the downloaded file to `gcp-oauth.keys.json`

## Step 2: Configure Gmail OAuth

### 2.1 Set Up Gmail MCP Authentication

1. Create the Gmail MCP configuration directory:
```bash
mkdir -p ~/.gmail-mcp
```

2. Move your OAuth keys file to this directory:
```bash
mv ~/Downloads/gcp-oauth.keys.json ~/.gmail-mcp/
```

3. Run the authentication setup:
```bash
npx @gongrzhe/server-gmail-autoauth-mcp auth
```

4. Follow the browser prompts to authenticate with your Google account
5. Grant the requested Gmail permissions
6. Credentials will be stored automatically in `~/.gmail-mcp/credentials.json`

### 2.2 Configure Google Calendar MCP (Optional)

If you need to set a specific OAuth credentials path for Google Calendar:

```bash
# Add to your shell profile (~/.zshrc, ~/.bashrc, or ~/.bash_profile)
export GOOGLE_OAUTH_CREDENTIALS="$HOME/.gmail-mcp/gcp-oauth.keys.json"
```

**Reload your shell**:
```bash
source ~/.zshrc  # or ~/.bashrc
```

Note: The Google Calendar MCP will handle authentication automatically on first use.

## Step 3: Install the Plugin

### For Local Development

1. Clone this repository:
```bash
git clone https://github.com/kjenney/personal-assistant-plugin
cd personal-assistant-plugin
```

2. Add the local marketplace:
```bash
claude plugin marketplace add ./marketplace.json
```

3. Install the plugin from the local marketplace:
```bash
claude plugin install personal-assistant@personal-assistant-dev
```

### From a Published Marketplace (Future)

Once published to a marketplace, you'll be able to install with:
```bash
claude plugin install personal-assistant@marketplace-name
```

## Step 4: Verify Installation

1. The plugin should be successfully installed. You can verify by checking if the slash commands are available.

2. Try running one of the plugin commands (after completing authentication setup in Step 5):
```
/morning-brief
```

## Step 5: Verify Authentication

### Gmail Authentication

Gmail authentication was completed in Step 2. To verify it's working:

1. In Claude Code, try a command that uses Gmail:
```
Can you check my recent emails?
```

2. If authentication was successful, you should see your emails
3. If you need to re-authenticate, run:
```bash
npx @gongrzhe/server-gmail-autoauth-mcp auth
```

### Calendar Authentication

1. Try a calendar command:
```
What's on my calendar today?
```

2. On first use, you'll be redirected to a browser for OAuth authentication
3. Sign in with your Google account
4. Grant the requested calendar permissions
5. Authentication is complete

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

1. **Gmail authentication**:
   - Verify `~/.gmail-mcp/gcp-oauth.keys.json` exists and is valid
   - Re-authenticate: `npx @gongrzhe/server-gmail-autoauth-mcp auth`
   - Check that credentials are stored in `~/.gmail-mcp/credentials.json`
   - Ensure OAuth client type is "Desktop app" or "Web application" with correct redirect URI

2. **Calendar authentication**:
   - Token location: Tokens are stored in Claude Code's config directory
   - Re-authenticate: Remove token files and authenticate again
   - OAuth redirect URI: Ensure it matches your Cloud Console settings

### MCP Server Issues

If MCP servers aren't loading:

1. **Check installation**:

* Run `claude` 
* Type `/mcp`
* Check status of MCP servers

2. **Verify Gmail MCP configuration**:
```bash
# Check that OAuth keys file exists
ls -l ~/.gmail-mcp/gcp-oauth.keys.json

# Check that credentials were created
ls -l ~/.gmail-mcp/credentials.json

# Optional: Check Calendar environment variable if set
echo $GOOGLE_OAUTH_CREDENTIALS
```

3. **Check Claude Code logs**:
```bash
claude --verbose
```
(Use the --verbose flag with any claude command to enable detailed logging)

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

1. **Never commit credentials**: Keep OAuth files (`gcp-oauth.keys.json`) and stored credentials secure
2. **Secure storage**: OAuth keys and credentials are stored in `~/.gmail-mcp/` - keep this directory protected
3. **Use encryption**: The Gmail MCP server automatically handles secure token storage
4. **Regular rotation**: Periodically rotate your OAuth credentials in Google Cloud Console
5. **Minimal scopes**: Only grant necessary permissions
6. **Monitor access**: Regularly check Google account activity and authorized applications

## Updating the Plugin

To update to the latest version (for local development):

```bash
# Pull the latest changes
git pull

# Reinstall the plugin
claude plugin uninstall personal-assistant
claude plugin install personal-assistant@personal-assistant-dev
```

Or from a published marketplace:

```bash
claude plugin uninstall personal-assistant
claude plugin install personal-assistant@marketplace-name
```

## Getting Help

If you encounter issues:

1. Check the [Claude Code documentation](https://docs.claude.com/en/docs/claude-code)
2. Review [MCP documentation](https://modelcontextprotocol.io/)
3. Check plugin logs: `claude --verbose`
4. File an issue in the plugin repository

## What's Next?

Now that your personal assistant is set up, you can:

- Run `/morning-brief` each morning to start your day
- Use `/meeting-prep` before important meetings
- Check `/email-summary` to triage your inbox
- Find time with `/free-time` for scheduling
- Ask natural language questions about your calendar and emails

Enjoy your AI-powered personal assistant!
