# SSH Key Setup and GitHub Configuration for IBM Code Engine

_All commands below are to be run in your terminal (e.g., Terminal on macOS)._  
_Ensure you're in your home directory unless specified otherwise._

---

## 🔐 STEP 1: Generate SSH Key Pair

```bash
ssh-keygen -t rsa -b 4096 -m PEM -f example-jwtRS256.key
```

- You’ll be prompted to enter a passphrase (optional).
- This will create two files:
  - `example-jwtRS256.key` (Private Key)
  - `example-jwtRS256.key.pub` (Public Key)

---

## 📂 STEP 2: Navigate to Home Directory and Verify Files

```bash
cd ~
ls -ltra
```

- Confirm that both `example-jwtRS256.key` and `example-jwtRS256.key.pub` are listed.

---

## 📄 STEP 3: View the Private Key

```bash
cat example-jwtRS256.key
```

- Copy the full contents of this file when prompted during IBM Code Engine configuration.

---

## 📄 STEP 4: View the Public Key

```bash
cat example-jwtRS256.key.pub
```

- Copy the public key to be added to GitHub Enterprise.

---

## ☁️ STEP 5: Configure Secret in IBM Code Engine

1. Visit [IBM Cloud](https://cloud.ibm.com).
2. Choose your account and navigate to **Resource List**.
3. Under **Containers**, select **Code Engine**.
4. Click on **Secrets and configmaps**.
5. Click **Create new** → Select **SSH Secret**.
6. Provide:
   - A name for the secret.
   - The private key from **Step 3** in the **Private Key** field.

---

## 🐙 STEP 6: Add SSH Key to GitHub Enterprise

1. Visit [GitHub Enterprise](https://github.ibm.com/).
2. Click your profile icon (top-right) → **Settings**.
3. In the left menu, click **SSH and GPG keys**.
4. Click **New SSH key** and fill in the form:
   - **Title**: e.g., "Work Laptop SSH"
   - **Key type**: Select **Authentication Key**
   - **Key**: Paste the public key from **Step 4**
5. Click **Add SSH key** to save.

---

✅ You're now ready to authenticate and deploy securely!
