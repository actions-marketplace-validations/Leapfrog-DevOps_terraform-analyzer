# Terraform Analyzer

An intelligent Terraform automation tool that combines infrastructure deployment with AI-powered error analysis and cost optimization. This project provides automated Terraform workflows with built-in error detection, automatic fixes, and cost analysis using OpenAI and Infracost.

## 🚀 Features

### Core Capabilities
- **Automated Terraform Operations**: Plan and apply infrastructure changes via GitHub Actions
- **AI-Powered Error Analysis**: Automatically analyzes Terraform failures using OpenAI GPT-4
- **Intelligent Auto-Fix**: Generates and applies code fixes for common Terraform errors
- **Cost Analysis**: Integrates with Infracost for infrastructure cost estimation and tracking
- **Pull Request Integration**: Provides detailed plan summaries and cost breakdowns in PR comments

### Infrastructure Components
- **AWS S3**: Bucket creation and management
- **AWS Lambda**: Function deployment with S3 integration
- **AWS EC2**: Instance provisioning (modular)
- **AWS VPC**: Network infrastructure (modular)
- **Remote State**: S3 backend with DynamoDB locking

## 📁 Project Structure

```
terraform-analyzer/
├── .github/workflows/
│   ├── terraform-analyzer.yml    # Main Terraform workflow
│   └── cost-analyzer.yml         # Cost analysis workflow
├── terraform/
│   ├── modules/                  # Reusable Terraform modules
│   │   ├── s3/                  # S3 bucket module
│   │   ├── lambda/              # Lambda function module
│   │   ├── ec2/                 # EC2 instance module
│   │   ├── vpc/                 # VPC networking module
│   │   └── remote-state/        # State management module
│   ├── main.tf                  # Main Terraform configuration
│   ├── variables.tf             # Input variables
│   └── outputs.tf               # Output values
├── terraform-analyzer.py        # AI error analysis script
└── README.md
```

## 🛠️ Setup & Configuration

### Prerequisites
- AWS Account with appropriate permissions
- GitHub repository with Actions enabled
- OpenAI API key (for error analysis)
- Infracost API key (for cost analysis)

### Required Secrets
Configure these secrets in your GitHub repository:

```yaml
DEPLOYMENT_ROLE: arn:aws:iam::ACCOUNT:role/deployment-role
OPENAI_API_KEY: sk-...
INFRACOST_API_KEY: ico-...
GITHUB_TOKEN: ghp_... (automatically provided)
```

### Required Variables
Set these repository variables:

```yaml
AWS_REGION: us-east-1
ENVIRONMENT: dev
DEPLOY_LAMBDA: true
```

### AWS IAM Setup
Create a deployment role with permissions for:
- S3 bucket operations
- Lambda function management
- EC2 instance management
- DynamoDB table access (for state locking)

## 🚦 Usage

### Automatic Triggers
The workflow automatically runs on:
- **Pull Requests** to `final_test` branch → Runs `terraform plan`
- **Manual Dispatch** → Choose between `plan` or `apply`

### Manual Execution
1. Go to Actions tab in your repository
2. Select "Terraform - Plan & Apply" workflow
3. Click "Run workflow"
4. Choose action: `plan` or `apply`

### Workflow Behavior

#### On Pull Request
1. Runs `terraform plan`
2. Posts plan summary as PR comment
3. Uploads plan artifacts for cost analysis
4. Triggers cost analyzer workflow
5. If errors occur, runs AI analysis and creates auto-fix branch

#### On Manual Apply
1. Runs `terraform apply`
2. If successful, deploys infrastructure
3. If errors occur, analyzes and attempts auto-fix

## 🤖 AI Error Analysis

### How It Works
When Terraform operations fail, the analyzer:

1. **Extracts Context**: Parses error logs and identifies relevant files
2. **AI Analysis**: Sends complete code context to OpenAI GPT-4
3. **Generates Fixes**: Creates corrected Terraform code blocks
4. **Applies Changes**: Automatically updates files with fixes
5. **Creates PR**: Pushes fixes to `auto-tf-fix` branch

### Supported Error Types
- Resource configuration errors
- Provider version conflicts
- Variable reference issues
- Module configuration problems
- AWS resource conflicts

### Auto-Fix Features
- Preserves original code structure
- Only modifies problematic sections
- Creates backup files before changes
- Generates detailed fix explanations
- Maintains code formatting and style

## 💰 Cost Analysis

### Infracost Integration
- Analyzes infrastructure costs before deployment
- Compares costs between different plan versions
- Tracks cost changes over time
- Stores cost data in dedicated `infracost` branch

### Cost Tracking
- Baseline costs stored per environment
- Diff analysis for PR changes
- Historical cost data retention
- Integration with GitHub step summaries

## 🔧 Configuration

### Terraform Variables
Customize infrastructure in `terraform/variables.tf`:

```hcl
variable "aws_region" {
  description = "AWS region"
  type        = string
  default     = "us-east-1"
}

variable "s3_bucket_name" {
  description = "S3 bucket name"
  type        = string
  default     = "sample-open-source"
}

variable "lambda_function_name" {
  description = "Lambda function name"
  type        = string
  default     = "sample-lambda"
}
```

### Workflow Customization
Modify `.github/workflows/terraform-analyzer.yml` to:
- Change trigger branches
- Adjust Terraform version
- Modify auto-fix branch names
- Configure additional AWS regions

## 📊 Monitoring & Debugging

### Logs & Artifacts
- Terraform logs stored in `logs/terraform.log`
- Plan files uploaded as GitHub artifacts
- Cost analysis results in step summaries
- Auto-fix results in GitHub summaries

### Troubleshooting
1. Check workflow logs for detailed error information
2. Review AI analysis output in failed runs
3. Examine auto-fix branch for proposed changes
4. Verify AWS permissions and credentials

## 🤝 Contributing

1. Fork the repository
2. Create feature branch: `git checkout -b feature/new-feature`
3. Make changes and test thoroughly
4. Commit changes: `git commit -m 'Add new feature'`
5. Push to branch: `git push origin feature/new-feature`
6. Create Pull Request

## 📝 License

This project is open source and available under the [MIT License](LICENSE).

## 🆘 Support

For issues and questions:
- Create GitHub Issues for bugs and feature requests
- Check workflow logs for deployment issues
- Review AI analysis output for Terraform errors
- Consult AWS documentation for permission issues

---

**Built with ❤️ by Team 5 Opensource**