# ⚙️ Harness CI/CD Setup Guide

## 🚀 Introduction

This guide walks you through setting up your environment using Harness for CI/CD, with GitHub and Docker Hub integrations.

> [!NOTE]
> This repo assumes you’re familiar with GitHub, Docker and basic CLI usage.

## 📌 Pre-requisites

Before you use this repository, make sure you have the following:

🔐 Tokens & Credentials - You’ll need API tokens for:

1. Harness
Account Token: Get this from the URL after logging into Harness. HARNESS_ACCOUNT_ID
API Toke: Click your profile icon (bottom left of the UI) → Access Management → API Keys → Generate Token. HARNESS_API_KEY

2. GitHub
Navigate to developer settings → Personal access tokens → Generate new token. GITHUB_TOKEN

3. Docker Hub
Navigate to your profile → Account Settings → Security → Create a Personal Access Token. DOCKER_HUB_TOKEN

## 🧰 Install the Harness CLI

Ensure you have Harness CLI installed. If not in your terminal navigate to where you install packages and run the following in your terminal (macOS example):

```bash
curl -LO https://github.com/harness/harness-cli/releases/download/v0.0.25-Preview/harness-v0.0.25-Preview-darwin-amd64.tar.gz
tar -xvf harness-v0.0.25-Preview-darwin-amd64.tar.gz
export PATH="$(pwd):$PATH"
echo 'export PATH="'$(pwd)':$PATH"' >> ~/.zshrc
source ~/.zshrc
```

> [!NOTE]
> Change .zshrc to .bashrc if you're using a different shell.

Then run harness update for the newest version and check installed correctly.

```bash
harness update
harness --version
```

> [!NOTE]
> You will be prompted [y/N]

## 🔑 Authenticate with Harness

Use the credentials you saved earlier:

```bash
harness login --api-key <HARNESS_API_TOKEN> --account-id <HARNESS_ACCOUNT_ID>
```

## 🔐 Add Secrets to Harness

Use the CLI to store your GitHub and Docker Hub PATs:

```bash
harness secret apply --token <GITHUB_PAT> --secret-name "github_pat"
harness secret apply --token <DOCKER_PAT> --secret-name "docker_pat"
```

The above secrets will be added to your default_project and default_org. If you wish to add them to different projects or orgs, you can use the --project and --org flags.

## 🔗 Add Connectors (GitHub & Docker)

Update the connector YAML files to match your GitHub and Docker details.

```text
github-connector.yaml
docker-connector.yaml
```

GitHub Connector (change repo/username as needed) and apply:

```bash
harness connector --file github-connector.yaml apply
```

> [!NOTE]
> You will be prompted for your GitHub username.
> Example: laurawarren88

Docker Hub Connector (change image/username as needed) and apply:

```bash
harness connector --file docker-connector.yaml apply
```

You will be prompted for your Docker username.
Example: lmwcode

## Set up a Harness Delegate

Open the harness-delegate.yaml file.
Update the delegateName  namespace, resources and limits fields if required.
Apply the file:

```bash
kubectl apply -f harness-delegate.yaml
```

## ☸️ Add Kubernetes Cluster Connector

Open kubernetes-cluster-connector.yaml.
Update the delegateName field if required.
Apply the file:

```bash
harness connector --file kubernetes-cluster-connector.yaml apply
```

## 🏗️ Set Up Your CI Pipeline

Open the cipipeline.yaml file.
Change the repository reference at the end to point to your desired repo.
Apply the pipeline:

```bash
harness pipeline -f cipipeline.yaml apply
```

## 🚀 Run the Pipeline from Harness UI

Go to your pipeline in the Harness UI.
Click the Build and Push stage.
Select Kubernetes
OS: Linux
Kubernetes Cluster Connector: (select the one you configured)
Namespace: your desired namespace
Click Run (top right).

Make sure it’s set to run from the main branch.
Click Run Pipeline.

📝 Notes
Remember to replace placeholder values (<YOUR_...>) with your actual tokens/usernames/repo names.
You can reuse this repo structure across environments with minimal changes.
