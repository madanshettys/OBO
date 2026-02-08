# 🚀 Deploy FastAPI App on IBM Code Engine from GitHub

This guide outlines how to deploy a FastAPI application to **IBM Code Engine** using source code from a **GitHub repository**. The deployment uses a Dockerfile-based strategy.

---

## 🧰 Prerequisites

- IBM Cloud account with a Code Engine instance
- Access to GitHub (IBM GitHub or GitHub.com)
- SSH key configured in GitHub and IBM Cloud  
  👉 [SSH Key Setup to Connect GitHub to IBM Code Engine](./ssh-key-setup.md)
- Container registry access (IBM Container Registry or other)

---

## 📦 Prepare the GitHub Repository

Clone the source repository using the command below:

```bash
git clone https://github.com/IBM/oic-i-agentic-ai-tutorials.git
```

After cloning, navigate to the **`i-oic-rbac-using-obo`** directory. All materials related to this use case are available there.

Inside the **`mcp_server_code`** folder, you will find the following files:
- `server.py`
- `requirements.txt`
- `Dockerfile`

Next, create a new GitHub repository using:  
https://github.ibm.com/new  

While creating the repository:
- Set the **repository name** to **`mcp_server_code`**
- Upload the following files as the repository contents:
  - `server.py`
  - `requirements.txt`
  - `Dockerfile`

This repository will later be used by **IBM Cloud Code Engine** to pull the source code and deploy the MCP server.

---

## 📁 Project Structure

```
mcp_server_code/
├── server.py
├── requirements.txt
└── Dockerfile
```

---

## 🛠️ Steps to Deploy

### 1. Navigate to Code Engine

- Go to https://cloud.ibm.com
- Navigate to **Resource List** → **Containers** → **Code Engine**
- Open or create a **Project**
- Select **Applications** → **Create**

---

### 2. Configure Build Source

Choose **Build container image from source code** and click **Specify build details**.

#### Source Tab
- **Code repo URL**: GitHub repository URL (`mcp_server_code`)
- **SSH Secret**: `github-code-engine-ssh`
- **Branch name**: `main`
- **Context directory**: Leave empty

#### Strategy Tab
- **Strategy**: Dockerfile
- **Dockerfile**: `Dockerfile`
- **Timeout**: `10m`
- **Build resources**: `M (1 vCPU / 4 GB)`

#### Output Tab
- **Repository (image name)**: `MCP`
- **Tag**: `latest`

Click **Done**.

---

### 3. Resources & Scaling

- CPU / Memory: **1 vCPU / 4 GB**
- Min / Max instances: **1**
- Request timeout: **300 seconds**

---

### 4. Domain Mapping

- **Visibility**: Public

---

### 5. Create the Application

Click **Create**, wait for **Running** state, then copy the application URL and append `/mcp`.

---

## 🔐 Store MCP Server Endpoint

Store the final URL securely as:

```
MCP_SERVER_URL
```
