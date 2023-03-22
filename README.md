# NGINX Management Suite Lab

This repository will build out a 4 server deployment in AWS allowing the user to test the NGINX Management Suite, API Connectivity Manager, Developer Portal, and API Gateway instances. Additionally, the NMS Security Monitor module is installed on the NMS instance. Finally, an instance with NGINX App Protect WAF is provided, and its agent is configured to send security metrics and events to NMS.

> Note: This deployment leverages [Tailscale](https://tailscale.com/) to connect into the EC2 instances.

![Lab diagram](./NMS_ACM_lab_env.png)

## Prerequisites
You need to have an AWS subscription, an NGINX+ license, and an NGINX Management Suite license with the Instance Manager and Security Monitoring modules enabled. You can get a free trial of NGINX+ [here](https://www.nginx.com/free-trial-request/).

### AWS programmatic access
You will need to have the AWS CLI installed and configured with your AWS credentials. You can find more information on how to do that [here](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html).

### Licensing
You will need to save your *nginx-repo.crt* and *nginx-repo.key* in the base folder of this project. Also, you will need to base64 encode your NMS license file and save it to *nms-license-b64.txt* in the base folder of this project. If you're using an NMS evaluation license, you can copy `nginx-instance-manager-eval.lic` to the base folder of this project, then run the following command to create the base64 encoded file that the module expects:
```shell
base64 nginx-instance-manager-eval.lic > nms-license-b64.txt
``` 

### SSH access
You will need an SSH key pair in AWS to access the EC2 instances. You can import one using the AWS cli by running the following command:

```shell
aws ec2 import-key-pair --key-name <your key name> --public-key-material fileb://~/.ssh/<your public key file>
```

## Deployment

Create a `terraform.tfvars` file in the base folder of this project with this content updated as appropriate:
A `terraform.tfvars.example` is provided as a template.
```shell
cp terraform.tfvars.example terraform.tfvars
```

```terraform
# terraform.tfvars.example

region              = "<your aws region name>"
owner_name          = "<your owner name>"
owner_email         = "<your owner email address>"
key_name            = "<your existing AWS ssh key name>"
tailscale_auth_key  = "<your tailscale auth key>"
nms_admin_password  = "<your password for the NMS admin account>"
```

Run the following Terraform commands to deploy the environment:

```bash
terraform init
terraform apply --auto-approve
```

Once deployment is complete, you can browse to `https://<nms server tailscale public ip>`.
