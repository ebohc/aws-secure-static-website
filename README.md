# AWS Secure Static Website Infrastructure

A production-ready, secure architecture for hosting static websites on AWS with built-in security controls and compliance monitoring.

## 🛡️ Security Features

- **Private S3 Origin** with CloudFront OAI (Origin Access Identity)
- **AWS WAF** with OWASP Top 10 protection rules
- **Least Privilege IAM** policies
- **Full Audit Trail** with AWS CloudTrail
- **DDoS Protection** with AWS Shield

## 🚀 Quick Start

### Deploy with AWS CloudFormation:

```bash
aws cloudformation create-stack \
  --stack-name secure-website \
  --template-body file://infrastructure/cloudformation-template.yml \
  --capabilities CAPABILITY_NAMED_IAM



Prerequisites

AWS Account
AWS CLI installed
Basic understanding of AWS services


Security Layers:

Network Layer: CloudFront CDN with AWS Shield
Application Layer: AWS WAF with managed rules
Storage Layer: Private S3 bucket
Monitoring Layer: CloudTrail for auditing


Project Outcomes

90% reduced attack surface by eliminating public S3 access
Automatic blocking of OWASP Top 10 vulnerabilities
Full API audit trail for security investigations



Project Structure
aws-secure-static-website/
├── infrastructure/     # CloudFormation templates
├── website-content/   # Sample website files
├── security/          # Security policies
├── scripts/           # Deployment scripts
└── architecture/      # Design diagrams


