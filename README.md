# Weatherapp
There was a beautiful idea of building an app that would show the upcoming weather. The developers wrote a nice backend and a frontend following the latest principles and - to be honest - bells and whistles. However, the developers did not remember to add any information about the infrastructure or even setup instructions in the source code.

Luckily we now have docker compose saving us from installing the tools on our computer, and making sure the app looks (and is) the same in development and in production. All we need is someone to add the few missing files!

## Prerequisites
- An openweathermap API key. This is set on the docker-compose.yml file on the project root
- You have ansible installed
- You have your AWS ec2 key pair, access_key and Secret key
- You have a RHEL 7 instance provisioned on AWS with base requirements installed. You can use the repos below to provision and set up the instance(s). _The provision used in the 
    - https://github.com/muge13/terraform-provision-aws-vpc
    - https://github.com/muge13/terraform-provision-aws-ec2
    - https://github.com/muge13/ansible-configure-base
## Provision (cloud)
Terraform script to create AWS EC2 instances per availability zone
### Instructions
Export aws credentials to the working environment
 ```
export AWS_ACCESS_KEY_ID="{}" &&  export AWS_SECRET_ACCESS_KEY="{}" && export AWS_REGION="eu-west-2"
```
Replace the variables in the command to suite your environment.
```
terraform init
terraform plan -var="vpc_id={{vpc-id}}" -var="key_name={{key-name}}"
terraform apply -var="vpc_id={{vpc-id}}" -var="key_name={{key-name}}"
terraform output
```
You can modify these common variables (Others are available on the variables.tf file):
- vpc-id
- aws_region
- key-name
- zone
- instance_count (Per availability zone)
- service
- role
- instance_type
- service (custom tag)
- role (custom tag)
- block_device
To destroy the instances (Given that the instance doesn't have termination protection enabled)
```
terraform destroy -var="vpc_id={{vpc-id}}" -var="key_name={{key-name}}"
```
## Ansible
### Docker
Docker is installed and Setup as part of the Ansible setup roles.
We add Dockerfile(s) and a docker-compose.yml file to define how to roll out the container images. 
The images are built on top of node:lts-alpine and the volumes linked and mounted to the deploy-folder on the running server.
The frontend is bound on port 80 and 8000, the backend is bound on port 9000. 
### Deployment (Ansible)
The deployment is run as part of the ansible Deploy roles.
```
ansible-playbook -i deploy/ec2.py -T 60 -f 100 --private-key=~/{user}.pem deploy/main.yml
```
#### Process:
- cleans up and initializes the deployment folder
- pulls in the latest code from the repo and 
- runs the docker containers based the docker-compose setup