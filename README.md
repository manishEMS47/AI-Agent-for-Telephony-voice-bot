# AI Agent for Telephony - Voice Bot

A production-ready conversational AI telephony system built on Vocode, integrating Deepgram, OpenAI, and ElevenLabs to deliver natural voice interactions for inbound and outbound phone calls.

## Overview

This AI-powered telephony solution enables businesses to deploy intelligent voice agents for various use cases including customer support, appointment scheduling, lead qualification, and information collection. The system provides seamless integration with existing telephony infrastructure through Twilio's robust platform.

### Key Features

- **Real-time Voice Processing**: Advanced speech-to-text and text-to-speech capabilities
- **Intelligent Conversation Management**: OpenAI-powered natural language understanding
- **Scalable Architecture**: Docker-based deployment with Kubernetes support
- **Comprehensive Monitoring**: Built-in Prometheus metrics and observability
- **Flexible Configuration**: Customizable agents, transcribers, and synthesizers

## Architecture

The system leverages the following technology stack:

- **Voice Processing**: Deepgram for speech transcription, ElevenLabs for synthesis
- **AI Engine**: OpenAI for conversational intelligence
- **Telephony**: Twilio for call management and routing
- **Infrastructure**: FastAPI, Redis, Docker, Kubernetes
- **Monitoring**: Prometheus metrics collection

## Prerequisites

### Required Dependencies
- Docker and Docker Compose
- Python 3.8+ with Poetry (for development)

### API Keys Required
- **Deepgram**: Speech transcription services
- **OpenAI**: Language model and conversation management
- **ElevenLabs**: Text-to-speech synthesis
- **Twilio**: Telephony services and phone number management

### Development Tools (Optional)
- Ngrok or Cloudflare Tunnels for local testing
- Kubernetes cluster for production deployment
- Helm 3.0+ for Kubernetes deployments

## Quick Start

### 1. Environment Configuration

```bash
cp .env.template .env
```

Configure your `.env` file with the required API keys:

```env
# Telephony Configuration
BASE_URI=your-domain.com
TWILIO_ACCOUNT_SID=your_twilio_sid
TWILIO_AUTH_TOKEN=your_twilio_token
FROM_PHONE=+1234567890
TO_PHONE=+1234567890

# AI Services
DEEPGRAM_API_KEY=your_deepgram_key
OPENAI_API_KEY=your_openai_key
OPENAI_BASE_URL=https://api.openai.com/v1
ELEVEN_LABS_API_KEY=your_eleven_labs_key
ELEVEN_LABS_VOICE_ID=your_voice_id
```

### 2. Local Development Setup

For local testing, expose your development server:

```bash
ngrok http 6000
```

Update your `.env` with the ngrok URL (without `https://`):
```env
BASE_URI=abc123.ngrok.app
```

### 3. Start the Application

```bash
docker-compose up --build
```

The application will be available at `http://localhost:6000`

## Telephony Configuration

### Inbound Call Setup

1. **Acquire Phone Number**
   - Log into your Twilio Console
   - Navigate to Phone Numbers → Manage → Buy a number
   - Purchase a phone number for your region

2. **Configure Webhook**
   - Go to Phone Numbers → Manage → Active Numbers
   - Select your purchased number
   - Set Webhook URL to: `https://your-domain.com/inbound_call`
   - Set HTTP method to `POST`
   - Save configuration

### Outbound Call Execution

Ensure your environment variables are configured, then execute:

```bash
poetry install
poetry run python outbound_call.py
```

## Monitoring and Observability

The system exposes Prometheus metrics on port 8000:

- `voicebot_session_count`: Total number of sessions initiated
- `voicebot_active_sessions`: Current active session count

Access metrics at: `http://localhost:8000/metrics`

## Production Deployment

### Kubernetes with Helm

#### Prerequisites
- Kubernetes cluster (EKS, GKE, AKS, or self-managed)
- Helm 3.0+
- kubectl configured for your cluster

#### Deployment Process

1. **Clone Repository**
```bash
git clone https://github.com/danieladdisonorg/AI-Agent-for-Telephony-voice-bot
cd AI-Agent-for-Telephony-voice-bot
```

2. **Create Configuration**

Create `production-values.yaml`:

```yaml
env:
  BASE_URI: "your-production-domain.com"
  DEEPGRAM_API_KEY: "your-deepgram-key"
  OPENAI_API_KEY: "your-openai-key"
  OPENAI_BASE_URL: "https://api.openai.com/v1"
  ELEVEN_LABS_API_KEY: "your-eleven-labs-key"
  ELEVEN_LABS_VOICE_ID: "your-voice-id"
  TWILIO_ACCOUNT_SID: "your-twilio-sid"
  TWILIO_AUTH_TOKEN: "your-twilio-token"
  FROM_PHONE: "your-twilio-number"
  TO_PHONE: "your-target-number"

resources:
  limits:
    cpu: 1000m
    memory: 1Gi
  requests:
    cpu: 500m
    memory: 512Mi

replicas: 3
```

3. **Deploy Application**
```bash
helm install voice-bot ./kubernetes/voice-bot-helm/voice-bot -f production-values.yaml
```

4. **Verify Deployment**
```bash
kubectl get pods -l app=voice-bot
kubectl get services voice-bot
```

#### Management Commands

**Update Deployment:**
```bash
helm upgrade voice-bot ./kubernetes/voice-bot-helm/voice-bot -f production-values.yaml
```

**Remove Deployment:**
```bash
helm uninstall voice-bot
```

**View Logs:**
```bash
kubectl logs -f deployment/voice-bot
```

## Configuration Options

### Agent Configuration
Customize conversation behavior through `AgentConfig` parameters in your deployment configuration.

### Transcriber Options
- **Default**: Deepgram (recommended for production)
- **Alternative**: Configure custom transcription services

### Synthesizer Options
- **Default**: ElevenLabs (high-quality voice synthesis)
- **Alternative**: Configure alternative TTS providers

## Troubleshooting

### Common Issues

**Connection Problems:**
- Verify all API keys are valid and have sufficient credits
- Check network connectivity and firewall rules
- Ensure webhook URLs are publicly accessible

**Audio Quality Issues:**
- Verify Deepgram and ElevenLabs configurations
- Check network latency and bandwidth
- Review audio codec settings

**Deployment Issues:**
```bash
# Check pod status
kubectl describe pod <pod-name>

# View detailed logs
kubectl logs <pod-name> --previous

# Check service connectivity
kubectl port-forward service/voice-bot 6000:3000
```

## Support and Contributing

For issues, feature requests, or contributions, please visit the [GitHub repository](https://github.com/danieladdisonorg/AI-Agent-for-Telephony-voice-bot).

## License

This project is licensed under the MIT License. See the LICENSE file for details.
