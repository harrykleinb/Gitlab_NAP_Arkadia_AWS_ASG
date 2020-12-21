# Gitlab_NAP_Arkadia_AWS_ASG


That Pipeline is the first of a group of 3 pipelines:

    1) Gitlab_NAP_Arkadia_AWS_ASG
    2) Gitlab_Setup_NAP_and_F5CS_DNSLB
    3) Remove_instance_f5cs_dns_lb


All the pipelines use mainly Ansible, and the AWS API for some actions (for simplicity).


**RESUME of the workflow:**


*Gitlab_NAP_Arkadia_AWS_ASG:* <br>
That pipeline deploys a full AWS VPC with all the needed resources.<br>
NAP instances are deployed through an Auto Scale Group.<br>
Arkadia instances are also deployed via an Auto Scale Group.<br>

*Gitlab_Setup_NAP_and_F5CS_DNSLB:* <br>
That second pipeline is automatically triggered When a NAP instance starts.<br>
That pipeline setups the NAP with the required nginx config files (nginx.conf, nap policy, etc).<br>
During the setup of a NAP instance, NGINX-ASG is configured to auto discover the Arkadia instances.<br>
Finally, the pipeline creates or updates a DNS LB F5 CS with the public IP address of the new NAP instance.<br>

    -> See the README of Gitlab_Setup_NAP_and_F5CS_DNSLB for more details.

*Remove_instance_f5cs_dns_lb:*<br>
When a NAP instance stops, that third pipeline is automatically triggered.<br>
That pipeline updates the DNS LB Service for removing the public IP addess of the NAP instance which has been stopped.<br>

    -> See the README of remove_instance_f5cs_dns_lb for more details.


Pre-Requirements:

    . AWS account with an access key id and a secret access key.
    . Gitlab Server into AWS with at least 1 Gitlab Runner.
    . Any AWS Region can be used for the Gilab Server and Runners.


Deploy a complete infrastructure into AWS with:

    . 1 VPC with a route table and an internet gateway
    . 2 AZ
    . 2 Subnets for NAPs (1 subnet in each AZ)
    . 2 Subnets for Web Servers (1 Subnet in each AZ)
    . 1 Auto Scale Group for NAPs
    . 1 Auto Scale Group for Web Servers
    . 1 Security Group for NAPs
    . 1 Security Group for Web Servers
    . 1 Role and policy (required for NGINX ASG  



Variables are:

    . var_key_pair                      # Name of the AWS key pair you want to bind to the instances
    . var_region                        # AWS region where you want to deploy the infra
    . var_id                            # Will be used to create specific name for the objects and instances
    . var_ip_admin:                     # IP Address of the admin host
    . var_nb_NAP                        # Number of NAP instances to start with into the AWS Auto Scale Group
    . var_nb_Arkadia                    # Number of Arkadia instances to start wirth into the AWS Auto Scale Group
