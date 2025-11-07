# Setup Guide

This guide will walk you through setting up the AIBot n8n Workflow step by step.

## 📋 Prerequisites

### Required Services
1. **n8n Instance** (self-hosted or n8n Cloud)
2. **Telegram Bot** (created via BotFather)
3. **OpenRouter Account** (for AI API access)

### System Requirements
- n8n version 1.0+ recommended
- Internet connection for API calls
- Valid domain/URL for webhooks (if self-hosting)

## 🤖 Step 1: Create Telegram Bot

1. Open Telegram and search for [@BotFather](https://t.me/botfather)
2. Send `/newbot` command
3. Follow prompts to name your bot
4. Save the bot token (format: `123456789:ABCdefGHIjklMNOpqrsTUVwxyz`)
5. Optional: Set bot description and profile picture

### Bot Configuration Commands
```
/setdescription - Set bot description
/setabouttext - Set about text
/setuserpic - Set profile picture
/setcommands - Set command menu
```

## 🔑 Step 2: Get OpenRouter API Key

1. Visit [OpenRouter.ai](https://openrouter.ai/)
2. Sign up or log in
3. Go to API Keys section
4. Create a new API key
5. Save the key securely (format: `sk-or-...`)

## ⚙️ Step 3: Import Workflow to n8n

### Method 1: JSON Import
1. Copy contents of `src/AIBot_Workflow.json`
2. In n8n: **Workflows** → **Add Workflow** → **Import from File**
3. Paste JSON content and click **Import**

### Method 2: Manual Creation
Follow the workflow structure in the documentation to recreate nodes manually.

## 🔐 Step 4: Configure Credentials

### Telegram API Credential
1. Go to **Credentials** → **Add Credential**
2. Select **Telegram API**
3. Enter your bot token from Step 1
4. Name it "Telegram account" (or update node references)
5. Test and save

### OpenRouter API Credential
1. Go to **Credentials** → **Add Credential**
2. Select **HTTP Header Auth**
3. Set:
   - **Name**: "Header Auth account"
   - **Header Name**: `Authorization`
   - **Header Value**: `Bearer YOUR_OPENROUTER_API_KEY`
4. Test and save

## 🔧 Step 5: Configure Workflow Nodes

### Update HTTP Request Node
In the "Call OpenRouter" node, update headers:
- **HTTP-Referer**: Your n8n instance URL
- **X-Title**: Your bot name

### Webhook Configuration
The Telegram Trigger will automatically generate webhook URLs. Ensure your n8n instance is accessible from the internet.

## ✅ Step 6: Test the Workflow

1. **Activate** the workflow in n8n
2. Send `/start` to your Telegram bot
3. Verify you receive the welcome message
4. Send a test question
5. Check that you receive an AI-generated response

### Troubleshooting
- Check n8n execution logs for errors
- Verify credentials are correctly configured
- Ensure webhook URLs are accessible
- Test API keys independently

## 🎛️ Step 7: Customization (Optional)

### Modify AI Behavior
Edit the system prompt in "Call OpenRouter" node:
```json
{
  "role": "system",
  "content": "Your custom bot personality here..."
}
```

### Adjust AI Parameters
- **Temperature**: 0.1-1.0 (creativity level)
- **Max Tokens**: Response length limit
- **Model**: Try different OpenRouter models

### Response Formatting
Modify the "Parse OpenRouter Response" function to change message formatting.

## 🚀 Step 8: Production Deployment

### Security Checklist
- [ ] Use environment variables for sensitive data
- [ ] Enable HTTPS for webhooks
- [ ] Set up proper logging
- [ ] Configure rate limiting if needed
- [ ] Monitor API usage and costs

### Monitoring
- Set up n8n workflow monitoring
- Monitor OpenRouter API usage
- Track bot performance metrics
- Set up error notifications

## 📞 Support

If you encounter issues:
1. Check the troubleshooting section
2. Review n8n execution logs
3. Verify all credentials and configurations
4. Open an issue with detailed error information

## 🔄 Updates

To update the workflow:
1. Export your current workflow (backup)
2. Import the new version
3. Reconfigure credentials if needed
4. Test thoroughly before activating