# Okta OAuth 2.0 – On-Behalf-Of (OBO) Flow Setup

This repository documents the complete setup for **OAuth 2.0 SSO / IDP authentication using Okta**, including an **On-Behalf-Of (OBO) token exchange flow**.

This setup is typically used when:
- A **frontend (SPA)** authenticates a user via Okta
- A **backend service** securely calls APIs **on behalf of the authenticated user**

---

## 1. Sign Up for Okta Free Trial

1. Go to the [Okta Free Trial](https://www.okta.com/free-trial/workforce-identity/).
2. Provide the following details:
   - **First Name**
   - **Last Name**
   - **Work Email**
   - **Country / Region**
   - **Phone Number**
3. Click **Start Free Trial**.
4. You will receive an **activation email**:
   - Activate your account
   - Set a password
   - Complete **device setup** for verification

After setup, you will receive an Okta Admin URL similar to:

```
https://{okta_org}-admin.okta.com/admin/getting-started
```

**Example:**
```
https://trial-8712265-admin.okta.com/admin/getting-started
```

---

## 2. Create OIDC Single-Page Application (Frontend)

This application handles **user login and SSO**.

### Steps

1. Go to **Admin Console → Applications → Applications**
2. Click **Create App Integration**
3. Select:
   - **Sign-in method:** OIDC – OpenID Connect
   - **Application type:** Single-Page Application
4. Click **Next**

### Configure Application Settings

- **App integration name:** `SPA WEB APPLICATION`
- **Grant types:**
  - [x] Authorization Code
  - [x] Refresh Token
- **Sign-in redirect URIs:**
  - Production: `https://yourdomain.com/your-chat-page/` (The URL endpoint where the webchat web application is hosted)
  - Local testing: `http://localhost:3000`
- **Sign-out redirect URIs:**
  - Production: `https://yourdomain.com/your-chat-page/` (The URL endpoint where the webchat web application is hosted)
  - Local testing: `http://localhost:3000`
- **Controlled access:** Allow everyone in your organization
- **Disable:** Enable immediate access with Federation Broker Mode
<img width="1078" height="1790" alt="image" src="https://github.com/user-attachments/assets/878426d8-4970-4564-bf28-f7705dddbf24" />

Click **Save**.

➡️ Copy and securely store the SPA Application **Client ID**.

---

## 3. Assign Users to SPA Application

1. Go to **Applications → Applications → SPA WEB APPLICATION**
2. Open the **Assignments** tab.
3. Click **Assign → Assign to People**.
4. Add users who should access the webchat.

---

## 4. Configure Authorization Server for SPA

1. Go to **Security → API → Authorization Servers**
2. Open the **Default** server
<img width="2074" height="732" alt="image" src="https://github.com/user-attachments/assets/792cfe5e-2dc6-427a-941d-e7ec85cbbf08" />

   
3. Go to **Access Policies** tab → **Add Policy**
   - **Name:** `SPA Access Policy`
   - **Description:** `SPA Access Policy`
   - **Assign to:** `SPA WEB APPLICATION`
   - Click **Create Policy**
<img width="1618" height="1248" alt="image" src="https://github.com/user-attachments/assets/81ec4069-5e36-43b2-9bc3-c509f34f165c" />
   - Click **Create Policy**

##  Create SPA Access Policy Rule

1. In the SPA access policy, click **Add Rule**
   <img width="1105" height="811" alt="Screenshot 2026-01-04 at 1 32 26 AM" src="https://github.com/user-attachments/assets/5234e0f4-c440-4534-a662-5ac002c0f1c2" />

2. Configure:
   - **Rule Name:** `SPA Rule`
   - **Grant type:**
      - [x] Authorization Code
   - **User:** Any user assigned the app
   - **Scopes:**
     - `openid`
     - `profile`
     - `email`
   - **Access token lifetime:** 5 minutes
   - **Refresh token lifetime:** 90 days
<img width="1296" height="1876" alt="image" src="https://github.com/user-attachments/assets/69fc70d2-c6f0-4f35-96bf-b2ce51abf658" />
   - Click **Create Policy**

---

## 6. Create API Service Application (Backend)

This application is used for **token exchange (OBO)** and backend API access.

### Steps

1. Go to **Admin Console → Applications → Applications**
2. Click **Create App Integration**
3. Select:
   - **Sign-in method:** API Services
4. Click **Next**

### Configure Application Settings

- **App integration name:** `API Services Application`
- **Client authentication:** 
   - [x] Client secret
- Click **Save**
- Copy and securely store:
  - **Client ID**
  - **Client Secret**
- Edit **General Settings**
   - In **Grant types**, select **Client Credentials**
      - **Click Advanced**, Select **Token Exchange**
   - Click **save**
   <img width="1360" height="1336" alt="image" src="https://github.com/user-attachments/assets/0bab4521-a923-4b47-9c1a-8dbd2bdfb26d" />


---

## 7. Configure Authorization Server for API Services

1. Go to **Security → API → Authorization Servers**
2. Open the **Default** server  
3. Go to **Access Policies** tab → **Add Policy**
   - **Name:** `API Services Access Policy`
   - **Description:** `API Services Access Policy`
   - **Assign to:** `API Services Application`
     <img width="1360" height="876" alt="image" src="https://github.com/user-attachments/assets/38290aa3-52fb-4dd1-ab41-dbc29cec40fa" />
   - click **Create Policy**

---

## 8. Add Custom Scope

1. Go to the **Scopes** tab
2. Click **Add Scope**
   - **Name:** `mcp.read`
   - **Display phrase:** `MCP Read`


---

## 9. Create API Services Policy Rule

1. In the API Services access policy, click **Add Rule**
2. Configure:
   - **Rule Name:** `API Services Rule`
   - **Grant types:**
     - Client Credentials
     - Authorization Code
     - Device Authorization
     - Token Exchange (Advanced → Non-interactive grants)
   - **Scopes requested:**
     - `mcp.read`
   - **Access token lifetime:** 5 minutes
   - **Refresh token lifetime:** 90 days

---

✅ Your Okta setup is now ready. Please store the following securely:

- SPA Application **Client ID**
- API Services Application **Client ID**
- API Services Application **Client Secret**


