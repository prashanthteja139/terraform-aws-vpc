Terraform AWS VPC Module

A reusable and production-ready Terraform module to provision an AWS VPC with public, private, and database subnets, NAT Gateway, route tables, and optional VPC peering with the default VPC.

This module is designed for real-world workloads (ex: Roboshop, microservices, backend systems) and follows best practices for network isolation, tagging, and scalability.

🚀 Features

Creates a custom AWS VPC

Public, Private, and Database subnets across multiple AZs

Internet Gateway for public access

NAT Gateway for outbound access from private & DB subnets

Separate route tables for each subnet type

Optional VPC peering with default VPC

Consistent tagging strategy

Modular, reusable, and environment-friendly


📁 Repository Structure

terraform-aws-vpc/

├── data.tf        # Data sources (AZs, default VPC, main route table)

├── locals.tf      # Common tags and AZ selection logic

├── outputs.tf     # Exported VPC & subnet IDs

├── peering.tf     # Optional VPC peering configuration

├── variables.tf   # Input variables

├── vpc.tf         # Core VPC, subnet, IGW, NAT & routing logic

├── README.md

🧱 Architecture Overview

VPC with configurable CIDR

2 Availability Zones (auto-selected)

Public Subnets

Internet Gateway attached

Auto-assign public IPs

Private Subnets

Outbound internet via NAT Gateway

Database Subnets

Isolated, outbound via NAT (no inbound internet)

VPC Peering (Optional)

Peers with default VPC

Routes added on both sides

🔧 Usage Example
module "vpc" {
  source = "./terraform-aws-vpc"

  project     = "roboshop"
  environment = "dev"

  cidr_block = "10.0.0.0/16"

  public_subnet_cidrs   = ["10.0.1.0/24", "10.0.2.0/24"]
  private_subnet_cidrs  = ["10.0.3.0/24", "10.0.4.0/24"]
  database_subnet_cidrs = ["10.0.5.0/24", "10.0.6.0/24"]

  is_peering_required = true
}

📥 Input Variables
Variable	                      Type	                    Description
project	                        string	                  Project name
environment	                    string	                  Environment (dev, qa, prod)
cidr_block	                    string	                  VPC CIDR block
public_subnet_cidrs	            list(string)	            Public subnet CIDRs
private_subnet_cidrs	          list(string)	            Private subnet CIDRs
database_subnet_cidrs         	list(string)	            Database subnet CIDRs
is_peering_required	            bool	                    Enable VPC peering with default VPC
*_tags	                        map(string)	              Resource-specific tags


📤 Outputs
Output	                   Description
vpc_id	                   VPC ID
public_subnet_ids	         List of public subnet IDs
private_subnet_ids	       List of private subnet IDs
database_subnet_ids	       List of database subnet IDs

🏷️ Tagging Strategy

All resources are tagged with:

Project     = <project>
Environment = <environment>
Terraform   = true


Additional custom tags can be passed per resource.

⚠️ Notes & Best Practices

NAT Gateway is created only once (cost-optimized setup)

AZ count is currently limited to 2 for predictability

Database subnets are isolated from direct internet access

VPC peering is optional and disabled by default

Designed to plug directly into EKS, ECS, or EC2-based stacks

🎯 Ideal Use Cases

Microservices architecture

E-commerce / shopping-style apps

Secure backend + database deployments

Dev / QA / Prod VPC standardization

Terraform learning with real-world patterns

👤 Author

Prashanth Teja
DevOps Engineer | Terraform | AWS | IaC
GitHub: prashanthteja139
