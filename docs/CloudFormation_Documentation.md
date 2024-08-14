DevOps Capstone Project: Initial CloudFormation Templates Documentation
Project Overview
This project is part of a comprehensive DevOps capstone designed to showcase end-to-end infrastructure automation, containerization, CI/CD pipelines, real-time monitoring, and more using a variety of tools and technologies. The initial phase focuses on automating the infrastructure provisioning using AWS CloudFormation.

Goals:
Automate the creation of network resources including VPC, Subnets, and Internet Gateway.
Deploy EC2 instances with associated IAM roles, security groups, and necessary networking.
Maintain infrastructure as code using CloudFormation with a modular and reusable approach.
Project Structure
The project is structured using multiple CloudFormation templates, organized as nested stacks to promote modularity and reusability. This allows each component of the infrastructure to be managed separately, making updates easier and reducing complexity.

Templates:
RootTemplate.yaml:

Serves as the entry point for deploying the infrastructure.
References the S3 bucket where nested stack templates are stored.
Deploys the VPCStack and ComputeStack using the nested stack approach.
VPC.yaml:

Handles the creation of the VPC and associated networking resources.
Resources include:
VPC
Public and Private Subnets
Internet Gateway
Route Tables and associations
Compute.yaml:

Manages the compute resources, specifically EC2 instances and related IAM roles and security groups.
Resources include:
EC2 Instance
IAM Role and Instance Profile
Security Group for SSH access
S3.yaml: (Deployed separately)

Creates an S3 bucket for storing CloudFormation templates.
This template was used to initialize the S3 bucket that stores other CloudFormation templates.
Resources Deployed
VPCStack:

VPC: Custom VPC with DNS support enabled.
Public Subnet: A subnet configured to auto-assign public IPs for instances.
Private Subnet: A subnet for private resources.
Internet Gateway: Enables internet access for resources in the public subnet.
Route Tables: Configured for routing traffic within the VPC.
ComputeStack:

EC2 Instance: A t2.micro instance with a role allowing EC2 and S3 permissions.
IAM Role: An IAM role for the EC2 instance, with policies allowing specific AWS actions.
Security Group: Allows SSH access from specific IP ranges.
Deployment Process
Initialize S3 Bucket:

The S3.yaml template was used to create an S3 bucket to store the nested stack templates.
Upload Templates to S3:

Uploaded VPC.yaml and Compute.yaml to the S3 bucket.
Deploy VPC Stack:

Deployed the VPCStack independently to ensure that the exported values (like VPC ID and Public Subnet ID) were available for other stacks.
Deploy Root Stack:

Deployed RootTemplate.yaml, which pulls the nested templates from S3 and deploys the VPCStack (if not done separately) and ComputeStack.
Challenges and Solutions
Issue with ImportValue:
The ImportValue function in the RootTemplate.yaml caused a rollback because the VPC stack needed to be deployed first. I resolved this by deploying the VPCStack independently, ensuring that the necessary outputs were available.
TemplateURL Error:
Initial errors were due to outdated templates not being synchronized with S3. Uploading the correct templates to S3 resolved the issue.
Best Practices Followed
Modularity: By using nested stacks, I ensured that each component of the infrastructure can be managed and updated independently.
Version Control: All templates and documentation are tracked in a Git repository, ensuring changes are auditable and reversible.
Parameterization: The use of parameters like EnvironmentName allows the templates to be reused across different environments (e.g., production, staging).
Next Steps
Documentation: I will continue to document subsequent phases of the project, including Docker and Kubernetes integration, CI/CD pipeline setup with Jenkins, and monitoring with Prometheus and Grafana.
Deployment: I will begin the next phase by deploying the containerization stack and integrating it with the existing infrastructure.