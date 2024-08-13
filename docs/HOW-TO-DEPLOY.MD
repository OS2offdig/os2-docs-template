---
layout: default
title: "🚀 How to Deploy"
---

# 🚀 How to Deploy

## Introduction

Welcome to the deployment guide for [OS2-PROJECTNAME]. This document will walk you through the process of deploying pre-built containers, whether you're setting up a development environment or deploying to a production cluster.

## 💻 Development
To set up a containerized development and test environment on a local computer or in a hosted developer enviroment like GitHub codespaces, you can use a simple `docker run` or `podman run` command to pull the containerimage and run the container.

#### Docker example
```shell
docker run -d -p {CONTAINERPORT:HOSTPORT} --name {CONTAINER_NAME} {IMAGE_NAME}:{TAG}
```
[Official documentation for `docker run`](https://docs.docker.com/reference/cli/docker/container/run/)

#### Podman example
```shell
podman run -d -p {HOSTPORT}:{CONTAINERPORT} --name {CONTAINER_NAME} {IMAGE_NAME}:{TAG}
```
[Official documentation for `podman run`](https://docs.podman.io/en/latest/markdown/podman-run.1.html)



## 🚢 Production
To make deployment of [OS2-PROJECTNAME] simple, fast and standardized, we recommend using the supplied deployment templates in the `https://github.com/{USERNAME}/{REPOSITORY}.git` deployment repository. 

### 🖥 Deployment on a single host
For less demanding production environments on a single VM or bare metal host, you can use tools like Ansible, Chef, or Puppet to deploy the containerimage.

### Examples
Below is examples that can be run either manually or automated with your Configuration Managment tool of choice. The examples expects the correct enviroment variables to exist and acces to the repository ressources.

### Ansible

Example using `ssh` and `ansible-pull` that pulls and runs the default `local.yml` ansible playbook from the `https://github.com/{USERNAME}/{REPOSITORY}.git` repository.

```shell
ssh remote_user@remote_host "ansible-pull -U https://github.com/{USERNAME}/{REPOSITORY}.git
```
🔗 [Official documentation for `ansible-pull`](https://docs.ansible.com/ansible/latest/cli/ansible-pull.html)

---

### 🌐 Cluster Deployment

For more demanding production environments that require scalability, fault tolerance, enchanced security, observability and high availability it is best practice to deploy [OS2-PROJECTNAME] to a cluster controlled by a container orchestrator with technologies like like [Kubernetes](https://kubernetes.io/), [Nomad](https://www.hashicorp.com/products/nomad) or [OpenShift](https://www.openshift.com/) together with [GitOps](https://opengitops.dev/) tools like [ArgoCD](https://argoproj.github.io/argo-cd/), [Flux](https://fluxcd.io/) or [Fleet](ttps://rancher.com/docs/rancher/v2.x/en/deploy-across-clusters/fleet/ 
) 

The deployment templates provided in the `https://github.com/{USERNAME}/{REPOSITORY}.git` deployment repository are designed for integration into a version-controlled [GitOps](https://opengitops.dev/) workflow. These templates are compatible with your chosen [GitOps](https://opengitops.dev/) tools and the container orchestrator of your choice.

🔗 [What is GitOps?](https://about.gitlab.com/topics/gitops/) &ensp; | &ensp;
🔗 [An illustrated guide to GitOps](https://www.redhat.com/architect/illustrated-guide-gitops)


## 🔄 GitOps Process
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

## ☁️ Managed GitOps Hosting
If preferred, select a managed GitOps hosting provider that aligns with your project's needs and goals.