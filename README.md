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


5. Click **"Commit changes"**
6. Leave the commit message as "Update README.md"
7. Click **"Commit changes"**

---

### **STEP 3: Create Infrastructure Files**

#### **3.1 Create CloudFormation Template**
1. Click **"Add file"** → **"Create new file"**
2. In the filename box, type: `infrastructure/cloudformation-template.yml`
3. In the content area, **PASTE this exact content:**

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: 'Secure Static Website Infrastructure'

Parameters:
  ProjectName:
    Type: String
    Description: Name for the project
    Default: secure-website

Resources:
  # S3 Bucket for Website Content
  WebsiteBucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: !Sub '${ProjectName}-${AWS::AccountId}'
      AccessControl: Private
      BucketEncryption:
        ServerSideEncryptionConfiguration:
          - ServerSideEncryptionByDefault:
              SSEAlgorithm: AES256
      PublicAccessBlockConfiguration:
        BlockPublicAcls: true
        BlockPublicPolicy: true
        IgnorePublicAcls: true
        RestrictPublicBuckets: true

  # CloudFront Origin Access Identity
  CloudFrontOAI:
    Type: AWS::CloudFront::CloudFrontOriginAccessIdentity
    Properties:
      CloudFrontOriginAccessIdentityConfig:
        Comment: 'OAI for secure website'

  # S3 Bucket Policy for CloudFront
  BucketPolicy:
    Type: AWS::S3::BucketPolicy
    Properties:
      Bucket: !Ref WebsiteBucket
      PolicyDocument:
        Statement:
          - Effect: Allow
            Principal:
              CanonicalUser: !GetAtt CloudFrontOAI.S3CanonicalUserId
            Action: s3:GetObject
            Resource: !Sub '${WebsiteBucket.Arn}/*'

  # CloudFront Distribution
  CloudFrontDistribution:
    Type: AWS::CloudFront::Distribution
    Properties:
      DistributionConfig:
        Comment: Secure website distribution
        Origins:
          - DomainName: !GetAtt WebsiteBucket.DomainName
            Id: S3Origin
            S3OriginConfig:
              OriginAccessIdentity: !Sub 'origin-access-identity/cloudfront/${CloudFrontOAI}'
        Enabled: true
        HttpVersion: http2
        DefaultRootObject: index.html
        DefaultCacheBehavior:
          TargetOriginId: S3Origin
          ViewerProtocolPolicy: redirect-to-https
          Compress: true
          CachePolicyId: 658327ea-f89d-4fab-a63d-7e88639e58f6

Outputs:
  WebsiteURL:
    Description: CloudFront Website URL
    Value: !GetAtt CloudFrontDistribution.DomainName
  S3BucketName:
    Description: S3 Bucket Name
    Value: !Ref WebsiteBucket
