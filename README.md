# Terraform AWS Infrastructure

A hands-on Infrastructure as Code (IaC) project that provisions AWS networking and compute resources using Terraform.

## Project Overview

This project demonstrates how to provision an AWS environment with Terraform, configure public and private subnets, manage network routing, and deploy a web server on an Amazon EC2 instance.

It also demonstrates secure instance access through AWS Systems Manager (SSM) Session Manager without requiring SSH access.

## Architecture

```text
                         Internet
                            |
                     Internet Gateway
                            |
                    Public Route Table
                            |
                    Public Subnet
                      10.0.1.0/24
                            |
                     Security Group
                      TCP Port 80
                            |
                      EC2 Instance
                   Amazon Linux 2023
                            |
                     Apache HTTP Server
                            |
                 Hello from Terraform EC2!

                         VPC
                      10.0.0.0/16

                    Private Subnet
                      10.0.2.0/24
                            |
                    Private Route Table
```

The private subnet and its route table are provisioned as part of the networking setup. A NAT Gateway is not configured in this project.

## Technologies Used

- Terraform
- Amazon Web Services (AWS)
- Amazon VPC
- Public and private subnets
- Internet Gateway and route tables
- Security Groups and Network ACLs
- Amazon EC2
- AWS IAM instance profile and role
- AWS Systems Manager Session Manager
- Amazon Linux 2023
- Apache HTTP Server
- Git and GitHub

## Infrastructure Resources

- VPC with CIDR `10.0.0.0/16`
- Public subnet: `10.0.1.0/24`
- Private subnet: `10.0.2.0/24`
- Internet Gateway
- Public and private route tables
- Security Group allowing inbound HTTP traffic on TCP port 80
- IAM role with `AmazonSSMManagedInstanceCore`
- EC2 instance running Amazon Linux 2023
- Apache HTTP server serving a custom web page

## Deployment Workflow

1. Configure AWS CLI authentication.
2. Initialize Terraform providers.
3. Validate the Terraform configuration.
4. Review the execution plan.
5. Apply the Terraform configuration.
6. Verify the AWS networking and EC2 resources.
7. Connect to the instance using SSM Session Manager.
8. Install and start Apache.
9. Verify the web server locally and through the public IP address.

## Terraform Commands

Initialize Terraform:

```bash
terraform init
```

Format configuration files:

```bash
terraform fmt
```

Validate configuration:

```bash
terraform validate
```

Review planned changes:

```bash
terraform plan
```

Provision infrastructure:

```bash
terraform apply
```

Destroy resources when they are no longer needed:

```bash
terraform destroy
```

Review the plan carefully before applying changes. Destroying the infrastructure will remove the Terraform-managed resources.

## Validation and Testing

The deployment was manually validated using AWS CLI, SSM Session Manager, and HTTP checks.

- Verified EC2 instance state and network interface.
- Verified the public IP address and Internet Gateway route.
- Verified Security Group egress and Network ACL rules.
- Verified SSM registration and `PingStatus: Online`.
- Installed Apache successfully through an SSM session.
- Confirmed Apache was running.
- Received `HTTP/1.1 200 OK` from `curl -I http://localhost`.
- Verified the custom web page through the public IP address.

## Troubleshooting Experience

During testing, the initial EC2 user-data script failed to install Apache because package repository requests timed out. SSM Agent also initially failed to connect to the SSM endpoint.

After troubleshooting the AWS networking configuration and confirming SSM registration, I connected through Session Manager, installed Apache, started the service, and verified successful HTTP responses.

This demonstrated practical troubleshooting of AWS connectivity, IAM instance access, Linux services, and web-server deployment.

## Security Considerations

- Uses AWS Systems Manager Session Manager instead of opening inbound SSH access.
- Requires IMDSv2 for EC2 instance metadata access.
- Excludes Terraform state files, saved plans, and local provider data from version control.
- Never commit AWS credentials, secrets, or sensitive variable files.
- Review Security Group rules and IAM permissions before production use.

## Cost Considerations

AWS resources can incur charges while they remain provisioned. Review active resources and destroy the test infrastructure when it is no longer needed.

## Future Improvements

- Automate Apache installation and page deployment through Terraform user data.
- Add automated infrastructure checks.
- Improve configuration reusability with Terraform variables and outputs.
- Introduce Terraform modules for reusable infrastructure components.
- Add CI validation using GitHub Actions.

## Author

**Samiul Sami**

GitHub: [Samiul-Sami-New](https://github.com/Samiul-Sami-New)
