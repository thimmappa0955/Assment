# Cisco_Assignment

AWS Elastic Container Service (ECS) Terraform module

Terraform module which creates ECS resources on AWS.

This module focuses purely on ECS and nothing else. Therefore only these resources can be created with this module:

ECS
IAM
However, having said the above to have a proper ECS cluster up and running multiple resources are needed. In most cases creating these resources is heavily opinionated and or context-bound. That is why this module does not create these resources. But you still need them to have a production ready environment. Therefore the example area shows how to create everything needed for a production environment.

Terraform versions

Terraform 0.12. Pin module version to ~> v2.0. Submit pull-requests to master branch.

Terraform 0.11. Pin module version to ~> v1.0. Submit pull-requests to terraform011 branch.

Usage

module "ecs" {
  source = "terraform-aws-modules/ecs/aws"

  name = "my-ecs"

  container_insights = true

  capacity_providers = ["FARGATE", "FARGATE_SPOT"]

  default_capacity_provider_strategy = {
    capacity_provider = "FARGATE_SPOT"
  }

  tags = {
    Environment = "Development"
  }
}
Conditional creation

Sometimes you need to have a way to create ECS resources conditionally but Terraform does not allow to use count inside module block, so the solution is to specify argument create_ecs.

# ECS cluster will not be created
module "ecs" {
  source  = "terraform-aws-modules/ecs/aws"
  version = "~> 2.0"

  create_ecs = false
  # ... omitted
}
Examples

Complete ECS
Requirements

Name	Version
terraform	>= 0.12.6
aws	>= 2.48
Providers

Name	Version
aws	>= 2.48
Inputs

Name	Description	Type	Default	Required
capacity_providers	List of short names of one or more capacity providers to associate with the cluster. Valid values also include FARGATE and FARGATE_SPOT.	list(string)	[]	no
container_insights	Controls if ECS Cluster has container insights enabled	bool	false	no
create_ecs	Controls if ECS should be created	bool	true	no
default_capacity_provider_strategy	The capacity provider strategy to use by default for the cluster. Can be one or more.	map(any)	{}	no
name	Name to be used on all the resources as identifier, also the name of the ECS cluster	string	null	no
tags	A map of tags to add to ECS Cluster	map(string)	{}	no
Outputs

Name	Description
this_ecs_cluster_arn	ARN of the ECS Cluster
this_ecs_cluster_id	ID of the ECS Cluster
this_ecs_cluster_name	The name of the ECS cluster