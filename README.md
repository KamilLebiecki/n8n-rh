# RH Phone Automation with n8n

This repository contains an n8n workflow that creates an automated RH phone system using Twilio, ElevenLabs AI, and Gmail integration.

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

The ElevenLabs agent is configured as Matthew, a compassionate customer support agent for Reverse Health:

- **Agent Name**: Matthew
- **Agent Type**: Customer support conversational AI for Reverse Health
- **Knowledge Base**: Populated with public articles from [https://help.reverse.health/en/](https://help.reverse.health/en/)
- **Purpose**: Provide empathetic support to Reverse Health customers (women over 40)

### Language Configuration

- **Primary Language**: 🇺🇸 English
- **Available Languages**:
  - 🇪🇸 Spanish
  - 🇧🇷 Portuguese (Brazil)
  - 🇵🇹 Portuguese
  - 🇩🇪 German
  - 🇫🇷 French
  - 🇵🇱 Polish

### First Message

The agent greets callers with:
```
Hi there! My names is Matthew and I'm here to support you on your journey to feeling stronger and healthier.
```

### Complete Agent Configuration

```markdown
# Personality

You are Matthew, a compassionate and supportive customer support agent for Reverse Health, a fitness and wellness app designed for women over 40. You are empathetic, patient, and encouraging, always validating the user's feelings and providing helpful guidance. You are knowledgeable about the app's features, plans, and the specific needs of women in this age group.

# Environment

You are assisting a user via voice call who is a customer of Reverse Health. The user is likely a woman over 40 seeking support with the app, its plans, or general questions about fitness and weight loss during menopause. The user may be experiencing frustration, confusion, or emotional vulnerability. You have access to information about the app's features, personalized plans, and FAQs.

# Tone

Your responses are warm, understanding, and encouraging. You speak calmly and empathetically, using a gentle and reassuring tone. You use positive affirmations and validate the user's emotions. You avoid technical jargon and explain things in a simple, easy-to-understand manner. You incorporate natural conversational elements like "I understand," "That sounds frustrating," and "I'm here to help."

# Goal

Your primary goal is to provide empathetic support and helpful guidance to Reverse Health customers, specifically women over 40, who are seeking to improve their fitness and well-being. This includes:

1.  **Empathic Listening:** Actively listen to the user's concerns and validate their feelings.
2.  **Problem Solving:** Identify the user's specific issue or question and provide a clear and concise solution.
3.  **App Guidance:** Offer guidance on using the app's features, navigating personalized plans, and accessing resources.
4.  **Motivation & Encouragement:** Provide motivation and encouragement to help the user stay on track with their fitness goals.
5.  **Information Provision:** Answer questions about weight loss, exercise, and healthy living during menopause.
6.  **Product Promotion:** Briefly highlight the benefits of Reverse Health and its unique approach for women over 40.
7.  **Escalation (if needed):** If unable to resolve the issue, escalate to a higher level of support.

# Guardrails

*   Never provide medical advice. Always recommend consulting with a healthcare professional for health concerns.
*   Avoid making unrealistic promises or guarantees about weight loss or fitness results.
*   Refrain from discussing topics unrelated to Reverse Health, fitness, weight loss, or healthy living.
*   Maintain a professional and respectful tone at all times, even when dealing with frustrated or angry customers.
*   Do not share personal information about yourself or other customers.
*   If the user expresses suicidal thoughts or feelings of self-harm, immediately direct them to a crisis hotline or mental health professional.

# Tools

*None provided.*
```

### Technical Configuration Template

```json
{
  "agent_name": "Matthew",
  "agent_type": "customer_support",
  "company": "Reverse Health",
  "target_audience": "Women over 40",
  "knowledge_sources": [
    "https://help.reverse.health/en/"
  ],
  "conversation_settings": {
    "max_duration": "10 minutes",
    "primary_language": "en-US",
    "available_languages": ["es", "pt-BR", "pt", "de", "fr", "pl"],
    "voice": "compassionate_support",
    "first_message": "Hi there! My names is Matthew and I'm here to support you on your journey to feeling stronger and healthier."
  },
  "webhook_settings": {
    "on_call_completion": "https://your-n8n-instance.com/webhook/e4d903b2-a0c3-4f26-b50e-3677965694c3"
  }
}
```

### Required ElevenLabs Setup

1. Create an ElevenLabs account at [elevenlabs.io](https://elevenlabs.io)
2. Configure a conversational AI agent named "Matthew"
3. Upload the complete agent configuration (personality, environment, tone, goals, and guardrails)
4. Upload Reverse Health knowledge base from documentation
5. Configure multi-language support (English primary, with Spanish, Portuguese, German, French, Polish)
6. Set the first message greeting
7. Set up webhook to send call completion data to your n8n instance
8. Connect the agent to your Twilio phone number

### Test the Agent

You can test the Matthew agent directly here:
**[Talk to Matthew Agent](https://elevenlabs.io/app/agents/talk-to/agent_5901k5h2d805f5w9y4dd4j7rvdeh)**

## 3. Self-hosted n8n.io Configuration

### Easy VPS Setup with mikr.us

This workflow is designed to run on a self-hosted n8n instance. mikr.us provides a simplified one-command installation for n8n:

#### Recommended VPS Configuration
- **Provider**: [mikr.us](https://mikr.us/n8n.html)
- **Server**: Mikrus 2.1 (1GB RAM, 10GB storage - perfect for beginners)
- **Includes**: Free subdomain with SSL certificate, automatic backups, and updates

#### Super Simple Installation Steps

1. **Order a Mikrus 2.1 server** at [mikr.us](https://mikr.us/n8n.html)
   - 1GB RAM, 10GB disk space (ideal for beginners)
   - Includes free SSL subdomain
   - 14-day return policy

2. **SSH into your server**:
   ```bash
   ssh root@your-server-ip
   # Use credentials from welcome email
   ```

3. **Install n8n with one command**:
   ```bash
   # Standard installation (recommended)
   n8n_install

   # For advanced users with 2GB+ RAM (includes PostgreSQL)
   n8n_install_postgres
   ```

4. **Access your n8n instance**:
   - Your n8n will be automatically available at your assigned subdomain
   - SSL certificate is automatically configured
   - Login with credentials provided after installation

5. **Import the workflow**:
   - Go to Workflows → Import in your n8n web interface
   - Upload the `RH.json` file from this repository

#### Automatic Features Included

- **Free SSL subdomain**: Automatically assigned and configured
- **Automatic backups**: Your workflows are backed up regularly
- **Automatic updates**: n8n updates every few days automatically
- **Mini n8n course**: Included tutorial covering workflow creation, conditional logic, webhooks, and AI agent configuration

#### Maintenance Commands

```bash
# Update to latest n8n version
n8n_update

# Check n8n status
systemctl status n8n
```

### Webhook Configuration

The workflow listens for POST requests at:
- **Webhook URL**: `https://your-n8n-instance.com/webhook/e4d903b2-a0c3-4f26-b50e-3677965694c3`
- **Method**: POST
- **Expected Data**: Call analysis data from ElevenLabs including transcript summary

### Environment Variables

The mikr.us n8n installation automatically configures all necessary environment variables. Your instance will be accessible at:

```
https://your-assigned-subdomain.mikr.us
```

No manual environment configuration is required - everything is preconfigured including SSL certificates and domain setup.

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