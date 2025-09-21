# Healthcare Phone Automation with n8n

This repository contains an n8n workflow that creates an automated healthcare phone system using Twilio, ElevenLabs AI, and Gmail integration.

## System Overview

The workflow creates a complete phone-to-email automation system:
1. **Twilio** provides the phone number and call routing
2. **ElevenLabs** handles AI-powered voice conversations
3. **n8n** processes the call data and triggers email notifications
4. **Gmail** sends conversation summaries to designated recipients

## 1. Twilio.com Configuration

### Phone Number Provisioning

The system uses a Twilio phone number that routes calls to the ElevenLabs AI agent:

- **Primary Number**: +1 430 243 2584
- **Vanity Number**: +1 430 2 HEALTH
- **Purpose**: Receives incoming calls and routes them to ElevenLabs Agent

### Setup Requirements

1. Create a Twilio account at [twilio.com](https://twilio.com)
2. Purchase or provision the phone number +1 430 243 2584
3. Configure the phone number to forward calls to your ElevenLabs agent
4. Set up webhook endpoints to capture call completion data

## 2. ElevenLabs.io AI Agent Configuration

### Agent Setup

The ElevenLabs agent is configured as a healthcare assistant with the following specifications:

- **Agent Type**: Healthcare conversational AI
- **Knowledge Base**: Populated with public articles from [https://help.reverse.health/en/](https://help.reverse.health/en/)
- **Purpose**: Provide healthcare information and support to callers

### Agent Configuration Template

```json
{
  "agent_type": "healthcare_assistant",
  "knowledge_sources": [
    "https://help.reverse.health/en/"
  ],
  "conversation_settings": {
    "max_duration": "10 minutes",
    "language": "en-US",
    "voice": "healthcare_professional"
  },
  "webhook_settings": {
    "on_call_completion": "https://your-n8n-instance.com/webhook/e4d903b2-a0c3-4f26-b50e-3677965694c3"
  }
}
```

### Required ElevenLabs Setup

1. Create an ElevenLabs account at [elevenlabs.io](https://elevenlabs.io)
2. Configure a conversational AI agent
3. Upload healthcare knowledge base from Reverse Health documentation
4. Set up webhook to send call completion data to your n8n instance
5. Connect the agent to your Twilio phone number

## 3. Self-hosted n8n.io Configuration

### VPS Setup with mikr.us

This workflow is designed to run on a self-hosted n8n instance. We recommend using mikr.us for VPS hosting:

#### VPS Requirements
- **Provider**: [mikr.us](https://mikr.us)
- **Minimum Specs**: 1 CPU, 1GB RAM, 10GB storage
- **OS**: Ubuntu 20.04 LTS or newer
- **Node.js**: Version 16 or higher

#### n8n Installation Steps

1. **Provision VPS on mikr.us**:
   ```bash
   # Update system
   sudo apt update && sudo apt upgrade -y

   # Install Node.js
   curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
   sudo apt-get install -y nodejs
   ```

2. **Install n8n**:
   ```bash
   # Install n8n globally
   npm install n8n -g

   # Create n8n user and directory
   sudo useradd -m n8n
   sudo mkdir /home/n8n/.n8n
   sudo chown n8n:n8n /home/n8n/.n8n
   ```

3. **Configure n8n as a service**:
   ```bash
   # Create systemd service file
   sudo nano /etc/systemd/system/n8n.service
   ```

4. **Import the workflow**:
   - Access your n8n instance via web browser
   - Go to Workflows → Import
   - Upload the `RH.json` file from this repository

### Webhook Configuration

The workflow listens for POST requests at:
- **Webhook URL**: `https://your-n8n-instance.com/webhook/e4d903b2-a0c3-4f26-b50e-3677965694c3`
- **Method**: POST
- **Expected Data**: Call analysis data from ElevenLabs including transcript summary

### Environment Variables

Set the following environment variables for your n8n instance:

```bash
export N8N_HOST=0.0.0.0
export N8N_PORT=5678
export N8N_PROTOCOL=https
export N8N_BASIC_AUTH_ACTIVE=true
export N8N_BASIC_AUTH_USER=your_username
export N8N_BASIC_AUTH_PASSWORD=your_secure_password
export WEBHOOK_URL=https://your-domain.com
```

## 4. Gmail Node Configuration

### SMTP Setup

The Gmail node sends automated email summaries of phone conversations:

- **Service**: Gmail SMTP
- **Credential ID**: UrZ558uaCpU0IBio
- **Email Format**: Plain text
- **From Address**: N8n <kamil.lebiecki@gmail.com>
- **To Address**: kamil.lebiecki@icloud.com

### Email Template

The automated emails include:

**Subject**: `[Call Time] [Caller ID]`
- Uses dynamic variables from the webhook data
- Format: `{{ system__time }} {{ system__caller_id }}`

**Body**:
```
Sending summary of the conversation:

{{ transcript_summary }}
```

### Gmail SMTP Configuration Steps

1. **Enable 2-Factor Authentication** on your Gmail account
2. **Generate App Password**:
   - Go to Google Account settings
   - Security → 2-Step Verification → App passwords
   - Generate password for "Mail"

3. **Configure n8n Gmail Credentials**:
   - **SMTP Server**: smtp.gmail.com
   - **Port**: 587
   - **Security**: STARTTLS
   - **Username**: your-gmail@gmail.com
   - **Password**: [app-password-from-step-2]

## Workflow Execution

### Data Flow

1. **Incoming Call** → Twilio number (+1 430 243 2584)
2. **Call Routing** → ElevenLabs AI agent
3. **AI Conversation** → Healthcare assistance using Reverse Health knowledge base
4. **Call Completion** → ElevenLabs sends webhook to n8n
5. **Data Processing** → n8n extracts call summary and metadata
6. **Email Notification** → Gmail sends summary to configured recipient

### Webhook Payload Structure

Expected JSON structure from ElevenLabs:

```json
{
  "body": {
    "data": {
      "conversation_initiation_client_data": {
        "dynamic_variables": {
          "system__time": "2024-01-15 14:30:00",
          "system__caller_id": "+1234567890"
        }
      },
      "analysis": {
        "transcript_summary": "Patient called regarding chronic pain management..."
      }
    }
  }
}
```

## Security Considerations

- Ensure HTTPS is enabled for your n8n instance
- Use strong authentication for n8n web interface
- Regularly rotate Gmail app passwords
- Monitor webhook endpoint for unauthorized access
- Consider implementing rate limiting on the webhook endpoint

## Troubleshooting

### Common Issues

1. **Webhook not receiving data**: Check ElevenLabs webhook configuration
2. **Email not sending**: Verify Gmail SMTP credentials and app password
3. **n8n not accessible**: Check firewall settings and port configuration
4. **Missing call data**: Verify Twilio → ElevenLabs integration

### Logs and Monitoring

- n8n execution logs: Available in the n8n web interface
- System logs: `sudo journalctl -u n8n -f`
- Webhook testing: Use tools like ngrok for local development

## Support

For issues related to:
- **n8n**: [n8n Community](https://community.n8n.io/)
- **Twilio**: [Twilio Support](https://support.twilio.com/)
- **ElevenLabs**: [ElevenLabs Documentation](https://docs.elevenlabs.io/)
- **mikr.us VPS**: [mikr.us Support](https://mikr.us/support)