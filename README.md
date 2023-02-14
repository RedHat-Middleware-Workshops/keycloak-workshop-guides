# This is the Red Hat SSO workshop guides project

The workshop can be deployed on OpenShift 4.7+ cluster. 
It requires the SSO, CodeReady, DG 8.2, Prometheus, Grafana and CodeReady operators. for more details check the redhat-cop/agnosticD repo on how to provision the infra.

## Contributing
Incase you find an issue; use the issues; 
Incase you want to fix an issue; create a PR to the development branch

## Docs/Instructions
After you have cloned this repo. Head off to the docs directory and run the runFile.sh and head of to localhost:4000 to view the documentation.

## How to deploy infra via RHPDS
If you are looking to deploy this workshop on OpenShift 4.7:

```
git clone https://github.com/redhat-cop/agnosticd
```

Request an OpenShift 4.7 Workshop from rhpds. Once the environment is provisioned, you should receive an email with credentials and links to your environment.

If you have generated a password less private key, you might want to do something like this:

```
cat ~/.ssh/id_ansible.pub | ssh  username@bastion.cph-05c1.example.opentlc.com "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >>  ~/.ssh/authorized_keys"
```

## Review the defaults variable file

* This file link:./defaults/main.yml[./defaults/main.yml] contains all the variables you need to define to control the deployment of your workload.
* The variable *ocp_username* is mandatory to assign the workload to the correct OpenShift user.
* A variable *silent=True* can be passed to suppress debug messages.
* You can modify any of these default values by adding `-e "variable_name=variable_value"` to the command line

### Deploy a Workload with the `ocp-workload` playbook [Mostly for testing]

```
TARGET_HOST="bastion.cph-05c1.OpenShiftworkshop.com"
OCP_USERNAME="username"
WORKLOAD="ocp4-workload-sso-workshop"
GUID=
SSH_KEY=~/.ssh/id_rsa
AWS_REGION=us-east-1

# a TARGET_HOST is specified in the command line, without using an inventory file
ansible-playbook -i ${TARGET_HOST}, ./configs/ocp-workloads/ocp-workload.yml \
    -e"ansible_ssh_private_key_file=${SSH_KEY}" \
    -e"ansible_user=${OCP_USERNAME}" \
    -e"ocp_username=${OCP_USERNAME}" \
    -e"ocp_workload=${WORKLOAD}" \
    -e"silent=False" \
    -e"guid=${GUID}" \
    -e"aws_region=${AWS_REGION}" \
    -e"num_users=50" \
    -e"subdomain_base=${TARGET_HOST}" \
    -e"ACTION=create"

```

### What does the installer do? 
- Install CodeReady Workspace Operator in namespace `codeready`
- Installs Operators DG 8.2, Prometheus, Grafana, OpenShift Serverless
- Creates project for each user e.g. `user1-sso` and `user1-sso2`