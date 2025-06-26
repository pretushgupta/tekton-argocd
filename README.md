# tekton-argocd
Explore how we implemented a Kubernetes-native CI/CD pipeline using Tekton and Argo CD

Detailed Step-by-Step Implementation

Step 1: Prepare Repositories

App Repo: Contains Dockerfile, source code, and optional .tekton/ folder.

Manifests Repo: Contains Kubernetes YAMLs.

Step 2: Deploy Tekton in AKS

Install Tekton components: Pipelines, Triggers, Dashboard (optional)

Create the required ClusterRoleBindings and RBAC for Tekton controller

kubectl apply -f https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml
kubectl apply -f https://storage.googleapis.com/tekton-releases/triggers/latest/release.yaml

Step 3: Define Tekton Pipeline

Create the following YAMLs:

Task for git-clone

Task for kaniko image build

Pipeline chaining both

PipelineRun triggered via EventListener

Step 4: Create Triggers

Create TriggerTemplate, TriggerBinding, and EventListener

Expose listener via Ingress or port-forward

Configure webhook in source repo to point to EventListener

Step 5: Push Image to ACR

Set up ServiceAccount with imagePullSecrets and Docker config JSON

Use Kaniko or Buildah for secure image build inside a Pod

Step 6: Update Manifest Repo

Automatically or manually update image tag in Deployment YAML

Example tag: myapp:v1.2.3

Step 7: Configure Argo CD

Install Argo CD in AKS

Connect to the manifest repo via Git

Create an Application pointing to the target namespace and repo path

Set sync policy to automated or manual