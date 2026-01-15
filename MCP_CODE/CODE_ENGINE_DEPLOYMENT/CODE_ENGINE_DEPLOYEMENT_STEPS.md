# 🚀 Deploy FastAPI App on IBM Code Engine from GitHub

This guide outlines how to deploy a FastAPI application to **IBM Code Engine** using source code from a **GitHub repository**. The deployment uses a Dockerfile-based strategy.

---

## 📁 Project Structure

```
Main_Folder/
├── server.py
├── requirements.txt
└── Dockerfile
```

---

## 🧰 Prerequisites

- IBM Cloud account with Code Engine enabled
- GitHub repository with your project
- SSH key configured in GitHub and IBM Cloud
- Registry access (IBM Container Registry or other)

---

## 🛠️ Steps to Deploy

### 1. **Navigate to Code Engine**

- Go to [cloud.ibm.com](https://cloud.ibm.com)
- Choose your account with Code Engine
- Go to **Resource List** > **Containers** > **Code Engine**
- Click on your **Project** or create a new one
- Select **Applications** > **Create**

---

### 2. **Configure Build Source**

Choose “**Build container image from source code**” and click on “**Specify build details**”.<img width="1339" height="845" alt="Screenshot 2026-01-15 at 12 58 28 PM" src="https://github.com/user-attachments/assets/7e22811b-2acb-464c-b424-1d39733896d9" />


#### **Source Tab**

- **Code repo URL**: Paste the GitHub repository URL that contains your application
- **SSH Secret**: Select the SSH key you configured to access the GitHub repository
- **Branch name**: Enter the branch to build from (e.g., `main`)
- **Context directory**: Leave this empty or set it as-is, since all files are in the root folder of the repository

#### **Strategy Tab**

- **Strategy**: Select `Dockerfile`
- **Dockerfile**: Enter `Dockerfile`
- **Timeout (seconds)**: `10m`
- **Build resources**: `M (1 vCPU / 4 GB)`

#### **Output Tab**

- **Registry server**: Select your container registry (e.g., `private.us.icr.io`)
- **Registry secret**: Choose the registry secret (Code Engine managed secret)
- **Namespace**: Select from the available namespaces 
- **Repository (image name)**: Enter `MCP`
- **Tag**: Use `latest` or select an appropriate version tag

Click **Done**.

---

### 3 Resources & Scaling

#### Instance resources
- **CPU and memory**: 1 vCPU / 4 GB  
- **Ephemeral storage**: 4 GB  

#### Autoscaling – Instance scaling range
- **Minimum number of instances**: 1  
- **Maximum number of instances**: 1  


#### Autoscaling – Request concurrency and timing settings
- **Target concurrency**: 100  
- **Max concurrency**: 100  
- **Request timeout (seconds)**: 300  
- **Scale-down delay (seconds)**: 0  



---

### 4. **Domain Mapping**

- **Visibility**: Set to **Public**

---

### 5. **Optional Settings**

- **Environment variables**: Add any key-value pairs required
- **Image start command/args**: Set custom values if needed

---

### 6. **Create the Application**

Click **Create** and wait for the status to show **Running**.

---

## 🔍 Accessing Logs & URL

1. After creation, go to **Applications**
2. Click on your application
3. Navigate to **Builds > Build Runs**
4. Use the **Application URL** to access your app
5. Check logs here if there are errors during deployment

---


## 📬 Contact

For issues, raise a GitHub issue or reach out via IBM Cloud support.
