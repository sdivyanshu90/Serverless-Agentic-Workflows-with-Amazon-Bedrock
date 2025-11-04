# Serverless Agentic Workflows with Amazon Bedrock

A comprehensive solution for building and deploying serverless agentic workflows using Amazon Bedrock, enabling intelligent, autonomous AI agents that can reason, plan, and execute complex tasks in a scalable, cost-effective manner.

## Overview

This repository demonstrates how to build serverless agentic workflows leveraging Amazon Bedrock's foundation models. Agentic workflows enable AI systems to autonomously break down complex tasks, make decisions, and execute actions through iterative reasoning and tool use.

### What are Agentic Workflows?

Agentic workflows are AI systems that can:
- **Reason and Plan**: Break down complex tasks into manageable steps
- **Use Tools**: Interact with external APIs, databases, and services
- **Iterate**: Refine outputs based on feedback and validation
- **Make Decisions**: Choose appropriate actions based on context
- **Learn**: Adapt behavior based on outcomes

## Features

- **Serverless Architecture**: Built on AWS Lambda for automatic scaling and pay-per-use pricing
- **Amazon Bedrock Integration**: Leverage state-of-the-art foundation models (Claude, Llama, etc.)
- **Multi-Agent Orchestration**: Coordinate multiple specialized agents for complex workflows
- **Tool Integration**: Extensible framework for adding custom tools and APIs
- **Event-Driven Processing**: Asynchronous task execution with AWS services
- **Cost Optimization**: Efficient resource utilization with serverless design
- **Monitoring & Logging**: Built-in observability with CloudWatch

## Architecture

```
┌─────────────┐
│   User/API  │
└──────┬──────┘
       │
       ▼
┌─────────────────┐
│  API Gateway    │
└──────┬──────────┘
       │
       ▼
┌─────────────────┐      ┌──────────────────┐
│ Lambda Function │◄────►│ Amazon Bedrock   │
│  (Agent Core)   │      │ Foundation Models│
└──────┬──────────┘      └──────────────────┘
       │
       ├─────►┌──────────────┐
       │      │  DynamoDB    │
       │      │  (State)     │
       │      └──────────────┘
       │
       ├─────►┌──────────────┐
       │      │   S3         │
       │      │  (Storage)   │
       │      └──────────────┘
       │
       └─────►┌──────────────┐
              │  EventBridge │
              │  (Events)    │
              └──────────────┘
```

## Prerequisites

Before you begin, ensure you have the following:

- **AWS Account**: With appropriate permissions for Lambda, Bedrock, API Gateway, DynamoDB, and S3
- **AWS CLI**: Installed and configured ([Installation Guide](https://aws.amazon.com/cli/))
- **Node.js**: Version 18.x or later ([Download](https://nodejs.org/))
- **Python**: Version 3.9 or later (if using Python-based agents)
- **SAM CLI** or **Serverless Framework**: For deployment
- **Amazon Bedrock Access**: Model access enabled in your AWS account

### AWS Bedrock Model Access

1. Navigate to Amazon Bedrock in AWS Console
2. Request model access for desired foundation models:
   - Anthropic Claude (recommended)
   - Meta Llama 2/3
   - Amazon Titan
   - AI21 Jurassic-2

## Installation

### Clone the Repository

```bash
git clone https://github.com/sdivyanshu90/Serverless-Agentic-Workflows-with-Amazon-Bedrock.git
cd Serverless-Agentic-Workflows-with-Amazon-Bedrock
```

### Install Dependencies

```bash
# For Node.js projects
npm install

# For Python projects
pip install -r requirements.txt
```

### Configure AWS Credentials

```bash
aws configure
```

Enter your AWS Access Key ID, Secret Access Key, and default region.

## Configuration

Create a configuration file `config.yaml` or `.env`:

```yaml
# AWS Configuration
AWS_REGION: us-east-1
BEDROCK_MODEL_ID: anthropic.claude-3-sonnet-20240229-v1:0

# Agent Configuration
MAX_ITERATIONS: 10
TIMEOUT_SECONDS: 300

# Resource Names
DYNAMODB_TABLE: agentic-workflows-state
S3_BUCKET: agentic-workflows-storage
API_GATEWAY_NAME: agentic-workflows-api
```

## Deployment

### Using AWS SAM

```bash
# Build the application
sam build

# Deploy to AWS
sam deploy --guided
```

### Using Serverless Framework

```bash
# Deploy to AWS
serverless deploy

# Deploy to specific stage
serverless deploy --stage prod
```

### Manual Deployment

```bash
# Package Lambda function
zip -r function.zip .

# Create Lambda function
aws lambda create-function \
  --function-name agentic-workflow \
  --runtime nodejs18.x \
  --handler index.handler \
  --zip-file fileb://function.zip \
  --role arn:aws:iam::YOUR_ACCOUNT:role/lambda-execution-role
```

## Usage

### Basic Example

```javascript
const { AgenticWorkflow } = require('./src/workflow');

// Initialize workflow
const workflow = new AgenticWorkflow({
  modelId: 'anthropic.claude-3-sonnet-20240229-v1:0',
  region: 'us-east-1'
});

// Execute task
const result = await workflow.execute({
  task: "Analyze customer feedback and generate a summary report",
  tools: ['database', 'sentiment-analysis', 'report-generator']
});

console.log(result);
```

### API Endpoint Example

```bash
# Invoke workflow via API
curl -X POST https://your-api-id.execute-api.us-east-1.amazonaws.com/prod/workflow \
  -H "Content-Type: application/json" \
  -d '{
    "task": "Research and summarize recent AI developments",
    "parameters": {
      "depth": "comprehensive",
      "sources": ["arxiv", "github", "news"]
    }
  }'
```

### Python Example

```python
from agentic_workflow import AgenticWorkflow

# Initialize workflow
workflow = AgenticWorkflow(
    model_id='anthropic.claude-3-sonnet-20240229-v1:0',
    region='us-east-1'
)

# Execute task
result = workflow.execute(
    task="Analyze sales data and provide recommendations",
    tools=['data-analysis', 'visualization', 'reporting']
)

print(result)
```

## Use Cases

### 1. **Customer Support Automation**
Autonomous agents that handle customer inquiries, access knowledge bases, and escalate complex issues.

### 2. **Data Analysis Pipelines**
Multi-step data processing workflows with automated insights generation and reporting.

### 3. **Content Generation**
Intelligent content creation with research, drafting, editing, and optimization stages.

### 4. **Research Assistant**
Automated literature review, summarization, and synthesis of information from multiple sources.

### 5. **Business Process Automation**
End-to-end automation of complex business processes with decision-making capabilities.

## Project Structure

```
.
├── src/
│   ├── agents/          # Agent implementations
│   ├── tools/           # Tool definitions and integrations
│   ├── workflows/       # Workflow orchestration logic
│   └── utils/           # Utility functions
├── tests/               # Unit and integration tests
├── infrastructure/      # IaC templates (CloudFormation/Terraform)
├── docs/                # Additional documentation
├── examples/            # Example use cases
├── template.yaml        # SAM template
├── serverless.yml       # Serverless Framework config
├── package.json         # Node.js dependencies
├── requirements.txt     # Python dependencies
└── README.md           # This file
```

## Key Components

### Agent Core
The main orchestration engine that:
- Manages conversation state
- Invokes Bedrock models
- Handles tool selection and execution
- Implements reasoning loops

### Tool System
Extensible framework for:
- API integrations
- Database queries
- File operations
- External service calls

### State Management
DynamoDB-based persistence for:
- Conversation history
- Workflow state
- Execution results
- Error tracking

## Best Practices

1. **Token Management**: Monitor and optimize token usage to control costs
2. **Error Handling**: Implement robust retry logic and graceful degradation
3. **Security**: Use IAM roles, encrypt sensitive data, validate inputs
4. **Monitoring**: Set up CloudWatch alarms for errors and performance metrics
5. **Testing**: Write comprehensive tests for agents, tools, and workflows
6. **Cost Control**: Set up billing alerts and optimize resource usage

## Monitoring and Debugging

### CloudWatch Logs

```bash
# View Lambda logs
aws logs tail /aws/lambda/agentic-workflow --follow

# Filter for errors
aws logs filter-log-events \
  --log-group-name /aws/lambda/agentic-workflow \
  --filter-pattern "ERROR"
```

### Metrics

- **Invocation Count**: Number of workflow executions
- **Duration**: Execution time per workflow
- **Error Rate**: Failed executions
- **Token Usage**: Bedrock API token consumption
- **Cost**: Total AWS service costs

## Troubleshooting

### Common Issues

**Issue**: Bedrock model access denied
```
Solution: Enable model access in AWS Bedrock console
```

**Issue**: Lambda timeout
```
Solution: Increase timeout in configuration or optimize workflow
```

**Issue**: Out of memory errors
```
Solution: Increase Lambda memory allocation
```

## Cost Considerations

- **Lambda**: $0.20 per 1M requests + compute time
- **Bedrock**: Varies by model (input/output tokens)
- **DynamoDB**: Pay per request or provisioned capacity
- **S3**: Storage and data transfer costs
- **API Gateway**: $3.50 per million API calls

**Estimated Cost Example**: A workflow processing 10,000 requests/month with Claude model:
- Lambda: ~$5
- Bedrock: ~$50-100 (depends on token usage)
- DynamoDB: ~$1
- S3: ~$1
- **Total**: ~$60-110/month

## Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Setup

```bash
# Install dependencies
npm install

# Run tests
npm test

# Run linter
npm run lint

# Local development
npm run dev
```

## Testing

```bash
# Run all tests
npm test

# Run unit tests
npm run test:unit

# Run integration tests
npm run test:integration

# Generate coverage report
npm run test:coverage
```

## Security

- Never commit AWS credentials or API keys
- Use AWS Secrets Manager for sensitive configuration
- Implement proper IAM least privilege policies
- Enable CloudTrail for audit logging
- Regularly update dependencies
- Use VPC for Lambda if accessing private resources

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- Amazon Web Services for Bedrock and serverless infrastructure
- Anthropic for Claude models
- The open-source community for tools and frameworks

## Resources

- [Amazon Bedrock Documentation](https://docs.aws.amazon.com/bedrock/)
- [AWS Lambda Documentation](https://docs.aws.amazon.com/lambda/)
- [Agentic AI Patterns](https://www.anthropic.com/research)
- [Serverless Framework](https://www.serverless.com/)
- [AWS SAM Documentation](https://docs.aws.amazon.com/serverless-application-model/)

## Support

For issues and questions:
- Open an issue in this repository
- Contact: [sdivyanshu90](https://github.com/sdivyanshu90)

## Roadmap

- [ ] Multi-agent collaboration patterns
- [ ] Enhanced tool library
- [ ] Web UI for workflow management
- [ ] Additional model support
- [ ] Performance optimization
- [ ] Advanced monitoring dashboard
- [ ] Cost optimization features

---

**Built with ❤️ using AWS Bedrock and Serverless Technologies**