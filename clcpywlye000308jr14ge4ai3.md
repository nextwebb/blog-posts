# Create Docker Containers Using Ansible 🐳🚀:  Introduction

🤔 Are you tired of manually setting up and configuring your Docker containers? 🙅‍♂️ Say no more! 🚀 With Ansible, you can automate the creation of Docker containers in just a few simple steps.

But first, what is Ansible? 🤔 Ansible is an open-source automation platform that allows you to automate configuration management, application deployment, and orchestration tasks. It is simple to use and does not require an agent or additional custom security infrastructure, making it an ideal choice for automation. This article will walk you through the process of quickly automating Docker container creation using Ansible.

### Prerequisites

This article assumes that you have 😉:

* Some essential experience with [Docker](https://docs.docker.com/get-started/)
    
* Know what a playbook is and how to create one in [Ansible](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_intro.html)
    
* Are familiar with [Jenkins](https://www.jenkins.io/doc/book/pipeline/getting-started/#snippet-generator) and how to set up a workflow
    

### **Let's dive into a real-world use case to see how Ansible can make Our lives easier. 🌎**

A load balancer is like a 🚨 traffic cop directing people to the right server when they want to use a web app. To ensure the app works for everyone, we have to ensure each server has the correct version of the app and the right dependencies installed. We can use a tool like Ansible to set everything up consistently on all the servers. 💻💾🧑💻💾🧑💻💾🧑

Now, look at code samples to see how we can use Ansible to create Docker containers. 📝

First, we need to install the Docker python module and the ansible Docker module:

```bash
pip install docker
pip install ansible[docker]
```

Next, we can create a [playbook](https://www.redhat.com/en/topics/automation/what-is-an-ansible-playbook#:~:text=Ansible%20Playbooks%20are%20lists%20of,as%20which%20user%20executes%20it.) `<playbook_name>.yml` that uses the `docker_container` Module to create a Docker container. 🐳 Here is an example of how to do this:

```javascript
   - name: Create Docker container 🐳
      hosts: localhost
      tasks:
        - name: Pull image from DockerHub 📥
          docker_image:
            name: ubuntu
            source: pull
        - name: Create Docker container 📦
          docker_container:
            name: ubuntu_container
            image: ubuntu
            state: started
```

This playbook will pull the latest version of the Ubuntu image from DockerHub and create a Docker container based on that image. Finally, we will give the container the name `ubuntu_container` and start it.

You can customize the container by adding additional arguments to the `docker_container` Module. For example, you can specify the command to run when the container is started or mount a volume to the container.

To run the playbook, you can then run the playbook using the ansible-playbook command:

```bash
ansible-playbook <playbook_name>.yml
```

We can integrate this playbook into a Jenkins workflow to simplify the process. This way, we can trigger the playbook to automatically run whenever certain events occur, such as a code change or a build completion.

To integrate this playbook into a Jenkins workflow, you can do the following:

1. Install the Ansible plugin in Jenkins. It will allow you to run Ansible playbooks as part of a Jenkins job.
    
2. Create a new Jenkins pipeline job by defining a pipeline script in a `Jenkinsfile`.
    
3. In the pipeline definition, specify the location of your playbook file and any required variables.
    
    *For example, below is* `JenkinsFile` *defining each stage of the pipeline job :*
    

```javascript
pipeline {
  agent {
    label 'my-ansible-agent'
  }
  stages {
    stage('Create Docker container') {
      steps {
        ansiblePlaybook(
          playbook: 'create_docker_container.yml', 
          extraVars: [
            containerName: 'ubuntu_container',
            imageName: 'ubuntu'
          ]
        )
      }
    }
  }
}
```

1. Run the Jenkins job to execute the playbook.
    

Note that you will need to have the Docker python module and the ansible Docker module installed on the Jenkins agent machine for the playbook to run successfully. You can do this by adding the following steps to your pipeline definition:

```javascript
stage('Install dependencies') {
  steps {
    sh 'pip install docker'
    sh 'pip install ansible[docker]'
  }
}
```

It will install the required dependencies before the playbook runs.

In summary, Ansible is a powerful tool that allows you to automate the creation of Docker containers 🚀 . In addition, you can easily set up and configure multiple Docker containers by creating playbooks 📦 . So next time you create Docker containers, give Ansible a try 💪!

I hope you enjoyed reading this article and found it helpful 🤓, as much as I did writing it 😊! You can find me on [GitHub](https://github.com/nextwebb), [Twitter](https://twitter.com/iam_nextwebb), and [LinkedIn](https://www.linkedin.com/in/peterson-oaikhenah-102645144/) if you have any questions or want to learn more. Don't forget to show your support by giving this article a like, leaving a comment, and sharing it with your friends and followers 😊.

Here are some useful references to check out:

* [**Ansible documentation**](https://docs.ansible.com/)
    
* [**Docker python module**](https://pypi.org/project/docker/)
    
* [**Ansible Docker module**](https://docs.ansible.com/ansible/latest/modules/docker_container_module.html)