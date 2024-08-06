---
layout: default
title: "🚀 How to Deploy"
---

# 🚀 How to Deploy

This document will walk you through the process of deploying pre-built containers to a single host or a cluster.

>  [!NOTE] Deployment templates
> ###### To make deployment of [OS2-PROJECTNAME] simple, fast and standardized, we recommend using the supplied deployment templates in the `https://github.com/{USERNAME}/{REPOSITORY}.git` deployment repository. 

 &ensp;

## 🖥 Single Host Deployment

The single host deployment strategy provided by [OS2-PROJECTNAME] operates without redundancy. It lacks the capabilities to auto-scale or auto-heal and it is not suited for more complex multi-container applications.

> [!CAUTION] Not Recommended for Production Use
> ###### This strategy is a cost-effective solution for short-term, limited use systems such as simple Proof of Concepts (PoC) or test systems. While this option allows for a reduction in initial investment, it introduces a single point of failure in the deployment. This deployment strategy is NOT recommended in production systems unless a comprehensive risk assessment process has been conducted and the associated risks have been documented and accepted.


### ⬛ Manual deployment
The following steps provide a general guide for manual running a single container application using 
 a runtime that complies with [the OCI standard for container runtimes](https://opencontainers.org/). While the examples use [podman](https://podman.io), you can replace it with any [OCI](https://opencontainers.org/)-compliant runtime tool that fits your specific needs and system configuration.

1. **Download the Container Image.**

   Use the `pull` command to download the container image from the registry. Add a tag if needed. 

      ```r
      $ podman pull <image-name>:<tag>
      ```

2. **Start the Container**

    Use the run command to start the container.
   
      ```r
      $ podman run <image-name>:<tag>.
      ```

3. **Verify Status**

   Check if the container is running using the `ps` command.
      ```r
      $ podman ps
      ```

4. **Stop and Remove**

   To stop the container, use `stop` and `rm` to remove it. 
      ```r
      $ podman stop <container-id>

      $ podman rm <container-id>
      ```

   :books: [Official podman documentation](https://podman.io/docs)

### 🅰️ Managed deployment
You can write deployments to execute the above steps with your preferred deployment tool, whether it is 
[Ansible](https://www.ansible.com/), [Chef](https://www.chef.io/), [Puppet](https://puppet.com/) or [Salt](https://saltproject.io/).

The below example uses `ssh` and `ansible-pull` to pull and run a default `local.yml` ansible playbook from a deployment repository.

###### Ansible playbook example:

`local.yml`

```yaml
- hosts: servers
  vars:
    image_name: 'your_image_name_here'
  tasks:
    - name: Pull an image
      containers.podman.podman_image:
        name: "{{ image_name }}"
        
    - name: Run the container
      containers.podman.podman_container:
        name: my_container
        image: "{{ image_name }}"
        state: started
      
    - name: Verify the container is running
      command: "podman ps -a | grep my_container"
      register: result
      
    - name: Fail if container is not running
      fail:
        msg: "Container is not running"
      when: "'my_container' not in result.stdout"
```

</details>

Store the playbook in a dedicated git repository for your deployment and execute it with `ansible-pull`

```r
$ ssh remote_user@remote_host "ansible-pull -U https://github.com/{USERNAME}/{REPOSITORY}.git
```
   :books: [Official documentation for `ansible-pull`](https://docs.ansible.com/ansible/latest/cli/ansible-pull.html) &ensp; | &ensp;
   :books: [Ansible community documentation - containers.podman](https://docs.ansible.com/ansible/latest/collections/containers/podman/index.html)

 &ensp;

## 🚢 Cluster Deployment

For production environments that require scalability, fault tolerance, enchanced security, observability and high availability it is best practice to deploy [OS2-PROJECTNAME] to a cluster controlled by a container orchestrator with technologies like [Kubernetes](https://kubernetes.io/), [Nomad](https://www.hashicorp.com/products/nomad) or [OpenShift](https://www.openshift.com/) together with [GitOps](https://opengitops.dev/) tools like [ArgoCD](https://argoproj.github.io/argo-cd/), [Flux](https://fluxcd.io/) or [Fleet](ttps://rancher.com/docs/rancher/v2.x/en/deploy-across-clusters/fleet/ 
)

:books: [What is GitOps?](https://about.gitlab.com/topics/gitops/) &ensp; | &ensp;
:books: [An illustrated guide to GitOps](https://www.redhat.com/architect/illustrated-guide-gitops)


### ♾️ GitOps deployment process
Deploy [OS2-PROJECTNAME] to your cluster in 3 steps:

1. **Initialize and Synchronize**: 
   - **Clone the Deployment Templates**: Start by cloning the deployment templates from `https://github.com/{USERNAME}/{REPOSITORY}.git`.
   - **Create a New Branch**: Establish a new branch for your modifications to preserve the integrity of the main branch.

2. **Customize and Collaborate**: 
   - **Customize the Templates**: Tailor the templates to fit your environment and requirements.
   - **Open a Pull Request (PR)**: Submit a PR for your branch, enabling peer review and collaboration.

3. **Merge and Deploy**: 
   - **Review and Approve**: Have your team review and approve the PR.
   - **Merge and Trigger**: Merge the approved PR into the main branch, which triggers the GitOps tool to synchronize and apply the configurations to your cluster.

### :gear: Operations

#### Updating the Application
Explain how to update the application. With GitOps, this is typically done by making a change to the manifests in the Git repository.

#### Rollbacks
Discuss how to rollback to a previous version of the application. With GitOps, this can be done by reverting the commit in the Git repository.

#### Monitoring and Logging
Explain how to monitor the health of the application and how to access the logs. Consider integrating with a centralized logging system.

#### Security
Discuss any security considerations such as network policies, RBAC, and secret management.

#### Troubleshooting
Provide common troubleshooting steps and known issues.