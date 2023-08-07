**Scenario:**

An enterprise financial organization has a plan to modernize their legacy applications silos and move them to the cloud native applications through 6R Strategy. They want to provision their Infrastructure using GitOps and Kubernetes. 


They are interested in seeing how they can leverage Azure Kubernetes as a Kubernetes managed platform, Kubernetes CD pipeline, GitOps and Argo CD/Flux in their transformation.


As a senior DevOps consultant, you are going to have a presentation and demo for CTO to introduce all above red coloured terminologies.


# Step 1: Introduction
"Modernization and Cloud-Native Transformation using GitOps and Kubernetes." In this session, we will explore how an enterprise financial organization can leverage modern technologies to revamp its legacy applications and embrace cloud-native practices for improved agility, scalability, and efficiency.

# Step 2: Modernization and Cloud Native Applications

Modernization: Modernization refers to the process of updating or transforming existing legacy applications to adapt to current technology trends, enhance performance, and improve user experiences.

Cloud-Native Applications: Cloud-native applications are designed and developed specifically to run on cloud infrastructure and leverage cloud services to achieve benefits like elasticity, resilience, and cost optimization.

  # Benefits:-

Scalability: Cloud-native applications can dynamically scale up or down based on demand, ensuring optimal resource utilization.

Resilience: With built-in fault tolerance and redundancy, cloud-native applications are more resilient to failures and disruptions.

Continuous Deployment: Cloud-native applications embrace DevOps practices, enabling continuous deployment and quicker feature rollouts.

  # Challenges:-

Complexity: Building cloud-native applications requires a thorough understanding of cloud technologies and distributed systems, which can be challenging.

Microservices Overhead: Adopting microservices architecture, although beneficial, can introduce management overhead for a larger number of services.

# Step 3: Microservices

Microservices: Microservices is an architectural style where an application is broken down into smaller, independent services that can be developed, deployed, and scaled independently.

  # Benefits:-

Independent Development: Each microservice can be developed and maintained by separate teams, promoting faster development cycles.

Fault Isolation: If one microservice fails, it doesn't impact the entire application, ensuring fault isolation.

Scalability: Only the necessary microservices can be scaled based on demand, reducing resource wastage.

  # Challenges:-

Distributed Complexity: The distributed nature of microservices can lead to challenges in managing communication and data consistency between services.

Operational Overhead: Managing multiple microservices requires robust monitoring, logging, and governance practices.

# Step 4: 6R Strategy

The 6R Strategy provides a framework for application migration to the cloud:

Rehost: Lift and shift existing applications to the cloud without making any major changes.

Replatform: Move applications to a cloud-native platform to take advantage of cloud features.

Repurchase: Replace existing applications with cloud-based software-as-a-service (SaaS) alternatives.

Refactor: Rearchitect applications to leverage cloud-native capabilities fully.

Retire: Decommission applications that are no longer required.

Retain: Keep applications on-premises if they cannot be migrated to the cloud.


# Step 5: DevOps vs. GitOps

DevOps: DevOps is a set of practices that aim to integrate development (Dev) and IT operations (Ops) to streamline the application lifecycle, from development to deployment and monitoring.
GitOps: GitOps is an extension of DevOps that uses Git as the single source of truth for infrastructure and application configurations.

  # Key Differences:
  DevOps:

  Centralized Control: DevOps relies on centralized configuration management and manual deployments.

  Procedural Approach: Changes are applied to the infrastructure through scripts and manual interventions.

  GitOps:

  Declarative Approach: GitOps uses declarative configuration stored in Git repositories, defining the desired state of the infrastructure.

  Automation: Changes are automatically applied to the infrastructure based on Git commits, ensuring consistency and version control.

# Step 6: GitOps and Argo CD
GitOps: GitOps leverages Git repositories as the source of truth for declarative configurations of infrastructure and applications.

Argo CD: Argo CD is a popular GitOps tool that automates the deployment of applications to Kubernetes clusters based on changes in Git repositories.

  # GitOps with GitHub and Flux v2
  ![gitops-githubfluxv2-detailed-flow](https://github.com/JayshreePaladiya02/arc-cicd-demo-src/assets/137833004/11904697-4c58-4b0d-81b7-0f0f6b1d17fe)

The diagram above describes in details a CI/CD process built with GitHub and Flux v2

- The manifests repository shows what happens with the applications in the K8s clusters. However, it only shows the desired state and very often the current state. But things can go wrong, labels can be misplaced, and the image can be unavailable, even though the manifests were successfully applied.
- To build a multistage CD workflow, there must be a backward connection from Flux v2 to GitHub that reports the deployment state/result. This backward connection allows the CD workflow to proceed with testing, notifying, deploying to the next environment, etc. There should be a guidance implementation of such a backward connection so that GitHub agents are not blocked waiting for Flux v2 to finish the deployment.
- To approve deployment to an environment, an approver should use two systems: GitHub/Flux v2 to see what has actually changed and GitHub to approve/reject, which is confusing and inconvenient. The goal is to integrate GitHub with Flux v2 to have a single point to perform at least basic deployment activities so that we can see/control the entire CD process from GitHub.

# Step 7: Kubernetes CI/CD Pipeline

Kubernetes CI/CD Pipeline: A Kubernetes CI/CD pipeline automates the process of building, testing, and deploying containerized applications to Kubernetes clusters.

  # Stages
  Code Integration: Developers commit code changes to the repository.

  Containerization: The code is built into container images using Docker or other containerization tools.

  Testing: Automated tests, such as unit tests and integration tests, are performed on the container images.

  Deployment: The tested container images are deployed to Kubernetes clusters.

# Step 8: Azure Compute Services for Microservices

Azure Compute Services are the core set of cloud computing services that allow you to deploy and manage workloads on Microsoft Azure. These services provide the infrastructure, tools, and platforms for computing and storage needs. Compute services are the building blocks of any cloud solution, providing the underlying technology that enables your applications and workloads to run in the cloud.

Azure Kubernetes Service (AKS): AKS is a managed Kubernetes service that simplifies the deployment and management of containerized applications.

Azure Functions: Azure Functions is a serverless computing service that enables event-driven microservices development.

Azure App Service: Azure App Service is a fully managed platform for building, deploying, and scaling web applications and APIs.

Here are a few of the more well-known services
  - Azure Virtual Machines
  - Azure Container Instances
  - Networking
  - Database services

# Step 9: Design Solution and Workflow Diagram
Modern Kubernetes deployments contain multiple applications, clusters, and environments. With GitOps, you can manage these complex setups more easily, tracking the desired state of the Kubernetes environments declaratively with Git. Using common Git tooling to declare cluster state, you can increase accountability, facilitate fault investigation, and enable automation to manage environments.

This conceptual overview explains GitOps as a reality in the full application change lifecycle using Azure Arc, Azure Repos, and Azure Pipelines. There is also an example of a single application change to GitOps-controlled Kubernetes environments.

  # Architecture:
  Consider an application deployed to one or more Kubernetes environments.

  ![gitops-flux2-ci-cd-arch](https://github.com/JayshreePaladiya02/arc-cicd-demo-src/assets/137833004/f5252e71-e4ec-4d57-8da6-e03a39fde311)

  # Application repository
  The application repository contains the application code that developers work on during their inner loop. The application’s deployment templates live in this repository in a generic form, like Helm or Kustomize. Environment-specific values aren't stored. Changes to this repo invoke a PR or CI pipeline that starts the deployment process.

  # Container registry
  The container registry holds all the first and third party images used in the Kubernetes environments. Tag first party application images with human readable tags and the Git commit used to build the image. Cache third-party images for security, speed, and resilience. Set a plan for timely testing and integration of security updates. For more information, see the ACR Consume and maintain public content guide for an example.

  # PR pipeline
  Pull requests to the application repository are gated on a successful run of the PR pipeline. This pipeline runs the basic quality gates, such as linting and unit tests on the application code. The pipeline tests the application and lints Dockerfiles and Helm templates used for deployment to a Kubernetes environment. Docker images should be built and tested, but not pushed. Keep the pipeline duration relatively short to allow for rapid iteration.

  # CI pipeline
  The application CI pipeline runs all the PR pipeline steps, expanding the testing and deployment checks. The pipeline can either be run for each commit to main or run at a regular cadence with a group of commits.

  At this stage, application tests which are too consuming to perform in the PR pipeline can be performing, including:
  - Pushing images to container registry
  - Image building, linting, and testing
  - Template generation of raw yamls
  - By the end of the CI build, artifacts are generated which can be used by the CD step to consume in preparation for deployment.

  # Flux
  Flux is an agent that runs in each cluster and is responsible for maintaining the desired state. The agent polls the GitOps repository at a user-defined interval and reconciles the cluster state with the state declared in the git repository.


  # CD pipeline
The CD pipeline is automatically triggered by successful CI builds. In this pipeline environment, environment-specific values are substituted into the previously published templates, and a new pull request is raised against the GitOps repository with these values. This pull request contains the proposed changes to the desired state of one or more Kubernetes clusters. Cluster administrators review the pull request and approve the merge to the GitOps repository. The pipeline waits for the pull request to merge, after which Flux syncs and applies the state changes.

  # GitOps repository
The GitOps repository represents the current desired state of all environments across clusters. Any change to this repository is picked up by the Flux service in each cluster and deployed. Changes to the desired state of the clusters are presented as pull requests, which are then reviewed, and finally merged upon approval of the changes. These pull requests contain changes to deployment templates and the resulting rendered Kubernetes manifests. Low-level rendered manifests allow more careful inspection of changes typically unseen at the template-level.

  # GitOps connector
GitOps Connector creates a connection between the Flux agent and the GitOps Repository/CD pipeline. While applying changes to the cluster, Flux notifies the GitOps connector of every phase change and health check performed. This component serves as an adapter. It "knows" how to communicate to a Git repository and it updates the Git commit status so the synchronization progress is visible in the GitOps repository. When the deployment finishes (whether it succeeds or fails), the connector notifies the CD pipeline to continue so the pipeline can perform post-deployment activities such as integration testing.

  # Kubernetes clusters
At least one Azure Arc-enabled Kubernetes or Azure Kubernetes Service (AKS) cluster serves the different environments needed by the application. For example, a single cluster can serve both a dev and QA environment through different namespaces. A second cluster can provide easier separation of environments and more fine-grained control.

   **Example workflow**
  As an application developer, Alice:

  - Writes application code.
  - Determines how to run the application in a Docker container.
  - Defines the templates that run the container and dependent services in a Kubernetes cluster.

# Step 10: Proof of Concept (POC)
# CI/CD using GitOps with Azure Arc-enabled Kubernetes Cluster

This repo contains the sample code for the Azure Arc GitOps CI/CD tutorial.

The GitOps repo can be found at <https://github.com/Azure/arc-cicd-demo-gitops>

Please see the tutorial at <https://docs.microsoft.com/azure/azure-arc/kubernetes/tutorial-gitops-ci-cd>
