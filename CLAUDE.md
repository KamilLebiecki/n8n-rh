# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This repository contains an n8n workflow configuration for a healthcare phone automation system. The workflow connects:

1. **Twilio** (+1 430 243 2584) - Phone number for incoming calls
2. **ElevenLabs AI Agent** - Handles voice conversations with healthcare knowledge
3. **n8n Webhook** - Processes call data and conversation summaries
4. **Gmail SMTP** - Sends email summaries of conversations

## Workflow Architecture

The main workflow file is `RH.json` which contains:

- **Webhook Node**: Listens for POST requests from ElevenLabs containing call analysis data
- **Email Send Node**: Configured to send conversation summaries via Gmail SMTP
- **Sticky Notes**: Documentation explaining each component's purpose

## Key Data Flow

1. Caller dials Twilio number
2. Twilio routes to ElevenLabs AI agent
3. ElevenLabs agent conducts healthcare conversation using knowledge from https://help.reverse.health/en/
4. After call completion, ElevenLabs sends POST webhook with conversation analysis
5. n8n processes the webhook data and extracts transcript summary
6. Email is sent to configured recipient with call timestamp, caller ID, and conversation summary

## Configuration Details

- **Webhook Path**: `e4d903b2-a0c3-4f26-b50e-3677965694c3`
- **Email Format**: Plain text
- **SMTP Credentials**: Gmail SMTP (credential ID: UrZ558uaCpU0IBio)
- **Email Subject**: Includes system time and caller ID from webhook data
- **Email Body**: Contains the conversation transcript summary

## Working with this Repository

Since this is a workflow configuration file rather than source code:

- The main artifact is `RH.json` - an n8n workflow export
- No build process, dependencies, or testing frameworks are present
- Modifications should be made through the n8n web interface rather than direct JSON editing
- To deploy: Import `RH.json` into an n8n instance
- Ensure SMTP credentials and webhook URLs are properly configured in your n8n environment

## Security Considerations

- The workflow contains credential references that must be configured in the target n8n instance
- Webhook endpoint should be secured appropriately
- Email addresses and phone numbers are hardcoded and should be reviewed for production use