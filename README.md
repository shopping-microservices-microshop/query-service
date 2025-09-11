# AI-Powered Query Service 🤖

An intelligent microservice that uses **AWS Bedrock** and **Claude 3.5 Sonnet** to provide natural language responses about product data. This service acts as a smart intermediary between users and product information, enabling conversational e-commerce experiences.

## 🚀 What Makes This Service Special

This isn't just another REST API - it's an **AI-powered query engine** that:

- **Understands Natural Language**: Ask questions like "What's a good budget laptop for coding?" instead of complex database queries
- **Provides Contextual Answers**: Leverages AWS Bedrock's Claude 3.5 Sonnet model for intelligent, human-like responses
- **Real-time Product Integration**: Dynamically fetches current product data and provides up-to-date recommendations
- **Cloud-Native AI**: Built for production with enterprise-grade AWS AI services

## 🧠 AI Architecture

```
┌─────────────┐    ┌──────────────┐    ┌─────────────────┐
│   User      │───▶│Query Service │───▶│ Product Service │
│   Query     │    │              │    │                 │
└─────────────┘    └──────┬───────┘    └─────────────────┘
                          │
                          ▼
                   ┌─────────────────┐
                   │   AWS Bedrock   │
                   │ Claude 3.5 Sonnet│
                   └─────────────────┘
```

### How the AI Integration Works

1. **Query Reception**: User sends natural language query via REST API
2. **Data Fetching**: Service retrieves current product catalog from product-service
3. **Prompt Engineering**: Creates a structured prompt combining user query with product data
4. **AI Processing**: Sends prompt to AWS Bedrock's Claude 3.5 Sonnet model
5. **Intelligent Response**: AI analyzes data and provides contextual, helpful answers

## 🛠️ Technology Stack

| Component | Technology | Purpose |
|-----------|------------|---------|
| **Runtime** | Node.js 20 | High-performance JavaScript runtime |
| **Web Framework** | Express.js | REST API endpoints |
| **AI Service** | AWS Bedrock | Managed AI/ML platform |
| **AI Model** | Claude 3.5 Sonnet | Advanced language model |
| **HTTP Client** | Axios | Inter-service communication |
| **Containerization** | Docker | Consistent deployment |
| **Orchestration** | Kubernetes | Production scaling |

## 🚀 Quick Start

### Prerequisites

- Node.js 20+ installed
- AWS account with Bedrock access
- AWS credentials configured

### Local Development

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd query-service
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Configure AWS credentials**
   ```bash
   export AWS_REGION="us-east-1"
   export AWS_ACCESS_KEY_ID="your-access-key"
   export AWS_SECRET_ACCESS_KEY="your-secret-key"
   ```

4. **Start the service**
   ```bash
   npm start
   ```

5. **Test the AI functionality**
   ```bash
   curl "http://localhost:8002/query?q=what+is+a+good+budget+laptop+for+coding"
   ```

### Docker Deployment

```bash
# Build the image
docker build -t query-service .

# Run with environment variables
docker run -p 8002:8002 \
  -e AWS_REGION=us-east-1 \
  -e AWS_ACCESS_KEY_ID=your-key \
  -e AWS_SECRET_ACCESS_KEY=your-secret \
  query-service
```

## 🔧 API Documentation

### Query Endpoint

**GET** `/query`

Processes natural language queries about products using AI.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `q` | string | ✅ | Natural language query about products |

#### Example Requests

```bash
# Find budget-friendly laptops
curl "http://localhost:8002/query?q=show+me+budget+laptops+under+800+dollars"

# Get product recommendations
curl "http://localhost:8002/query?q=what+smartphone+has+the+best+camera"

# Compare products
curl "http://localhost:8002/query?q=compare+gaming+laptops+with+good+graphics"
```

#### Response Format

```json
{
  "query": "what is a good budget laptop for coding",
  "answer": "Based on the available products, I'd recommend the Dell Inspiron 15 3000 for budget-conscious coding. At $649.99, it offers good value with an Intel Core i5 processor and 8GB RAM, which should handle most programming tasks well. It strikes a good balance between performance and affordability for development work."
}
```

## 🤖 AI Capabilities

### Supported Query Types

- **Product Recommendations**: "What's the best laptop for gaming?"
- **Price Comparisons**: "Show me phones under $500"
- **Feature Analysis**: "Which laptop has the longest battery life?"
- **Use Case Matching**: "Good phone for photography"
- **Specification Queries**: "Laptops with 16GB RAM"

### AI Model Features

- **Context Awareness**: Understands product relationships and user intent
- **Accurate Information**: Only uses provided product data, no hallucination
- **Natural Responses**: Conversational, helpful tone
- **Comparative Analysis**: Can compare multiple products intelligently

## 🏗️ Production Deployment

### Kubernetes Configuration

The service includes production-ready Kubernetes manifests:

```yaml
# Deployment with resource limits and health checks
apiVersion: apps/v1
kind: Deployment
metadata:
  name: query-service-deployment
spec:
  replicas: 1
  template:
    spec:
      containers:
      - name: query-service
        image: willchrist/query-service:latest
        resources:
          requests:
            cpu: "200m"
            memory: "256Mi"
          limits:
            cpu: "1000m"
            memory: "512Mi"
        startupProbe:
          httpGet:
            path: /query?q=startup
            port: 8002
```

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `AWS_REGION` | AWS region for Bedrock | `us-east-1` |
| `AWS_ACCESS_KEY_ID` | AWS access credentials | Required |
| `AWS_SECRET_ACCESS_KEY` | AWS secret credentials | Required |
| `PRODUCT_SERVICE_HOST` | Product service hostname | `product-service` |
| `PORT_PRODUCT` | Product service port | `8001` |

## 🔍 Development Features

### Code Quality

- **Modern JavaScript**: ES6+ features with async/await
- **Error Handling**: Comprehensive error catching and user-friendly messages
- **Logging**: Detailed console logging for debugging
- **Type Safety**: JSDoc comments for better code documentation

### Security Best Practices

- **Non-root User**: Docker container runs as non-privileged user
- **Secrets Management**: Environment variables for sensitive data
- **Input Validation**: Query parameter validation
- **Error Sanitization**: No sensitive information in error responses

### Testing & CI/CD

The repository includes comprehensive testing and security workflows:

- **Linting**: ESLint for code quality
- **SAST**: CodeQL static analysis
- **SCA**: Trivy security scanning
- **DAST**: OWASP ZAP dynamic testing
- **Unit Tests**: Jest testing framework

## 🔮 AI Prompt Engineering

The service uses sophisticated prompt engineering to ensure accurate, helpful responses:

```javascript
const createPrompt = (userQuery, products) => {
    return `Human: You are a helpful and friendly e-commerce assistant. 
    Your task is to answer the user's query based ONLY on the provided 
    list of products. Do not invent products or details.
    
    [Product data and query insertion]
    
    Please provide a clear and concise answer.`;
};
```

### Prompt Design Principles

- **Role Definition**: Clear AI assistant persona
- **Data Constraints**: Only use provided product information
- **Response Format**: Structured, helpful answers
- **Safety Measures**: Prevent hallucination and misinformation

## 📊 Performance & Scaling

### Resource Usage

- **Memory**: ~256MB baseline, 512MB limit
- **CPU**: 200m request, 1000m limit
- **Startup Time**: ~10-15 seconds with health checks
- **Response Time**: ~2-5 seconds for AI queries

### Scaling Considerations

- **Horizontal Scaling**: Stateless design allows multiple replicas
- **AWS Bedrock Limits**: Consider API rate limits and costs
- **Product Service Dependency**: Ensure product service availability
- **Caching Strategy**: Consider implementing response caching for common queries

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-ai-feature`)
3. Commit changes (`git commit -m 'Add amazing AI feature'`)
4. Push to branch (`git push origin feature/amazing-ai-feature`)
5. Open a Pull Request



## 🆘 Support

For issues related to:
- **AI responses**: Check AWS Bedrock service status and credentials
- **Service connectivity**: Verify product-service is running
- **Performance**: Monitor resource usage and scaling

## 🔗 Related Services

- [Product Service](https://github.com/shopping-microservices-microshop/product-service) - Provides product data
- [Frontend Service](https://github.com/shopping-microservices-microshop/frontend-service) - User interface
- [Cart Service](https://github.com/shopping-microservices-microshop/cart-service) - Shopping cart functionality
- [Deploy Centre](https://github.com/shopping-microservices-microshop/deploy-centre) - Infrastructure automation

---

**Built with ❤️ and 🤖 AI** - Making e-commerce more intelligent, one query at a time.
