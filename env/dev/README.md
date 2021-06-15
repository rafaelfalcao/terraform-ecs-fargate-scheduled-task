# Environment Dev Terraform

Creates the dev environment's infrastructure. These templates are designed to be customized.
The optional components can be removed by simply deleting the `.tf` file.


## Components

| Name | Description | Optional |
|------|-------------|:----:|
| [main.tf][edm] | Terrform remote state, AWS provider, output |  |
| [ecs.tf][ede] | ECS Cluster, Task Definition, ecsTaskExecutionRole, CloudWatch Log Group |  |
| [nsg.tf][edn] | NSG for Task |  |
| [secretsmanager.tf][edn] | Sets up integration with Secrets Manager |  |
| [role.tf][edr] | Application Role for container | Yes |
| [cicd.tf][edc] | IAM user that can be used by CI/CD systems | Yes |
| [logs-logzio.tf][edll] | Ship container logs to logz.io | Yes |
| [secretsmanager.tf][edsm] | Add a Secrets Manager secret with a CMK KMS key. Also gives app role and ECS task definition role access to read secrets from Secrets Manager | Yes |
| [ssm-parameters.tf][ssm] | Add a CMK KMS key for use with SSM Parameter Store. Also gives ECS task definition role access to read secrets from parameter store. | Yes |


## Usage

```
# Sets up Terraform to run
$ terraform init

# Executes the Terraform run
$ terraform apply
```


## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|:----:|:-----:|:-----:|
| app | The application's name | string | - | yes |
| aws\_profile | The AWS Profile to use | string | - | yes |
| container\_name | name of the container in the task definition | string | `app` | no |
| environment | The environment that is being built | string | - | yes |
| logz\_token | The auth token to use for sending logs to Logz.io | string | - | yes |
| logz\_url | The endpoint to use for sending logs to Logz.io | string | `https://listener.logz.io:8071` | no |
| private\_subnets | The private subnets, minimum of 2, that are a part of the VPC(s) | string | - | yes |
| public\_subnets | The public subnets, minimum of 2, that are a part of the VPC(s) | string | - | yes |
| region | The AWS region to use for the dev environment's infrastructure Currently, Fargate is only available in `us-east-1`. | string | `us-east-1` | no |
| saml\_role | The SAML role to use for adding users to the ECR policy | string | - | yes |
| schedule\_expression | The shedule on which to run the fargate task. Follows the CloudWatch Event Schedule Expression format: https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/ScheduledEvents.html | string | - | yes |
| secrets\_saml\_users | The users (email addresses) from the saml role to give access | list | - | yes |
| tags | Tags for the infrastructure | map | - | yes |
| vpc | The VPC to use for the Fargate cluster | string | - | yes |

## Outputs

| Name | Description |
|------|-------------|
| aws\_profile | Command to set the AWS\_PROFILE |
| cicd\_keys | The AWS keys for the CICD user to use in a build system |
| docker\_registry | The URL for the docker image repo in ECR |
| register | Command to register a new task definition for the task |
| status | Command to view the status of the Fargate service |



[edm]: main.tf
[ede]: ecs.tf
[edl]: lb.tf
[edn]: nsg.tf
[edlhttp]: lb-http.tf
[edlhttps]: lb-https.tf
[edd]: dashboard.tf
[edr]: role.tf
[edc]: cicd.tf
[edap]: autoscale-perf.tf
[edat]: autoscale-time.tf
[edll]: logs-logzio.tf
[edsm]: secretsmanager.tf
[ssm]: ssm-parameters.tf
[alb-docs]: https://docs.aws.amazon.com/elasticloadbalancing/latest/application/application-load-balancers.html
[up]: https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/ScheduledEvents.html
