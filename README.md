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

Run the following in your terminal (macOS example):

```bash
curl -LO https://github.com/harness/harness-cli/releases/download/v0.0.25-Preview/harness-v0.0.25-Preview-darwin-amd64.tar.gz
tar -xvf harness-v0.0.25-Preview-darwin-amd64.tar.gz
export PATH="$(pwd):$PATH"
echo 'export PATH="'$(pwd)':$PATH"' >> ~/.zshrc
source ~/.zshrc
harness --version
```

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

## 🔗 Add Connectors (GitHub & Docker)

Update the connector YAML files to match your GitHub and Docker details.

```text
github-connector.yaml
docker-connector.yaml
```

GitHub Connector (change repo/username as needed) and apply:

```bash
harness connector -f harness/github-connector.yaml apply
```

> [!NOTE]
> You will be prompted for your GitHub username.
> Example: laurawarren88

Docker Hub Connector (change image/username as needed) and apply:

```bash
harness connector -f harness/docker-connector.yaml apply
```

You will be prompted for your Docker username.
Example: lmwcode

## ☸️ Add Kubernetes Cluster Connector

Open kubernetes-cluster-connector.yaml.
Update the delegateName field if required.
Apply the file:

```bash
harness connector -f harness/kubernetes-cluster-connector.yaml apply
```

## 🏗️ Set Up Your CI Pipeline

Open the cipipeline.yaml file.
Change the repository reference at the end to point to your desired repo.
Apply the pipeline:

```bash
harness pipeline -f harness/cipipeline.yaml apply
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
