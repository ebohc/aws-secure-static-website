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
