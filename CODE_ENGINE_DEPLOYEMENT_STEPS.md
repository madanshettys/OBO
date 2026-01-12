# 🚀 Deploy FastAPI App on IBM Code Engine from GitHub

This guide outlines how to deploy a FastAPI application to **IBM Code Engine** using source code from a **GitHub repository**. The deployment uses a Dockerfile-based strategy.

---

## 📁 Project Structure

```
Main_Folder/
├── app/
│   └── main.py  # Your FastAPI code
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

Choose **"Build container image from source code"**.

#### **Source Tab**

- **Code repo URL**: Paste your GitHub repository URL
- **SSH Secret**: Select the SSH key you configured for GitHub access
- **Branch name**: Enter the branch to build from (e.g., `main`)

#### **Strategy Tab**

- **Build strategy**: Select `Dockerfile`
- **Dockerfile name**: Enter `Dockerfile`
- **Timeout (seconds)**: e.g., `600`
- **Build resources**: Set as needed (e.g., 1 CPU, 2 GB RAM)

#### **Output Tab**

- **Registry server**: Select your container registry
- **Registry secret**: Choose the secret that allows access to your registry
- **Repository name**: Specify the image name (e.g., `my-fastapi-app`)
- **Tag (optional)**: e.g., `latest`

Click **Done**.

---

### 3. **Set Runtime Configuration**

- **CPU and memory**: e.g., 0.5 CPU, 1 GB RAM
- **Ephemeral storage (GB)**: e.g., `1`
- **Min/Max instances**: e.g., 1 min, 3 max
- **Target concurrency**: e.g., `100`
- **Max concurrency**: e.g., `250`
- **Request timeout**: e.g., `300`
- **Scale-down delay**: e.g., `30`

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

## 🐳 Example `Dockerfile`

```Dockerfile
FROM registry.access.redhat.com/ubi8/python-311

WORKDIR /app

COPY ./requirements.txt .
COPY app/ ./app/

USER 0
RUN chown -R 1001:0 /app && chown 1001:0 requirements.txt
USER 1001

RUN pip install --no-cache-dir -r requirements.txt

EXPOSE 8500

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8500"]
```

---

## 🧾 Example `requirements.txt`

```
fastapi
uvicorn
python-dotenv
```

---

## ✅ Result

Once deployed, your FastAPI app will be available at the public URL provided by IBM Code Engine. Logs and builds can be managed directly from the dashboard.

---

## 🧠 Troubleshooting Tips

- Check Build Logs under **Build Runs** for errors
- Validate SSH and registry secrets
- Ensure Dockerfile and branch are correctly referenced
- Keep all files (`Dockerfile`, `requirements.txt`, `app`) in root or adjust COPY paths

---

## 📬 Contact

For issues, raise a GitHub issue or reach out via IBM Cloud support.
