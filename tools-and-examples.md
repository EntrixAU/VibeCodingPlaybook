---
layout: page
title: "Tools & Practical Examples"
permalink: /tools-and-examples/
---

# Tools & Practical Examples

A comprehensive guide showing how to implement best practices using specific AI development tools. This page provides hands-on examples for GitHub Copilot, Cursor, and Claude, demonstrating practical implementation of security, governance, and quality measures.

## 🛠️ Tool Overview

### GitHub Copilot
- **Best for**: Code completion, inline suggestions, chat-based assistance
- **Strengths**: IDE integration, contextual completions, enterprise features
- **Use cases**: Daily development, code review assistance, documentation

### Cursor
- **Best for**: Full codebase understanding, multi-file editing, AI-first development
- **Strengths**: Codebase-wide context, natural language commands, advanced AI features
- **Use cases**: Complex refactoring, architecture changes, large-scale development

### Claude (via API/Interface)
- **Best for**: Complex reasoning, security analysis, architectural decisions
- **Strengths**: Long-context understanding, detailed explanations, safety-focused
- **Use cases**: Code review, security analysis, architectural planning

---

## 🎯 Implementing Golden Rules

### Rule #1: AI Generates → Human Reviews → CI/CD Validates

#### GitHub Copilot Implementation

**Step 1: Configure Copilot for Review-First Workflow**
```json
// .vscode/settings.json
{
  "github.copilot.enable": {
    "*": true,
    "yaml": false,
    "plaintext": false
  },
  "github.copilot.advanced": {
    "secret_scanning": true,
    "length": 500
  },
  "editor.inlineSuggest.enabled": true,
  "github.copilot.editor.enableAutoCompletions": true
}
```

**Step 2: Use Copilot Chat for Planning**
```markdown
# In Copilot Chat
@workspace I need to implement user authentication. Can you:
1. Analyze the current codebase structure
2. Suggest the best approach for JWT authentication
3. Identify potential security considerations
4. Provide a step-by-step implementation plan

Please don't generate code yet, just the plan.
```

**Step 3: Generate Code with Review Markers**
```python
# Copilot-generated code with review markers
def authenticate_user(username: str, password: str) -> AuthResult:
    """
    COPILOT-GENERATED: User authentication with JWT
    REVIEW REQUIRED: Security validation needed
    """
    # TODO: Human review - validate input sanitization
    if not username or not password:
        return AuthResult(success=False, error="Invalid credentials")
    
    # TODO: Human review - confirm bcrypt usage is correct
    user = get_user_by_username(username)
    if user and bcrypt.checkpw(password.encode(), user.password_hash):
        # TODO: Human review - JWT token generation security
        token = generate_jwt_token(user.id)
        return AuthResult(success=True, token=token)
    
    return AuthResult(success=False, error="Invalid credentials")
```

#### Cursor Implementation

**Step 1: Configure Cursor for Safe AI Usage**
```json
// .cursor-settings.json
{
  "cursor.ai.enabled": true,
  "cursor.ai.maxContextLength": 8000,
  "cursor.ai.excludePatterns": [
    "**/.env*",
    "**/secrets/**",
    "**/*.key"
  ],
  "cursor.ai.requireExplicitApproval": true,
  "cursor.ai.logInteractions": true
}
```

**Step 2: Use Cursor's Planning Mode**
```
# Cursor Command (Cmd+K)
Plan the implementation of secure user authentication:
- Analyze current auth patterns in the codebase
- Identify security requirements from existing code
- Create implementation steps with security checkpoints
- Don't implement yet, just create the plan
```

**Step 3: Implement with Review Gates**
```python
# Generated with Cursor with built-in review points
class AuthenticationService:
    """
    AI-GENERATED with Cursor
    Security Review Status: PENDING
    Human Approval Required: YES
    """
    
    def __init__(self, secret_key: str):
        # CURSOR-REVIEW: Validate secret key handling
        self.secret_key = secret_key
        self.rate_limiter = RateLimiter()
    
    def authenticate(self, username: str, password: str) -> AuthResult:
        # CURSOR-REVIEW: Input validation implementation
        if not self._validate_input(username, password):
            return AuthResult(success=False, error="Invalid input")
        
        # CURSOR-REVIEW: Rate limiting logic
        if self.rate_limiter.is_limited(username):
            return AuthResult(success=False, error="Rate limited")
        
        # Implementation continues...
```

#### Claude Implementation

**Step 1: Security-First Prompt Engineering**
```markdown
# Claude Prompt
I need to implement user authentication. Before generating any code:

1. Analyze these security requirements:
   - OWASP authentication guidelines
   - JWT best practices
   - Rate limiting requirements
   - Input validation standards

2. Create a security checklist for human review

3. Then provide code with explicit security annotations

Context: Python Flask application, existing bcrypt usage, JWT tokens required.
```

**Step 2: Claude's Security-Annotated Output**
```python
"""
CLAUDE-GENERATED AUTHENTICATION MODULE
Security Analysis Completed: ✅
Human Review Required: ✅

SECURITY CHECKLIST FOR REVIEWER:
□ Input validation prevents injection attacks
□ Password hashing uses secure bcrypt settings
□ JWT tokens have appropriate expiration
□ Rate limiting prevents brute force
□ Error messages don't leak information
□ Logging doesn't expose sensitive data
"""

import bcrypt
import jwt
from datetime import datetime, timedelta
from typing import Optional

class SecureAuthenticator:
    def __init__(self, secret_key: str, bcrypt_rounds: int = 12):
        # SECURITY: Store secret key securely, validate strength
        if len(secret_key) < 32:
            raise ValueError("Secret key must be at least 32 characters")
        self.secret_key = secret_key
        self.bcrypt_rounds = bcrypt_rounds
        self.rate_limiter = RateLimiter(max_attempts=5, window_minutes=15)
    
    def authenticate_user(self, username: str, password: str) -> AuthResult:
        """
        SECURITY ANNOTATIONS:
        - Input validation: Username/password sanitization
        - Rate limiting: Prevents brute force attacks
        - Timing attacks: Constant-time comparison where possible
        - Information disclosure: Generic error messages
        """
        
        # SECURITY: Input validation and sanitization
        username = self._sanitize_username(username)
        if not self._validate_credentials_format(username, password):
            self._log_auth_attempt(username, "invalid_format", False)
            return AuthResult(success=False, error="Invalid credentials")
        
        # SECURITY: Rate limiting check
        if self.rate_limiter.is_rate_limited(username):
            self._log_auth_attempt(username, "rate_limited", False)
            return AuthResult(success=False, error="Too many attempts")
        
        # SECURITY: User lookup with timing attack mitigation
        user = self._get_user_constant_time(username)
        
        # SECURITY: Password verification with constant-time comparison
        password_valid = False
        if user and user.password_hash:
            password_valid = bcrypt.checkpw(
                password.encode('utf-8'), 
                user.password_hash.encode('utf-8')
            )
        
        if password_valid:
            # SECURITY: Generate JWT with secure settings
            token = self._generate_secure_jwt(user.id, user.username)
            self._log_auth_attempt(username, "success", True)
            return AuthResult(success=True, token=token, user=user)
        else:
            # SECURITY: Generic error message, log actual reason
            self._log_auth_attempt(username, "invalid_credentials", False)
            return AuthResult(success=False, error="Invalid credentials")
```

---

## 🔒 Implementing Secure Prompting Patterns

### GitHub Copilot: Secure Prompt Templates

**Setup Custom Instructions**
```markdown
# Create .github/copilot-instructions.md
## Security Guidelines for Copilot
- Always validate user inputs
- Use parameterized queries for databases
- Never hardcode secrets or credentials
- Implement proper error handling
- Add security comments to generated code
- Follow OWASP security guidelines
- Use bcrypt for password hashing (min cost 12)
- Implement rate limiting for authentication endpoints
```

**Secure Code Generation Example**
```python
# Copilot prompt: "Create secure password reset endpoint following our security guidelines"

@app.route('/reset-password', methods=['POST'])
def reset_password():
    """
    COPILOT-GENERATED: Password reset endpoint
    Security Guidelines Applied: Input validation, rate limiting, secure tokens
    """
    try:
        # Input validation
        data = request.get_json()
        email = data.get('email', '').strip().lower()
        
        # Validate email format
        if not re.match(r'^[^@]+@[^@]+\.[^@]+$', email):
            return jsonify({'error': 'Invalid email format'}), 400
        
        # Rate limiting check
        if rate_limiter.is_limited(f"password_reset:{email}"):
            return jsonify({'error': 'Too many requests'}), 429
        
        # Generate secure reset token
        reset_token = secrets.token_urlsafe(32)
        expiry = datetime.utcnow() + timedelta(hours=1)
        
        # Store reset token securely
        store_reset_token(email, reset_token, expiry)
        
        # Send email (don't reveal if email exists)
        send_reset_email(email, reset_token)
        
        # Generic success response
        return jsonify({'message': 'If email exists, reset link sent'}), 200
        
    except Exception as e:
        logger.error(f"Password reset error: {str(e)}")
        return jsonify({'error': 'Internal server error'}), 500
```

### Cursor: Context-Aware Security Implementation

**Setup Security Context**
```markdown
# Create .cursor/security-context.md
## Project Security Standards
- Framework: Flask with SQLAlchemy
- Authentication: JWT with 24-hour expiration
- Password hashing: bcrypt cost factor 12
- Rate limiting: 5 attempts per 15 minutes
- Input validation: All user inputs sanitized
- Database: Parameterized queries only
- Logging: Security events logged, no PII
- Error handling: Generic error messages
```

**Cursor Multi-File Security Implementation**
```
# Cursor Command: Apply security context to implement OAuth integration
Implement OAuth2 authentication integration following our security standards:
1. Update models/user.py with OAuth fields
2. Create services/oauth_service.py with secure OAuth flow
3. Add routes/auth.py OAuth endpoints
4. Update middleware for OAuth token validation
5. Ensure all changes follow our security context

Apply security annotations and review markers to all generated code.
```

**Generated Security-First Code**
```python
# services/oauth_service.py - Generated by Cursor
"""
CURSOR-GENERATED: OAuth2 Service Implementation
Security Context Applied: ✅
Multi-file Security Review Required: ✅
"""

import secrets
import hashlib
from typing import Optional, Dict, Any
from datetime import datetime, timedelta

class SecureOAuthService:
    """
    OAuth2 implementation following project security standards
    """
    
    def __init__(self, client_id: str, client_secret: str, redirect_uri: str):
        # CURSOR-SECURITY: Validate OAuth configuration
        self._validate_oauth_config(client_id, client_secret, redirect_uri)
        self.client_id = client_id
        self.client_secret = client_secret
        self.redirect_uri = redirect_uri
        self.rate_limiter = RateLimiter()
    
    def generate_auth_url(self, state: Optional[str] = None) -> Dict[str, str]:
        """
        CURSOR-SECURITY: Generate secure OAuth authorisation URL
        - CSRF protection via state parameter
        - Secure random state generation
        - URL parameter validation
        """
        
        # Generate cryptographically secure state
        if not state:
            state = secrets.token_urlsafe(32)
        
        # Store state for validation
        self._store_oauth_state(state, expires_in=600)  # 10 minutes
        
        # Construct authorisation URL with security parameters
        auth_url = (
            f"https://oauth-provider.com/oauth/authorize?"
            f"client_id={self.client_id}&"
            f"redirect_uri={self.redirect_uri}&"
            f"response_type=code&"
            f"state={state}&"
            f"scope=read:user"
        )
        
        return {
            "auth_url": auth_url,
            "state": state,
            "expires_at": (datetime.utcnow() + timedelta(minutes=10)).isoformat()
        }
```

### Claude: Advanced Security Analysis and Implementation

**Security Analysis Prompt**
```markdown
# Claude Security Analysis Request
Please perform a comprehensive security analysis of this authentication system and then provide an improved implementation:

[Attach existing code]

Analysis Requirements:
1. Identify all potential security vulnerabilities
2. Check against OWASP Top 10
3. Evaluate against authentication best practices
4. Assess timing attack vulnerabilities
5. Review error handling for information disclosure
6. Analyze rate limiting effectiveness

Then provide:
1. Detailed security report with risk ratings
2. Improved implementation addressing all issues
3. Security testing recommendations
4. Monitoring and alerting suggestions
```

**Claude's Comprehensive Security Implementation**
```python
"""
CLAUDE SECURITY ANALYSIS & IMPLEMENTATION
===========================================

SECURITY ANALYSIS RESULTS:
- Timing Attack Vulnerability: HIGH RISK
- Information Disclosure: MEDIUM RISK  
- Rate Limiting: NEEDS IMPROVEMENT
- Input Validation: GOOD
- Error Handling: NEEDS IMPROVEMENT

IMPROVEMENTS IMPLEMENTED:
✅ Constant-time operations for user lookup
✅ Enhanced rate limiting with progressive delays
✅ Secure error handling with generic messages
✅ Comprehensive security logging
✅ Input sanitization and validation
✅ JWT security enhancements
"""

import hmac
import hashlib
import secrets
import time
from typing import Optional, Dict, Any
from datetime import datetime, timedelta

class SecurityHardenedAuthenticator:
    """
    Enterprise-grade authentication with comprehensive security measures
    """
    
    def __init__(self, config: AuthConfig):
        self.config = config
        self.rate_limiter = AdvancedRateLimiter()
        self.security_logger = SecurityLogger()
        self.metrics = SecurityMetrics()
    
    def authenticate_user(self, username: str, password: str, client_ip: str) -> AuthResult:
        """
        CLAUDE-SECURITY: Hardened authentication with comprehensive protections
        
        Security Measures Implemented:
        - Constant-time operations to prevent timing attacks
        - Progressive rate limiting with IP and user-based limits
        - Comprehensive security event logging
        - Generic error responses to prevent enumeration
        - Input sanitization and validation
        - Secure JWT generation with proper claims
        """
        
        start_time = time.time()
        
        try:
            # SECURITY: Input validation and sanitization
            username_clean = self._sanitize_username(username)
            if not self._validate_input_format(username_clean, password):
                self._log_security_event("auth_invalid_format", username_clean, client_ip)
                return self._create_generic_failure_response(start_time)
            
            # SECURITY: Multi-layer rate limiting
            rate_limit_result = self.rate_limiter.check_limits(username_clean, client_ip)
            if rate_limit_result.is_limited:
                self._log_security_event("auth_rate_limited", username_clean, client_ip, 
                                       {"limit_type": rate_limit_result.limit_type})
                return self._create_rate_limited_response(start_time, rate_limit_result.retry_after)
            
            # SECURITY: Constant-time user lookup
            user_data = self._get_user_constant_time(username_clean)
            
            # SECURITY: Constant-time password verification
            password_valid = self._verify_password_constant_time(password, user_data)
            
            if password_valid and user_data:
                # SUCCESS PATH: Generate secure session
                token_data = self._generate_secure_session(user_data, client_ip)
                
                self._log_security_event("auth_success", username_clean, client_ip, {
                    "user_id": user_data.id,
                    "session_id": token_data["session_id"]
                })
                
                self.metrics.record_successful_auth(username_clean, client_ip)
                
                return AuthResult(
                    success=True,
                    token=token_data["token"],
                    expires_at=token_data["expires_at"],
                    session_id=token_data["session_id"]
                )
            
            else:
                # FAILURE PATH: Generic response with security logging
                actual_reason = "user_not_found" if not user_data else "invalid_password"
                
                self._log_security_event("auth_failure", username_clean, client_ip, {
                    "reason": actual_reason,
                    "attempt_count": self.rate_limiter.get_attempt_count(username_clean)
                })
                
                self.metrics.record_failed_auth(username_clean, client_ip, actual_reason)
                
                return self._create_generic_failure_response(start_time)
        
        except Exception as e:
            # SECURITY: Error handling without information disclosure
            error_id = secrets.token_hex(8)
            self._log_security_event("auth_error", username, client_ip, {
                "error_id": error_id,
                "error_type": type(e).__name__
            })
            
            # Log full error details securely (not in response)
            self.security_logger.log_error(f"Authentication error {error_id}: {str(e)}")
            
            return AuthResult(
                success=False,
                error="Authentication service temporarily unavailable",
                error_id=error_id
            )
    
    def _verify_password_constant_time(self, password: str, user_data: Optional[UserData]) -> bool:
        """
        CLAUDE-SECURITY: Constant-time password verification
        Prevents timing attacks by always performing bcrypt operation
        """
        
        # Always perform bcrypt operation to maintain constant time
        if user_data and user_data.password_hash:
            return bcrypt.checkpw(password.encode('utf-8'), user_data.password_hash)
        else:
            # Perform dummy bcrypt to maintain timing consistency
            bcrypt.checkpw(b'dummy_password', b'$2b$12$dummy.hash.to.maintain.timing')
            return False
    
    def _get_user_constant_time(self, username: str) -> Optional[UserData]:
        """
        CLAUDE-SECURITY: Constant-time user lookup
        Always takes the same amount of time regardless of user existence
        """
        
        # Implementation that always performs same database operations
        # to prevent user enumeration via timing attacks
        user = db.session.query(User).filter(
            User.username == username,
            User.is_active == True
        ).first()
        
        # Always perform additional query to maintain consistent timing
        db.session.query(User).filter(User.id == -1).first()
        
        return UserData.from_user(user) if user else None
```

---

## 🔍 Implementing Security Scanning Integration

### GitHub Copilot: Security-First Development

**Setup Security Extensions**
```json
// .vscode/extensions.json
{
  "recommendations": [
    "GitHub.copilot",
    "GitHub.copilot-chat",
    "ms-vscode.vscode-json",
    "redhat.vscode-yaml",
    "ms-python.python",
    "ms-python.flake8",
    "ms-python.black-formatter"
  ]
}
```

**Copilot Security Workflow**
```python
# Use Copilot Chat for security review
"""
@workspace Can you review this authentication code for security issues?

Focus on:
- Input validation vulnerabilities
- SQL injection possibilities  
- Authentication bypass scenarios
- Information disclosure risks
- Rate limiting effectiveness

Provide specific recommendations for each issue found.
"""

def login_endpoint():
    # Copilot will analyse and suggest improvements
    pass
```

### Cursor: Integrated Security Pipeline

**Setup Security Pipeline in Cursor**
```yaml
# .cursor/security-pipeline.yml
security_checks:
  pre_commit:
    - name: "Secret Scanning"
      command: "truffleHog git file://. --only-verified"
      
    - name: "SAST Scanning"
      command: "semgrep --config=auto --error ."
      
    - name: "Dependency Check"
      command: "safety check"
      
  ai_integration:
    - name: "AI Security Review"
      trigger: "on_ai_generation"
      action: "auto_scan_generated_code"
      
    - name: "Security Annotation"
      trigger: "on_code_generation"
      action: "add_security_comments"
```

**Cursor Security-Integrated Development**
```
# Cursor Command with Security Integration
Generate a secure API endpoint for user registration with:
- Automatic security scanning of generated code
- Integration with our security pipeline
- Security annotations in generated code
- Compliance with our security standards

After generation, run security scans and provide remediation suggestions.
```

### Claude: Advanced Security Analysis

**Comprehensive Security Review Prompt**
```markdown
# Claude Security Analysis
Please perform a comprehensive security audit of this codebase:

1. **Static Analysis**: Identify potential vulnerabilities
2. **Dynamic Analysis**: Suggest runtime security tests
3. **Architecture Review**: Evaluate security architecture
4. **Compliance Check**: Verify against security standards
5. **Threat Modeling**: Identify potential attack vectors

Provide:
- Detailed vulnerability report with CVSS scores
- Specific remediation steps
- Security testing recommendations
- Monitoring and alerting suggestions
- Compliance gap analysis
```

**Claude's Security Implementation with Scanning**
```python
"""
CLAUDE COMPREHENSIVE SECURITY IMPLEMENTATION
===========================================

INTEGRATED SECURITY MEASURES:
- Real-time vulnerability scanning
- Automated security testing
- Continuous security monitoring
- Compliance validation
- Threat detection and response
"""

import subprocess
import json
from typing import Dict, List, Any
from dataclasses import dataclass

@dataclass
class SecurityScanResult:
    tool: str
    severity: str
    vulnerability_type: str
    description: str
    file_path: str
    line_number: int
    remediation: str

class IntegratedSecurityScanner:
    """
    CLAUDE-GENERATED: Integrated security scanning system
    Combines multiple security tools with AI analysis
    """
    
    def __init__(self):
        self.scan_tools = {
            'semgrep': self._run_semgrep,
            'bandit': self._run_bandit,
            'safety': self._run_safety_check,
            'truffleHog': self._run_secret_scan
        }
        self.ai_analyzer = AISecurityAnalyzer()
    
    def comprehensive_security_scan(self, target_path: str) -> Dict[str, Any]:
        """
        CLAUDE-SECURITY: Run comprehensive security scanning
        Integrates multiple tools with AI-powered analysis
        """
        
        scan_results = []
        
        # Run all security scanning tools
        for tool_name, scan_function in self.scan_tools.items():
            try:
                tool_results = scan_function(target_path)
                scan_results.extend(tool_results)
                
                # Log scan completion
                self._log_scan_completion(tool_name, len(tool_results))
                
            except Exception as e:
                self._log_scan_error(tool_name, str(e))
        
        # AI-powered analysis of results
        ai_analysis = self.ai_analyzer.analyze_vulnerabilities(scan_results)
        
        # Generate comprehensive report
        security_report = self._generate_security_report(scan_results, ai_analysis)
        
        # Trigger alerts for critical issues
        self._handle_critical_vulnerabilities(scan_results)
        
        return security_report
    
    def _run_semgrep(self, target_path: str) -> List[SecurityScanResult]:
        """Run Semgrep SAST scanning"""
        cmd = [
            'semgrep', 
            '--config=auto', 
            '--json', 
            '--quiet',
            target_path
        ]
        
        result = subprocess.run(cmd, capture_output=True, text=True)
        
        if result.returncode == 0:
            semgrep_data = json.loads(result.stdout)
            return self._parse_semgrep_results(semgrep_data)
        else:
            raise Exception(f"Semgrep scan failed: {result.stderr}")
    
    def _run_bandit(self, target_path: str) -> List[SecurityScanResult]:
        """Run Bandit security linter for Python"""
        cmd = [
            'bandit',
            '-r', target_path,
            '-f', 'json',
            '-q'
        ]
        
        result = subprocess.run(cmd, capture_output=True, text=True)
        
        if result.stdout:
            bandit_data = json.loads(result.stdout)
            return self._parse_bandit_results(bandit_data)
        
        return []
    
    def _generate_security_report(self, scan_results: List[SecurityScanResult], 
                                 ai_analysis: Dict[str, Any]) -> Dict[str, Any]:
        """
        CLAUDE-SECURITY: Generate comprehensive security report
        Combines tool results with AI insights
        """
        
        # Categorize vulnerabilities by severity
        critical_vulns = [r for r in scan_results if r.severity == 'CRITICAL']
        high_vulns = [r for r in scan_results if r.severity == 'HIGH']
        medium_vulns = [r for r in scan_results if r.severity == 'MEDIUM']
        low_vulns = [r for r in scan_results if r.severity == 'LOW']
        
        # Calculate risk score
        risk_score = self._calculate_risk_score(scan_results)
        
        return {
            "scan_timestamp": datetime.utcnow().isoformat(),
            "risk_score": risk_score,
            "vulnerability_summary": {
                "critical": len(critical_vulns),
                "high": len(high_vulns),
                "medium": len(medium_vulns),
                "low": len(low_vulns),
                "total": len(scan_results)
            },
            "vulnerabilities": [
                {
                    "id": f"{r.tool}_{hash(r.description)}"[:16],
                    "tool": r.tool,
                    "severity": r.severity,
                    "type": r.vulnerability_type,
                    "description": r.description,
                    "file": r.file_path,
                    "line": r.line_number,
                    "remediation": r.remediation
                }
                for r in scan_results
            ],
            "ai_analysis": ai_analysis,
            "recommendations": self._generate_recommendations(scan_results, ai_analysis),
            "compliance_status": self._check_compliance_status(scan_results)
        }
```

---

## 📝 Implementing Audit Trails and Logging

### GitHub Copilot: Logging Integration

**Setup Copilot Activity Logging**
```python
# Use Copilot to generate comprehensive logging system
"""
@workspace Generate a logging system that captures:
- All AI-generated code with metadata
- Human review decisions and modifications
- Security scan results
- Deployment status and approvals
- Compliance validation results

Include structured logging with JSON format for easy parsing.
"""

import logging
import json
from datetime import datetime
from typing import Dict, Any, Optional

class AIActivityLogger:
    """
    COPILOT-GENERATED: Comprehensive AI activity logging system
    Captures all AI interactions for audit and compliance
    """
    
    def __init__(self, log_file: str = "ai_activity.log"):
        self.logger = logging.getLogger("ai_activity")
        self.logger.setLevel(logging.INFO)
        
        # JSON formatter for structured logging
        formatter = logging.Formatter('%(message)s')
        
        # File handler for persistent logging
        file_handler = logging.FileHandler(log_file)
        file_handler.setFormatter(formatter)
        self.logger.addHandler(file_handler)
    
    def log_ai_generation(self, tool: str, prompt: str, generated_code: str, 
                         developer: str, file_path: str) -> str:
        """
        COPILOT-AUDIT: Log AI code generation event
        """
        event_id = f"ai_gen_{int(datetime.utcnow().timestamp())}"
        
        log_entry = {
            "event_id": event_id,
            "event_type": "ai_code_generation",
            "timestamp": datetime.utcnow().isoformat(),
            "tool": tool,
            "developer": developer,
            "file_path": file_path,
            "prompt_hash": hashlib.sha256(prompt.encode()).hexdigest(),
            "code_hash": hashlib.sha256(generated_code.encode()).hexdigest(),
            "lines_generated": len(generated_code.split('\n')),
            "review_status": "pending"
        }
        
        self.logger.info(json.dumps(log_entry))
        return event_id
```

### Cursor: Comprehensive Activity Tracking

**Setup Cursor Audit System**
```python
# Cursor-generated comprehensive audit system
"""
CURSOR COMMAND: Create enterprise audit logging system that:
1. Tracks all AI interactions across the development lifecycle
2. Integrates with existing monitoring systems
3. Provides compliance-ready audit trails
4. Includes automated report generation
5. Supports multiple output formats (JSON, CSV, PDF)

Generate with security annotations and review markers.
"""

import asyncio
import aiofiles
from typing import AsyncGenerator, Dict, Any
from dataclasses import dataclass, asdict
from datetime import datetime, timedelta

@dataclass
class AuditEvent:
    """
    CURSOR-GENERATED: Structured audit event for AI activities
    """
    event_id: str
    timestamp: datetime
    event_type: str
    tool: str
    developer: str
    session_id: str
    metadata: Dict[str, Any]
    
    def to_dict(self) -> Dict[str, Any]:
        return {
            **asdict(self),
            'timestamp': self.timestamp.isoformat()
        }

class EnterpriseAuditSystem:
    """
    CURSOR-GENERATED: Enterprise-grade audit system for AI development
    SECURITY-REVIEWED: Implements tamper-evident logging
    COMPLIANCE-READY: Supports SOC2, ISO27001, GDPR requirements
    """
    
    def __init__(self, config: AuditConfig):
        self.config = config
        self.event_queue = asyncio.Queue()
        self.storage = AuditStorage(config.storage_config)
        self.encryption = AuditEncryption(config.encryption_key)
        self.compliance_checker = ComplianceChecker()
        
    async def log_ai_interaction(self, interaction_data: Dict[str, Any]) -> str:
        """
        CURSOR-AUDIT: Log AI interaction with full context
        """
        event_id = self._generate_event_id()
        
        # Create structured audit event
        audit_event = AuditEvent(
            event_id=event_id,
            timestamp=datetime.utcnow(),
            event_type=interaction_data.get('type', 'ai_interaction'),
            tool=interaction_data.get('tool', 'unknown'),
            developer=interaction_data.get('developer', 'unknown'),
            session_id=interaction_data.get('session_id', ''),
            metadata={
                'prompt_context': interaction_data.get('prompt_context', {}),
                'generated_content': self._hash_content(
                    interaction_data.get('generated_content', '')
                ),
                'review_status': interaction_data.get('review_status', 'pending'),
                'security_scan_results': interaction_data.get('security_results', {}),
                'compliance_flags': interaction_data.get('compliance_flags', []),
                'performance_metrics': interaction_data.get('performance', {})
            }
        )
        
        # Encrypt sensitive data
        encrypted_event = await self.encryption.encrypt_event(audit_event)
        
        # Add to processing queue
        await self.event_queue.put(encrypted_event)
        
        # Trigger compliance check
        await self.compliance_checker.validate_event(audit_event)
        
        return event_id
    
    async def generate_audit_report(self, start_date: datetime, 
                                  end_date: datetime,
                                  report_type: str = 'compliance') -> Dict[str, Any]:
        """
        CURSOR-AUDIT: Generate comprehensive audit reports
        """
        
        # Retrieve events for date range
        events = await self.storage.get_events_by_date_range(start_date, end_date)
        
        # Decrypt events for analysis
        decrypted_events = []
        for event in events:
            decrypted = await self.encryption.decrypt_event(event)
            decrypted_events.append(decrypted)
        
        # Generate report based on type
        if report_type == 'compliance':
            return await self._generate_compliance_report(decrypted_events)
        elif report_type == 'security':
            return await self._generate_security_report(decrypted_events)
        elif report_type == 'performance':
            return await self._generate_performance_report(decrypted_events)
        else:
            return await self._generate_summary_report(decrypted_events)
```

### Claude: Advanced Audit and Compliance System

**Comprehensive Audit Implementation**
```python
"""
CLAUDE ENTERPRISE AUDIT SYSTEM
===============================

COMPLIANCE FRAMEWORKS SUPPORTED:
- SOC 2 Type II
- ISO 27001:2013
- GDPR Article 30 (Records of Processing)
- NIST Cybersecurity Framework
- PCI DSS (where applicable)

AUDIT TRAIL FEATURES:
- Tamper-evident logging with cryptographic signatures
- Immutable audit records with blockchain verification
- Real-time compliance monitoring and alerting
- Automated report generation for multiple frameworks
- Integration with enterprise SIEM systems
"""

import hashlib
import hmac
import json
from datetime import datetime, timedelta
from typing import Dict, List, Any, Optional
from cryptography.fernet import Fernet
from cryptography.hazmat.primitives import hashes
from cryptography.hazmat.primitives.kdf.pbkdf2 import PBKDF2HMAC

class EnterpriseComplianceAuditor:
    """
    CLAUDE-GENERATED: Enterprise-grade compliance and audit system
    Designed for regulated industries with strict audit requirements
    """
    
    def __init__(self, config: ComplianceConfig):
        self.config = config
        self.crypto_key = self._derive_encryption_key(config.master_key)
        self.cipher_suite = Fernet(self.crypto_key)
        self.audit_chain = AuditBlockchain()
        self.compliance_rules = ComplianceRuleEngine()
        
    def log_ai_development_activity(self, activity: AIActivityData) -> AuditRecord:
        """
        CLAUDE-COMPLIANCE: Create tamper-evident audit record
        
        Compliance Requirements Met:
        - SOC 2 CC6.8: Audit logging and monitoring
        - ISO 27001 A.12.4.1: Event logging
        - GDPR Article 30: Records of processing activities
        """
        
        # Create base audit record
        audit_record = AuditRecord(
            record_id=self._generate_audit_id(),
            timestamp=datetime.utcnow(),
            activity_type=activity.type,
            ai_tool=activity.tool,
            developer_id=activity.developer_id,
            session_id=activity.session_id,
            
            # AI-specific audit data
            ai_model_version=activity.ai_model_version,
            prompt_classification=self._classify_prompt_sensitivity(activity.prompt),
            generated_content_hash=self._hash_content(activity.generated_content),
            human_review_status=activity.review_status,
            
            # Security and compliance data
            security_classification=activity.security_classification,
            data_categories=activity.data_categories,
            compliance_flags=self._evaluate_compliance_flags(activity),
            
            # Technical metadata
            code_complexity_score=self._calculate_complexity(activity.generated_content),
            security_scan_results=activity.security_scan_results,
            quality_metrics=activity.quality_metrics,
            
            # Governance data
            approval_chain=activity.approval_chain,
            risk_assessment=self._assess_risk_level(activity),
            business_justification=activity.business_justification
        )
        
        # Add cryptographic integrity protection
        audit_record.integrity_hash = self._calculate_integrity_hash(audit_record)
        audit_record.digital_signature = self._sign_record(audit_record)
        
        # Encrypt sensitive fields
        encrypted_record = self._encrypt_sensitive_fields(audit_record)
        
        # Add to immutable audit chain
        chain_entry = self.audit_chain.add_record(encrypted_record)
        
        # Store in multiple locations for redundancy
        self._store_audit_record(encrypted_record, chain_entry)
        
        # Check compliance in real-time
        compliance_status = self.compliance_rules.evaluate_record(audit_record)
        if not compliance_status.is_compliant:
            self._trigger_compliance_alert(audit_record, compliance_status)
        
        return audit_record
    
    def generate_compliance_report(self, framework: str, 
                                 period: timedelta) -> ComplianceReport:
        """
        CLAUDE-COMPLIANCE: Generate framework-specific compliance reports
        
        Supported Frameworks:
        - SOC2: Focus on security controls and access management
        - ISO27001: Information security management system
        - GDPR: Data protection and privacy compliance
        - NIST: Cybersecurity framework alignment
        """
        
        end_date = datetime.utcnow()
        start_date = end_date - period
        
        # Retrieve audit records for period
        audit_records = self._get_audit_records(start_date, end_date)
        
        # Framework-specific analysis
        if framework.upper() == 'SOC2':
            return self._generate_soc2_report(audit_records, start_date, end_date)
        elif framework.upper() == 'ISO27001':
            return self._generate_iso27001_report(audit_records, start_date, end_date)
        elif framework.upper() == 'GDPR':
            return self._generate_gdpr_report(audit_records, start_date, end_date)
        elif framework.upper() == 'NIST':
            return self._generate_nist_report(audit_records, start_date, end_date)
        else:
            raise ValueError(f"Unsupported compliance framework: {framework}")
    
    def _generate_soc2_report(self, records: List[AuditRecord], 
                             start_date: datetime, end_date: datetime) -> SOC2Report:
        """
        CLAUDE-SOC2: Generate SOC 2 Type II compliance report
        
        Trust Service Criteria Covered:
        - Security (CC6): AI tool access controls and monitoring
        - Availability (CC7): AI service availability and incident response
        - Processing Integrity (CC8): AI code quality and validation
        - Confidentiality (CC9): AI data handling and protection
        """
        
        # CC6 - Security Analysis
        security_metrics = {
            'ai_access_violations': self._count_access_violations(records),
            'unauthorized_ai_usage': self._count_unauthorized_usage(records),
            'security_incidents': self._analyze_security_incidents(records),
            'access_review_compliance': self._assess_access_reviews(records)
        }
        
        # CC7 - Availability Analysis  
        availability_metrics = {
            'ai_service_uptime': self._calculate_ai_uptime(records),
            'incident_response_times': self._analyze_incident_response(records),
            'backup_and_recovery': self._assess_backup_procedures(records)
        }
        
        # CC8 - Processing Integrity Analysis
        integrity_metrics = {
            'ai_code_review_rate': self._calculate_review_rate(records),
            'quality_gate_compliance': self._assess_quality_gates(records),
            'error_rates': self._analyze_error_rates(records),
            'human_oversight_compliance': self._assess_human_oversight(records)
        }
        
        # Generate comprehensive SOC 2 report
        return SOC2Report(
            report_period=f"{start_date.date()} to {end_date.date()}",
            total_ai_activities=len(records),
            security_metrics=security_metrics,
            availability_metrics=availability_metrics,
            integrity_metrics=integrity_metrics,
            compliance_exceptions=self._identify_compliance_exceptions(records),
            remediation_actions=self._generate_remediation_actions(records),
            control_effectiveness=self._assess_control_effectiveness(records),
            management_assertion=self._generate_management_assertion(records)
        )
```

---

## 📊 Tool Comparison and Recommendations

### Feature Comparison Matrix

| Feature | GitHub Copilot | Cursor | Claude |
|---------|----------------|--------|--------|
| **Code Generation** | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| **Security Analysis** | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| **Context Understanding** | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| **Enterprise Integration** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ |
| **Compliance Support** | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| **Audit Capabilities** | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |

### Use Case Recommendations

#### For Daily Development
**Recommended**: GitHub Copilot + Cursor
- Copilot for inline suggestions and quick completions
- Cursor for complex refactoring and multi-file changes
- Combined workflow provides comprehensive coverage

#### For Security-Critical Applications
**Recommended**: Claude + GitHub Copilot Enterprise
- Claude for comprehensive security analysis and planning
- Copilot Enterprise for security-aware code generation
- Manual security review for all AI-generated code

#### For Enterprise Compliance
**Recommended**: All three tools with proper governance
- GitHub Copilot Enterprise for audit trails and enterprise controls
- Cursor for development efficiency with security configurations
- Claude for compliance analysis and report generation

---

## 🎯 Implementation Roadmap

### Phase 1: Tool Setup and Configuration (Week 1-2)
- [ ] Configure GitHub Copilot with security settings
- [ ] Set up Cursor with enterprise security configurations
- [ ] Establish Claude integration for security analysis
- [ ] Create security scanning pipeline integration

### Phase 2: Security Integration (Week 3-4)
- [ ] Implement automated security scanning for AI-generated code
- [ ] Set up audit logging for all AI interactions
- [ ] Configure compliance checking workflows
- [ ] Establish security review processes

### Phase 3: Governance and Compliance (Week 5-6)
- [ ] Deploy comprehensive audit system
- [ ] Implement compliance reporting automation
- [ ] Set up real-time monitoring and alerting
- [ ] Establish governance dashboards

### Phase 4: Optimization and Training (Week 7-8)
- [ ] Optimize AI tool configurations based on usage patterns
- [ ] Conduct team training on secure AI development practices
- [ ] Establish continuous improvement processes
- [ ] Create knowledge sharing and best practice documentation

---

*This comprehensive guide provides practical, hands-on examples for implementing AI development best practices using specific tools. Each example includes security considerations, compliance measures, and governance frameworks suitable for enterprise environments.*

---

**Navigation**: [← Prompting Best Practices](prompting-best-practices.html) | [Enterprise Alignment →](enterprise-alignment.html)
