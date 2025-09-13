---
layout: page
title: "Tools & Ecosystem"
permalink: /tools/
---

# Tools & Ecosystem

The AI-assisted development ecosystem is rapidly evolving. Here's a comprehensive guide to the essential tools and technologies that enable safe, productive vibe coding.

## 🛠️ IDE Plugins & Code Assistants

### Cursor
**The AI-first IDE experience**

#### Key Features:
- **Codebase-aware AI** - Understands your entire project context
- **Multi-file editing** - Make changes across multiple files simultaneously  
- **Natural language commands** - Describe what you want in plain English
- **Privacy controls** - Fine-grained control over what AI can access

#### Best Practices:
```bash
# .cursorignore - Protect sensitive files
.env
secrets/
*.key
*.pem
node_modules/
.git/
```

#### Configuration Example:
```json
{
  "cursor.ai.enabled": true,
  "cursor.ai.maxContextLength": 8000,
  "cursor.ai.excludePatterns": [
    "**/.env*",
    "**/secrets/**",
    "**/*.key"
  ],
  "cursor.ai.requireExplicitApproval": true
}
```

### GitHub Copilot
**The pioneer in AI pair programming**

#### Enterprise Features:
- **Organization-wide policies** - Control AI suggestions across teams
- **Audit logging** - Track AI usage and suggestions
- **Content filtering** - Block potentially problematic suggestions

#### Integration Example:
```yaml
# .github/copilot.yml
suggestions:
  enabled: true
  exclude_paths:
    - "secrets/"
    - "*.env"
    - "credentials/"
audit:
  log_suggestions: true
  log_acceptances: true
policies:
  require_review: true
  block_secrets: true
```

### Codeium
**Free alternative with enterprise features**

#### Advantages:
- **Free for individuals** - No cost barrier for experimentation
- **On-premise deployment** - Keep code and AI processing internal
- **Multi-language support** - Broad language ecosystem coverage

---

## 🤖 Agent Frameworks

### Model Context Protocol (MCP)
**Standardized tool orchestration for AI agents**

#### Core Concepts:
- **Servers** - Provide tools and resources to AI models
- **Clients** - Applications that use AI models with MCP servers
- **Tools** - Specific capabilities (file access, API calls, etc.)

#### Example MCP Server:
```python
from mcp import Server, Tool
from mcp.types import TextContent

class CodeReviewServer(Server):
    def __init__(self):
        super().__init__("code-review-server")
        
    @Tool("security_scan")
    async def security_scan(self, file_path: str) -> TextContent:
        """Run security analysis on a file"""
        results = await self.run_security_scanner(file_path)
        return TextContent(
            type="text",
            text=f"Security scan results: {results}"
        )
```

### Amazon Bedrock Agents
**Enterprise-grade AI agent platform**

#### Key Features:
- **Managed infrastructure** - AWS handles scaling and reliability
- **Knowledge bases** - RAG with your documentation
- **Action groups** - Define what agents can do
- **Guardrails** - Built-in safety controls

#### Configuration:
```yaml
bedrock_agent:
  name: "development-assistant"
  foundation_model: "anthropic.claude-3-sonnet"
  knowledge_bases:
    - company_docs
    - api_documentation
  action_groups:
    - code_review
    - security_scan
    - deployment
  guardrails:
    - no_secrets_exposure
    - require_human_approval
```

### LangChain
**Flexible framework for AI application development**

#### Use Cases:
- **Custom agent workflows** - Build specialized development assistants
- **Multi-step reasoning** - Complex problem-solving chains
- **Tool integration** - Connect AI to existing development tools

---

## 🔒 Governance & Security Tools

### Static Analysis Security Testing (SAST)

#### Trivy
**Comprehensive vulnerability scanner**

```bash
# Scan for vulnerabilities
trivy fs --security-checks vuln,config,secret .

# Generate SBOM
trivy fs --format spdx-json --output sbom.json .

# CI/CD Integration
trivy fs --exit-code 1 --severity HIGH,CRITICAL .
```

#### Grype
**Vulnerability scanner for container images and filesystems**

```yaml
# .grype.yml configuration
db:
  auto-update: true
check-for-app-update: false
fail-on-severity: "high"
output: "table"
scope: "all-layers"
```

#### Snyk
**Developer-first security platform**

```bash
# Test for vulnerabilities
snyk test

# Monitor for new vulnerabilities
snyk monitor

# Test container images
snyk container test myapp:latest
```

### Policy Enforcement

#### Open Policy Agent (OPA)
**Policy-as-code engine**

```rego
package kubernetes.admission

deny[msg] {
    input.request.kind.kind == "Pod"
    input.request.object.metadata.labels["ai-generated"] == "true"
    not input.request.object.metadata.annotations["human-reviewed"]
    msg := "AI-generated pods require human review annotation"
}
```

#### Conftest
**Test your configuration files using OPA**

```bash
# Test Kubernetes manifests
conftest verify --policy policy/ k8s-manifests/

# Test Dockerfile
conftest verify --policy policy/ Dockerfile
```

---

## 📊 Audit & Logging

### ELK Stack (Elasticsearch, Logstash, Kibana)
**Comprehensive logging and analytics**

#### Logstash Configuration:
```ruby
input {
  beats {
    port => 5044
  }
}

filter {
  if [fields][log_type] == "ai_code_generation" {
    json {
      source => "message"
    }
    mutate {
      add_tag => ["ai_generated"]
    }
  }
}

output {
  elasticsearch {
    hosts => ["localhost:9200"]
    index => "ai-development-logs-%{+YYYY.MM.dd}"
  }
}
```

### Loki
**Lightweight log aggregation**

```yaml
# promtail configuration
server:
  http_listen_port: 9080
  grpc_listen_port: 0

positions:
  filename: /tmp/positions.yaml

clients:
  - url: http://loki:3100/loki/api/v1/push

scrape_configs:
  - job_name: ai-development
    static_configs:
      - targets:
          - localhost
        labels:
          job: ai-dev-logs
          __path__: /var/log/ai-development/*.log
```

### Supabase Activity Logs
**Real-time activity tracking**

```sql
-- Create audit table
CREATE TABLE ai_code_audit (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    timestamp TIMESTAMPTZ DEFAULT NOW(),
    developer_id TEXT NOT NULL,
    ai_model TEXT NOT NULL,
    prompt_hash TEXT NOT NULL,
    code_hash TEXT NOT NULL,
    review_status TEXT NOT NULL,
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Enable RLS
ALTER TABLE ai_code_audit ENABLE ROW LEVEL SECURITY;

-- Create policy for developers to see their own logs
CREATE POLICY "Developers can view own audit logs" ON ai_code_audit
    FOR SELECT USING (auth.uid()::text = developer_id);
```

---

## 🔐 Secure Context Managers

### AWS Secrets Manager
**Centralized secrets management**

```python
import boto3
from botocore.exceptions import ClientError

class SecureContextManager:
    def __init__(self):
        self.secrets_client = boto3.client('secretsmanager')
    
    def get_secret(self, secret_name):
        try:
            response = self.secrets_client.get_secret_value(
                SecretId=secret_name
            )
            return response['SecretString']
        except ClientError as e:
            # Log error without exposing secret details
            logger.error(f"Failed to retrieve secret: {secret_name}")
            raise
    
    def mask_for_ai(self, data):
        """Mask sensitive data before sending to AI"""
        masked_data = data.copy()
        for key in ['password', 'token', 'key', 'secret']:
            if key in masked_data:
                masked_data[key] = '***MASKED***'
        return masked_data
```

### HashiCorp Vault
**Enterprise secrets management**

```python
import hvac

class VaultContextManager:
    def __init__(self, vault_url, vault_token):
        self.client = hvac.Client(url=vault_url, token=vault_token)
    
    def get_database_credentials(self, role_name):
        """Get dynamic database credentials"""
        response = self.client.secrets.database.generate_credentials(
            name=role_name
        )
        return {
            'username': response['data']['username'],
            'password': response['data']['password'],
            'lease_duration': response['lease_duration']
        }
    
    def create_ai_safe_context(self, original_config):
        """Create AI-safe version of configuration"""
        safe_config = original_config.copy()
        
        # Replace actual secrets with references
        safe_config['database_url'] = '${vault:database/config#url}'
        safe_config['api_key'] = '${vault:api/keys#primary}'
        
        return safe_config
```

---

## 🔧 Integration Examples

### CI/CD Pipeline with Security Gates

```yaml
# .github/workflows/ai-code-security.yml
name: AI Code Security Pipeline

on:
  pull_request:
    branches: [main]

jobs:
  security-scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Check for AI-generated code
        run: |
          if git log --oneline -1 | grep -q "ai-generated"; then
            echo "AI_GENERATED=true" >> $GITHUB_ENV
          fi
      
      - name: Run Trivy scan
        if: env.AI_GENERATED == 'true'
        run: |
          trivy fs --exit-code 1 --severity HIGH,CRITICAL .
      
      - name: Run SAST with Semgrep
        if: env.AI_GENERATED == 'true'
        run: |
          semgrep --config=auto --error .
      
      - name: Check policy compliance
        if: env.AI_GENERATED == 'true'
        run: |
          conftest verify --policy .policies/ .
      
      - name: Require manual review
        if: env.AI_GENERATED == 'true'
        run: |
          echo "::warning::This PR contains AI-generated code and requires manual security review"
```

### Monitoring Dashboard

```python
# Grafana dashboard configuration
dashboard = {
    "dashboard": {
        "title": "AI Development Metrics",
        "panels": [
            {
                "title": "AI Code Generation Rate",
                "type": "graph",
                "targets": [
                    {
                        "expr": "rate(ai_code_generations_total[5m])",
                        "legendFormat": "Generations per second"
                    }
                ]
            },
            {
                "title": "Security Issues Detected",
                "type": "singlestat",
                "targets": [
                    {
                        "expr": "sum(security_issues_total{source='ai_generated'})",
                        "legendFormat": "Total Issues"
                    }
                ]
            },
            {
                "title": "Review Approval Rate",
                "type": "piechart",
                "targets": [
                    {
                        "expr": "ai_code_reviews_by_status",
                        "legendFormat": "{{status}}"
                    }
                ]
            }
        ]
    }
}
```

---

## 🚀 Getting Started Recommendations

### For Small Teams (< 10 developers)
1. **Start with**: Cursor or GitHub Copilot
2. **Add security**: Trivy + GitHub Actions
3. **Implement logging**: Simple file-based audit logs
4. **Governance**: Manual review process

### For Medium Teams (10-50 developers)
1. **IDE**: Cursor + GitHub Copilot Enterprise
2. **Security**: Trivy + Snyk + OPA policies
3. **Logging**: ELK stack or Loki
4. **Governance**: Automated policy enforcement

### For Large Enterprises (50+ developers)
1. **Platform**: Bedrock Agents + MCP
2. **Security**: Full SAST/DAST pipeline + Vault
3. **Logging**: Enterprise SIEM integration
4. **Governance**: Comprehensive review boards + compliance automation

---

Ready to implement these tools systematically? Continue to [Adoption Framework](adoption.html).

---

[← Previous: Governance](governance.html) | [Next: Adoption Framework →](adoption.html)
