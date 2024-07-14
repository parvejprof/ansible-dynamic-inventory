# Ansible Dynamic Inventory for AWS EC2

This README provides instructions on how to set up and use Ansible Dynamic Inventory for managing AWS EC2 instances.

## Prerequisites

1. **Ansible**: Ensure you have Ansible installed:
    ```bash
    apt install ansible
    ```

2. **Boto3**: The AWS SDK for Python is required to interact with AWS services. Install it using pip:
    ```bash
    pip install boto3 botocore
    ```

3. **AWS Credentials**: Configure your AWS credentials, use AWS CLI to configure or use environment variables:
    ```bash
    aws configure
    ```
    Or

    ```bash
    export AWS_ACCESS_KEY_ID='your_access_key_id'
    export AWS_SECRET_ACCESS_KEY='your_secret_access_key'
    export AWS_REGION='your_region'
    ```    

## Setting Up Ansible Dynamic Inventory

1. **Configure the Inventory Script**:
    Edit the `aws_ec2.yaml` file to match your environment and AWS setup.

2. **Update Your Ansible Configuration**:
    Ensure your `ansible.cfg` is set to use the dynamic inventory.

    ```ini
    [defaults]
    inventory=/etc/ansible/aws_ec2.yaml
    host_key_checking=false
    remote_user=ec2-user
    private_key_file=/etc/ansible/testkey.pem

    [privilege_escalation]
    become=True
    become_method=sudo
    become_user=root
    #become_ask_pass=False
    ```

## Using Ansible with AWS Dynamic Inventory

1. **Test the Inventory**:
    Verify that the dynamic inventory is working correctly by listing your EC2 instances.

    ```bash
    ansible-inventory -i aws_ec2.yml  --list
    ansible-inventory -i aws_ec2.yml  --graph
    ```
2. **filter instances by tags**:
    Verify that the dynamic inventory is collecting instances based on tag

    ```bash
    ansible test01* --list-hosts
    ```

3. **Run Playbooks**:
    You can now run your Ansible playbooks against the dynamic inventory to install the httpd server and start it!

    Run the playbook:
    ```bash
    ansible-playbook webserver.yml
    ```

## Troubleshooting

- **Challenges**:
    - Verify that the `boto3` library is installed and up to date.
    - If python3 interpreter is there, then need to use 'dnf' instead of 'yum' in playbook config file.

## Done!