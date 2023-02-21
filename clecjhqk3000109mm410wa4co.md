# Automating Prometheus Monitoring on AWS with Ansible and Terraform

👀 Are you tired of manually setting up and configuring Prometheus monitoring on AWS? Look no further than Ansible and Terraform. You can automate the process by utilizing these tools, from provisioning EC2 instances to deploying and configuring Prometheus.

🚀 This tutorial will look at deploying Prometheus monitoring on AWS using Ansible for configuration management and Terraform for infrastructure as code. We will be deploying Prometheus on EC2 instances and behind a load balancer.

## 🧑‍💻 Prerequisites

* An AWS account
    
* Basic knowledge of AWS and Terraform
    
* Basic knowledge of Ansible
    
* A text editor
    

## 🚀 Project Overview

We will deploy Prometheus monitoring on three EC2 instances behind a load balancer. We will use Terraform to create the EC2 instances, load balancer, and Ansible to configure the instances and deploy Prometheus.

Here is the project directory map:

📁 prometheus-ansible-terraform/

├── 📄 inventory.ini

├── 📄 [main.tf](http://main.tf)

├── 📄 playbook.yml

├── 📄 prometheus.yml

├── 📄 site.yml

Here are the steps we will take:

1. Deploy EC2 instances and a load balancer using Terraform
    
2. Configure EC2 instances using Ansible
    
3. Install and configure Prometheus on the EC2 instances
    

## 🔧 Terraform Configuration

1. Create a new directory for your Terraform configuration and create a [`main.tf`](http://main.tf) file.
    
2. Inside the [`main.tf`](http://main.tf) file, define the provider as AWS, and specify the region:
    
    ```yaml
    pythonCopy codeprovider "aws" {
      region = "us-east-1"
    }
    ```
    
3. Create an EC2 instance and load balancer resource in the [`main.tf`](http://main.tf) file:
    
    ```yaml
    resource "aws_instance" "prometheus" {
      ami           = "ami-0c94855ba95c71c99"
      instance_type = "t2.micro"
      count         = 3
    
      tags = {
        Name = "prometheus"
      }
    }
    
    resource "aws_lb" "prometheus" {
      name = "prometheus"
      internal = false
    
      listener {
        lb_port = 80
        protocol = "HTTP"
    
        instance_port = 9090
        instance_protocol = "HTTP"
      }
    }
    ```
    
    This Terraform configuration will create three EC2 instances and a load balancer. The EC2 instances will be running the Amazon Machine Image (AMI) with ID `ami-0c94855ba95c71c99`, which is a base Amazon Linux image. We will use this as the base for our Prometheus deployment.
    
    The load balancer will listen on port 80 and forward traffic to port 9090 on the EC2 instances.
    
4. Run the following command to initialize the Terraform project:
    
    ```bash
    terraform init
    terraform apply
    ```
    

## 🛠️ Ansible Configuration

1. Create a new directory for your Ansible configuration and create a `inventory.ini` file. Inside the file, specify the EC2 public IP addresses provisioned by the terraform code:
    
    ```bash
    [prometheus]
    ec2-x-x-x-x.compute-1.amazonaws.com
    ec2-y-y-y-y.compute-1.amazonaws.com
    ec2-z-z-z-z.compute-1.amazonaws.com
    ```
    
2. Create a `site.yml` file with the following playbook that configures the EC2 instances:
    
    ```yaml
    - hosts: prometheus
      become: true
      tasks:
        - name: Install necessary packages
          yum:
            name:
              - wget
              - git
              - unzip
              - java-1.8.0-openjdk
            state: present
    
        - name: Download and install node_exporter
          get_url:
            url: https://github.com/prometheus/node_exporter/releases/download/v1.0.1/node_exporter-1.0.1.linux-amd64.tar.gz
            dest: /tmp/node_exporter.tar.gz
          become_user: ec2-user
    
        - name: Extract node_exporter
          unarchive:
            src: /tmp/node_exporter.tar.gz
            dest: /usr/local/bin/
            remote_src: true
            copy: no
          become_user: root
    
        - name: Configure node_exporter
          template:
            src: templates/node_exporter.service.j2
            dest: /etc/systemd/system/node_exporter.service
            owner: root
            group: root
            mode: '0644'
          notify:
            - reload systemd
    ```
    
3. Create a `prometheus.yml` file that defines the Prometheus configuration:
    

```yaml
   global:
     scrape_interval: 5s

   scrape_configs:
     - job_name: ec2-instances
       ec2_sd_configs:
         - region: us-east-1
           port: 9100
       relabel_configs:
         - source_labels: [__meta_ec2_tag_Name]
           target_label: instance
         - source_labels: [__meta_ec2_private_ip]
           target_label: ip

     - job_name: load-balancers
       ec2_sd_configs:
         - region: us-east-1
           port: 9100
           filters:
             - name: "tag:Component"
               values: ["LoadBalancer"]
       relabel_configs:
         - source_labels: [__meta_ec2_tag_Name]
           target_label: load_balancer
         - source_labels: [__meta_ec2_private_ip]
           target_label: ip
```

This configuration file specifies two scrape jobs: one for EC2 instances and one for load balancers. The EC2 job uses EC2 service discovery to dynamically discover all running instances and scrape their metrics using the Node Exporter on port 9100.

The load balancer job filters by instances with a Component tag set to LoadBalancer.

Next, let's create an Ansible playbook to install and configure Prometheus on our EC2 instances. We'll create a file named `playbook.yml` in the same directory as our `site.yml` playbook.

1. Create a new Ansible playbook in a file named `playbook.yml` that installs and configures Prometheus:
    
    ```yaml
    - name: Install Prometheus
      hosts: prometheus
      become: true
      gather_facts: true
    
      tasks:
        - name: Install Prometheus
          apt:
            name: prometheus
            state: present
    
        - name: Copy Prometheus configuration file
          template:
            src: prometheus.yml
            dest: /etc/prometheus/prometheus.yml
    
        - name: Start Prometheus service
          systemd:
            name: prometheus
            state: started
            enabled: yes
    ```
    
2. Run the `site.yml` playbook to configure the EC2 instances:
    
    ```bash
    ansible-playbook -i inventory.ini site.yml
    ```
    
3. Run the `prometheus.yml` playbook to install and configure Prometheus:
    
    ```bash
    ansible-playbook -i inventory.ini playbook.yml
    ```
    

## Conclusion🎉

In this tutorial, we learned how to use Terraform to deploy an infrastructure on AWS that includes EC2 instances, an Application Load Balancer, and a Prometheus server. We also learned how to use Ansible to configure and manage the Prometheus server. With these tools, we can automate the deployment and management of our infrastructure and ensure that our Prometheus monitoring system is up and running smoothly.

This is a basic example, but you can do many other things with Terraform and Ansible to manage your infrastructure and automate your workflows. By mastering these tools, you can build robust and scalable cloud-based systems that meet the needs of your business or organization.

Don't hesitate to reach out to me on [**GitHub**](https://github.com/nextwebb), [**Twitter**](https://twitter.com/iam_nextwebb), and [**LinkedIn**](https://www.linkedin.com/in/peterson-oaikhenah-102645144/) if you have any questions or want to learn more. And don't forget to give a thumbs up 👍, leave a comment 💬 and share this article with your network 😊.

## References:

* Terraform documentation: [**https://www.terraform.io/docs/index.html**](https://www.terraform.io/docs/index.html)
    
* Ansible documentation: [**https://docs.ansible.com/**](https://docs.ansible.com/)
    
* Prometheus documentation: [**https://prometheus.io/docs/introduction/overview/**](https://prometheus.io/docs/introduction/overview/)
    
* AWS documentation: [**https://aws.amazon.com/documentation/**](https://aws.amazon.com/documentation/)