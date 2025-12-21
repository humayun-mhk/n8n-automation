# N8N Workflows Portfolio

<div align="center">

[![N8N](https://img.shields.io/badge/N8N-0.220%2B-red?logo=n8n&logoColor=white)](https://n8n.io/)
[![OpenAI](https://img.shields.io/badge/OpenAI-GPT--4-green?logo=openai&logoColor=white)](https://openai.com/)
[![ElevenLabs](https://img.shields.io/badge/ElevenLabs-TTS-blue?logo=elevenlabs&logoColor=white)](https://elevenlabs.io/)
[![No-Code](https://img.shields.io/badge/No--Code-Automation-purple?logo=workflow&logoColor=white)](https://n8n.io/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

A collection of advanced **N8N workflow automation systems** showcasing AI integration, multi-agent orchestration, and enterprise automation. Each workflow demonstrates production-ready automation patterns with AI/LLM capabilities.

[Workflows](#-workflows) • [Technologies](#-technologies) • [Quick Start](#quick-start) • [Integration Guide](#integration-guide) • [Use Cases](#-use-cases)

</div>

---

## 📋 Overview

This portfolio contains **7 sophisticated N8N workflows** demonstrating expertise in:
- **AI/LLM Integration** (OpenAI GPT-4, Claude)
- **Voice Processing** (ElevenLabs TTS/STT, Elevenlabs)
- **Multi-Agent Orchestration** (Agent coordination, decision trees)
- **Enterprise Automation** (Resume screening, data processing)
- **Customer Service Bots** (Restaurant assistant, therapy chatbot)
- **Content Generation** (Social media automation)
- **Complex Workflows** (Conditional logic, branching, error handling)

Each workflow is production-tested and includes error handling, logging, and scalability considerations.

---

## 🚀 Workflows

### 1. **AI Resume Screening** 📄
**Purpose**: Automated resume analysis and candidate evaluation

**Features:**
- Uploads and parses resumes (PDF/DOC)
- AI-powered resume screening using GPT-4
- Extracts key information (skills, experience, education)
- Scores candidates based on job requirements
- Generates evaluation reports
- Categorizes resumes (Match %, Fit Score, Red Flags)

**Use Case:**
- HR automation
- Recruitment process optimization
- Candidate shortlisting
- Skill matching analysis

**Integrations:**
- OpenAI GPT-4 (resume analysis)
- File storage (local/cloud)
- Database (candidate tracking)
- Email notifications

**Workflow Steps:**
1. Resume file ingestion
2. PDF/DOC parsing
3. Content extraction
4. AI analysis (skills, experience)
5. Scoring algorithm
6. Report generation
7. Database storage
8. Notification dispatch

---

### 2. **AI Agent Orchestrator** 🤖
**Purpose**: Multi-agent system coordination and task delegation

**Features:**
- Master agent for task routing
- Multiple specialized sub-agents
- Dynamic task delegation
- Agent communication and coordination
- Fallback mechanisms
- Performance monitoring
- State management across agents

**Use Case:**
- Complex business process automation
- Multi-step decision making
- Agent-based task distribution
- Workflow optimization

**Integrations:**
- OpenAI API (agent intelligence)
- Function calling
- State management (Redis/Database)
- Logging system

**Architecture:**
```
┌──────────────────────────┐
│   Main Orchestrator      │
└────────┬─────────────────┘
         │
    ┌────┴────┬─────────┬──────────┐
    ▼         ▼         ▼          ▼
┌────────┐ ┌────┐ ┌────────┐ ┌────────┐
│Agent 1 │ │Ag2 │ │Agent 3 │ │Agent 4 │
└────────┘ └────┘ └────────┘ └────────┘
```

---

### 3. **AI Therapy Assistant** 💬
**Purpose**: AI-powered mental health support chatbot

**Features:**
- Conversational AI with empathy
- Mental health guidance
- Symptom assessment
- Crisis detection & escalation
- Session logging
- Progress tracking
- Privacy-first design

**Use Case:**
- Mental health support
- User well-being
- Therapy session automation
- Wellness app integration

**Integrations:**
- OpenAI GPT-4 (conversational AI)
- Database (session history)
- Email (follow-up reminders)
- Twilio/SMS (escalation alerts)

**Safety Features:**
- Crisis keyword detection
- Automatic escalation to human counselor
- Session recordings with consent
- Data encryption

---

### 4. **AI-Powered Restaurant Assistant** 🍽️
**Purpose**: Complete restaurant automation system (N8N + OpenAI + ElevenLabs)

**Features:**
- Voice-based food ordering
- Menu management integration
- Order tracking
- Payment processing
- Table reservations
- Customer feedback collection
- Multi-language support

**Use Case:**
- QSR automation
- Customer ordering system
- Restaurant management
- Reservation handling

**Integrations:**
- OpenAI GPT-4 (order understanding)
- ElevenLabs (voice synthesis/understanding)
- Payment gateway (Stripe/PayPal)
- POS system integration
- Database (orders, inventory)
- SMS/Email notifications

**Voice Flow:**
```
User speaks → ElevenLabs STT → GPT-4 Processing → 
Order confirmation → ElevenLabs TTS → Response
```

---

### 5. **Content Creation Automation** 📱
**Purpose**: Social media content generation (Facebook + LinkedIn)

**Features:**
- AI-powered content generation
- Multi-platform posting
- Schedule management
- Content calendar
- Performance analytics
- Hashtag optimization
- Image/video attachment

**Use Case:**
- Social media management
- Marketing automation
- Content strategy execution
- Engagement optimization

**Integrations:**
- OpenAI GPT-4 (content writing)
- Facebook Graph API
- LinkedIn API
- Canva API (image generation)
- Analytics platforms
- Content calendar (Airtable/Google Sheets)

**Content Types:**
- Blog post snippets
- Product announcements
- Industry insights
- Promotional content
- User testimonials

---

### 6. **Invoice Management System** 💰
**Purpose**: Automated invoice processing and tracking

**Features:**
- Invoice generation
- Document parsing
- Payment tracking
- Due date reminders
- Tax calculations
- Financial reporting
- Multi-currency support

**Use Case:**
- Accounting automation
- Billing process
- Financial management
- Compliance tracking

**Integrations:**
- Google Docs/Sheets (templates)
- Email (distribution)
- Payment gateways
- Accounting software (QuickBooks)
- Database (transaction history)
- Scheduled tasks (reminders)

---

### 7. **Nutritionist AI Agent** 🥗
**Purpose**: Personalized nutrition guidance and meal planning

**Features:**
- Dietary preference assessment
- Nutritional analysis
- Meal plan generation
- Recipe suggestions
- Calorie tracking
- Macro calculations
- Health goal monitoring

**Use Case:**
- Health & wellness apps
- Nutrition coaching
- Personalized meal planning
- Dietary management

**Integrations:**
- OpenAI GPT-4 (nutritionist)
- Nutrition database API
- User profile management
- Progress tracking
- Email reminders
- Mobile app integration

**AI Capabilities:**
- Personalization based on preferences
- Dietary restriction handling
- Health goal optimization
- Nutrient balance analysis

---

### 8. **Voice Agent System** 🎙️
**Purpose**: Intelligent voice-based conversational AI

**Features:**
- Real-time voice processing
- Natural language understanding
- Contextual responses
- Multi-turn conversations
- Emotion detection
- Language support (multilingual)
- Low-latency processing

**Use Case:**
- Customer support calls
- Voice automation
- IVR systems
- Voice assistants

**Integrations:**
- ElevenLabs (voice I/O)
- OpenAI GPT-4 (understanding)
- Twilio (call handling)
- Call recording
- Analytics dashboard

**Voice Features:**
- Speaker identification
- Accent adaptation
- Emotion detection
- Background noise reduction

---

## 🛠️ Technologies

### Core Platform
- **N8N** (0.220+) - Workflow automation engine
- **Node-RED** - Low-code programming

### AI/LLM Services
- **OpenAI API** (GPT-4, GPT-3.5-turbo)
  - Content generation
  - Data analysis
  - Natural language understanding
  - Function calling

- **ElevenLabs**
  - Text-to-Speech (TTS)
  - Speech-to-Text (STT)
  - Voice cloning
  - Multilingual support

- **Anthropic Claude** (optional)
  - Long context processing
  - Complex reasoning

### Integrations
| Category | Services |
|----------|----------|
| **Communication** | Twilio, SendGrid, Gmail, Slack |
| **Payments** | Stripe, PayPal, Square |
| **Databases** | PostgreSQL, MongoDB, Airtable |
| **Cloud Storage** | AWS S3, Google Drive, Dropbox |
| **APIs** | REST, GraphQL, Webhooks |
| **Social Media** | Facebook, LinkedIn, Twitter, Instagram |
| **OCR/Document** | AWS Textract, Google Vision |
| **Analytics** | Google Analytics, Mixpanel |

### Infrastructure
- **Deployment**: Docker, Cloud platforms (AWS, GCP, Azure)
- **Monitoring**: Logging, error tracking
- **Security**: API key management, encryption, auth

---

## 🚀 Quick Start

### Prerequisites
- N8N instance (self-hosted or cloud)
- OpenAI API key
- ElevenLabs API key (for voice workflows)
- Node.js 16+ (for self-hosting)
- Docker (recommended)

### Installation

#### Option 1: N8N Cloud
1. Visit [n8n.cloud](https://n8n.cloud)
2. Create account
3. Import workflows from this repository
4. Configure API credentials

#### Option 2: Self-Hosted
```bash
# Docker installation
docker run -d --name n8n -p 5678:5678 \
  -e DB_TYPE=postgresql \
  -e DB_POSTGRESDB_HOST=postgres \
  n8nio/n8n

# Access at http://localhost:5678
```

#### Option 3: Docker Compose
```bash
git clone https://github.com/humayun-mhk/N8N-Workflows-Portfolio.git
cd N8N-Workflows-Portfolio
docker-compose up -d
```

### Configuration

1. **Set Environment Variables**
```bash
cat > .env << EOF
OPENAI_API_KEY=sk-...
ELEVENLABS_API_KEY=...
DATABASE_URL=postgresql://...
WEBHOOK_URL=https://your-domain.com
EOF
```

2. **Import Workflows**
- Download `.json` files from repository
- In N8N: Menu → Import
- Select workflow JSON
- Configure credentials
- Test and deploy

3. **Configure Credentials**
- OpenAI API key
- ElevenLabs API key
- Database connections
- Third-party API tokens

---

## 📖 Integration Guide

### OpenAI Integration

```json
{
  "node": "OpenAI",
  "credentials": {
    "apiKey": "sk-..."
  },
  "parameters": {
    "model": "gpt-4",
    "messages": [
      {
        "role": "user",
        "content": "{{ $json.input }}"
      }
    ],
    "temperature": 0.7,
    "maxTokens": 2000
  }
}
```

### ElevenLabs Integration

```json
{
  "node": "ElevenLabs",
  "credentials": {
    "apiKey": "..."
  },
  "parameters": {
    "action": "textToSpeech",
    "voiceId": "21m00Tcm4TlvDq8ikWAM",
    "text": "{{ $json.message }}",
    "modelId": "eleven_monolingual_v1"
  }
}
```

### Webhook Triggers

```javascript
// Receive data from external systems
{
  "trigger": "webhook",
  "method": "POST",
  "path": "/api/webhook/resume-upload",
  "response": {
    "status": "success",
    "processId": "{{ $json.id }}"
  }
}
```

### Database Operations

```json
{
  "node": "PostgreSQL",
  "operation": "executeQuery",
  "query": "INSERT INTO candidates (name, email, score) VALUES ({{ $json.name }}, {{ $json.email }}, {{ $json.score }})"
}
```

---

## 💼 Use Cases

### Human Resources
- **Resume Screening**: Automated candidate evaluation
- **Scheduling**: Interview coordination
- **Onboarding**: New hire automation

### Healthcare & Wellness
- **Therapy Support**: AI therapist chatbot
- **Nutrition Planning**: Personalized meal plans
- **Appointment Booking**: Patient scheduling

### Food & Beverage
- **Order Management**: Voice/text ordering
- **Reservation System**: Table booking automation
- **Inventory Tracking**: Stock management

### Marketing & Content
- **Social Media**: Multi-platform posting
- **Email Campaigns**: Automated marketing
- **Content Calendar**: Schedule management

### Finance & Accounting
- **Invoice Processing**: Automated billing
- **Payment Tracking**: Due date reminders
- **Financial Reporting**: Automated reports

---

## 🏗️ Architecture Patterns

### Event-Driven Architecture
```
Event Source → Trigger → Process → Action
     ↓           ↓         ↓        ↓
  Webhook    Condition  Transform  Notify
```

### Agent Pattern
```
User Input → Agent 1 → Decision Tree → Agent 2 → Output
                ↓            ↓            ↓
           Analyze      Route Task    Execute
```

### Pipeline Pattern
```
Input → Step 1 → Step 2 → Step 3 → Step 4 → Output
```

---

## 🔒 Security Best Practices

### API Key Management
```bash
# Use environment variables
export OPENAI_API_KEY=sk-...
export ELEVENLABS_API_KEY=...

# Never hardcode credentials
# Use N8N credential manager
```

### Data Privacy
- Encrypt sensitive data in transit (HTTPS)
- Use database encryption at rest
- Implement access controls
- Log audit trails
- GDPR compliance

### Error Handling
```json
{
  "node": "ErrorHandler",
  "catch": true,
  "parameters": {
    "onError": "log",
    "retryOn": ["networkError", "timeout"],
    "maxRetries": 3,
    "retryDelay": 1000
  }
}
```

---

## 📊 Workflow Metrics

### Performance Considerations
- **Latency**: < 2 seconds for most operations
- **Throughput**: Handle 1000+ requests/hour
- **Availability**: 99.9% uptime target
- **Scalability**: Horizontal scaling support

### Monitoring
- Workflow execution logs
- Error rate tracking
- Execution time analysis
- Cost monitoring (API calls)

---

## 🧪 Testing Workflows

### Local Testing
```bash
# Start N8N locally
docker run -p 5678:5678 n8nio/n8n

# Test webhook
curl -X POST http://localhost:5678/webhook/test \
  -H "Content-Type: application/json" \
  -d '{"test": "data"}'
```

### CI/CD Integration
```yaml
# GitHub Actions example
- name: Test N8N Workflows
  run: |
    npm install -g n8n
    n8n export --output workflows.json
    # Validate exported workflows
```

---

## 📈 Scalability & Deployment

### Self-Hosted Deployment
```bash
# Production setup with PostgreSQL
docker-compose -f docker-compose.prod.yml up -d
```

### Cloud Deployment
- **AWS**: EC2 + RDS
- **GCP**: Cloud Run + Cloud SQL
- **Azure**: App Service + Azure SQL
- **Heroku**: One-click deployment

### Load Balancing
- Distribute API calls across multiple instances
- Queue-based processing for high volume
- Caching layer for repeated requests

---

## 🔧 Customization

### Creating Custom Nodes
```javascript
// Custom N8N node template
import { INodeType, INodeTypeDescription } from 'n8n-workflow';

export class MyCustomNode implements INodeType {
  description: INodeTypeDescription = {
    displayName: 'My Custom Node',
    name: 'myCustomNode',
    group: ['transform'],
    version: 1,
    inputs: ['main'],
    outputs: ['main'],
    properties: [
      {
        displayName: 'Parameter',
        name: 'parameter',
        type: 'string',
        default: ''
      }
    ]
  };
}
```

### Extending Workflows
- Add new API integrations
- Create custom triggers
- Build specialized actions
- Chain multiple workflows

---

## 📚 Documentation

### Workflow Documentation
Each workflow includes:
- Purpose and use case
- Step-by-step breakdown
- Integration requirements
- Configuration examples
- Troubleshooting guide

### Exported Workflow Files
- `AI_resume_screening.json`
- `AI_Agent_Orchestrator.json`
- `AI_Therapy_Assistant.json`
- `AI_Restaurant_Assistant.json`
- `Content_Creation_Facebook_LinkedIn.json`
- `Invoice_Management.json`
- `Nutritionist_AI_Agent.json`
- `Voice_Agent.json`

---

## 🤝 Contributing

### Workflow Submission
1. Fork repository
2. Create feature branch: `git checkout -b feature/new-workflow`
3. Add workflow JSON file
4. Include documentation
5. Test thoroughly
6. Submit pull request

### Code Standards
- Descriptive node names
- Error handling in all branches
- Input/output validation
- Clear workflow comments
- Performance optimization

---

## 🐛 Troubleshooting

### Common Issues

**Issue**: API key authentication fails
```
Solution: Verify key in N8N credentials manager
         Check API quota and permissions
         Ensure key is not expired
```

**Issue**: Webhook not triggering
```
Solution: Verify webhook URL is public
         Check firewall/network settings
         Enable workflow
         Review error logs
```

**Issue**: Slow workflow execution
```
Solution: Add parallel processing
         Optimize API calls (batch when possible)
         Reduce data transformations
         Monitor API response times
```

**Issue**: Memory issues on self-hosted
```
Solution: Increase Docker memory allocation
         Enable database optimization
         Archive old execution logs
         Use pagination for large datasets
```

---

## 📞 Support & Resources

### Documentation
- [N8N Official Docs](https://docs.n8n.io/)
- [OpenAI API Reference](https://platform.openai.com/docs/)
- [ElevenLabs Documentation](https://elevenlabs.io/docs/)

### Community
- [N8N Community Forum](https://community.n8n.io/)
- [N8N Slack Community](https://n8n.io/community)
- [GitHub Issues](https://github.com/humayun-mhk/N8N-Workflows-Portfolio/issues)

### Contact
- Email: humayunkhann47@gmail.com
- GitHub: [@humayun-mhk](https://github.com/humayun-mhk)

---

## 📄 License

MIT License - See LICENSE file for details

---

## 🎯 Key Skills Demonstrated

✅ **Workflow Design**: Complex multi-step automation
✅ **AI Integration**: OpenAI, ElevenLabs, LLM orchestration
✅ **API Management**: Third-party service integration
✅ **Database Design**: Data modeling and querying
✅ **Error Handling**: Robust error management
✅ **Security**: API keys, data encryption, access control
✅ **Scalability**: High-volume processing
✅ **Problem Solving**: Creative automation solutions
✅ **Documentation**: Clear technical writing
✅ **Full-Stack Thinking**: End-to-end system design

---

## 🚀 Getting Started

1. **Choose a workflow** that interests you
2. **Download the JSON file** from repository
3. **Set up N8N** (cloud or self-hosted)
4. **Import the workflow**
5. **Configure credentials** (OpenAI, ElevenLabs, etc.)
6. **Test and deploy**
7. **Monitor execution**
8. **Customize** as needed

---

<div align="center">

**⭐ If you found these workflows helpful, please star the repository!**

**For collaboration or custom workflows, feel free to reach out!**

[Back to Top](#n8n-workflows-portfolio)

</div>
