# API Reference

This document provides detailed information about the APIs and configurations used in the AIBot n8n Workflow.

## 🤖 OpenRouter API

### Base URL
```
https://openrouter.ai/api/v1/chat/completions
```

### Authentication
- **Method**: Bearer Token
- **Header**: `Authorization: Bearer sk-or-...`

### Request Format
```json
{
  "model": "openrouter/polaris-alpha",
  "messages": [
    {
      "role": "system",
      "content": "System prompt"
    },
    {
      "role": "user", 
      "content": "User message"
    }
  ],
  "temperature": 0.6,
  "max_tokens": 512
}
```

### Response Format
```json
{
  "choices": [
    {
      "message": {
        "role": "assistant",
        "content": "AI response text"
      }
    }
  ],
  "usage": {
    "prompt_tokens": 50,
    "completion_tokens": 100,
    "total_tokens": 150
  }
}
```

### Available Models
- `openrouter/polaris-alpha` - Primary model used
- `anthropic/claude-3-haiku` - Alternative option
- `openai/gpt-3.5-turbo` - Alternative option
- `meta-llama/llama-3.1-8b-instruct` - Alternative option

### Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `model` | string | required | Model identifier |
| `messages` | array | required | Conversation messages |
| `temperature` | float | 0.6 | Creativity (0.0-1.0) |
| `max_tokens` | integer | 512 | Response length limit |
| `top_p` | float | 1.0 | Nucleus sampling |
| `frequency_penalty` | float | 0.0 | Repetition penalty |

## 📱 Telegram Bot API

### Webhook Configuration
The workflow uses webhooks for real-time message processing.

### Message Structure
```json
{
  "update_id": 277351169,
  "message": {
    "message_id": 38,
    "from": {
      "id": 1926282123,
      "is_bot": false,
      "first_name": "User",
      "username": "username"
    },
    "chat": {
      "id": 1926282123,
      "type": "private"
    },
    "date": 1762533484,
    "text": "User message"
  }
}
```

### Send Message API
```json
{
  "chat_id": 1926282123,
  "text": "Response message",
  "parse_mode": "MarkdownV2"
}
```

### Supported Message Types
- Text messages
- Commands (starting with `/`)
- Code blocks and inline code
- Formatted text with MarkdownV2

## 🔧 n8n Workflow Configuration

### Node Types Used

#### 1. Telegram Trigger
- **Type**: `n8n-nodes-base.telegramTrigger`
- **Version**: 1
- **Updates**: `["message"]`

#### 2. Function Node (Prep Message)
- **Type**: `n8n-nodes-base.function`
- **Purpose**: Message preprocessing
- **Input**: Telegram update object
- **Output**: Structured message data

#### 3. HTTP Request Node (Call OpenRouter)
- **Type**: `n8n-nodes-base.httpRequest`
- **Version**: 4.1
- **Method**: POST
- **Timeout**: 30000ms

#### 4. Function Node (Parse Response)
- **Type**: `n8n-nodes-base.function`
- **Purpose**: Response formatting
- **Features**: MarkdownV2 escaping, code formatting

#### 5. Telegram Node (Send Message)
- **Type**: `n8n-nodes-base.telegram`
- **Version**: 1
- **Action**: Send message

### Environment Variables

| Variable | Description | Example |
|----------|-------------|---------|
| `TELEGRAM_BOT_TOKEN` | Bot token from BotFather | `123456789:ABC...` |
| `OPENROUTER_API_KEY` | OpenRouter API key | `sk-or-...` |
| `N8N_WEBHOOK_URL` | n8n instance URL | `https://your-n8n.com` |

### Error Handling

The workflow includes error handling for:
- Invalid API responses
- Network timeouts
- Malformed messages
- Authentication failures

### Rate Limiting

Consider implementing rate limiting for:
- API calls per minute
- Messages per user
- Total workflow executions

## 📊 Monitoring & Logging

### Key Metrics to Track
- Workflow execution count
- API response times
- Error rates
- Token usage
- User engagement

### Log Levels
- **INFO**: Normal operations
- **WARN**: Recoverable errors
- **ERROR**: Critical failures

### Health Checks
- Webhook connectivity
- API key validity
- Model availability
- Response formatting

## 🔒 Security Considerations

### API Key Management
- Store keys in n8n credentials
- Use environment variables
- Rotate keys regularly
- Monitor usage

### Webhook Security
- Use HTTPS endpoints
- Validate webhook signatures
- Implement rate limiting
- Monitor for abuse

### Data Privacy
- Don't log sensitive user data
- Implement data retention policies
- Follow GDPR/privacy regulations
- Secure credential storage