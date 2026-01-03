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
  - Production: `https://yourdomain.com/your-chat-page/`
  - Local: `http://localhost:8080/`
- **Sign-out redirect URIs:**
  - Production: `https://yourdomain.com/your-chat-page/`
  - Local: `http://localhost:8080/`
- **Controlled access:** Allow everyone in your organization
- **Disable:** Enable immediate access with Federation Broker Mode

Click **Save**.

➡️ Copy and securely store the **Client ID**.

---

## 3. Assign Users to SPA Application

Path:
```
Applications → Applications → SPA WEB APPLICATION
```

1. Open the **Assignments** tab.
2. Click **Assign → Assign to People**.
3. Add users who should access the webchat.

---

## 4. Configure Authorization Server for SPA

1. Go to **Security → API → Authorization Servers**
2. Open the **Default** server (or create a custom one)
3. Go to **Access Policies → Add Policy**
   - **Name:** `SPA Access Policies`
   - **Description:** `SPA Access Policies`
   - **Assign to:** `SPA WEB APPLICATION`

---

## 5. Create SPA Access Policy Rule

1. In the SPA access policy, click **Add Rule**
2. Configure:
   - **Rule Name:** `SPA Rule`
   - **Grant type:** Authorization Code
   - **User:** Any user assigned the app
   - **Scopes:**
     - `openid`
     - `profile`
     - `email`
   - **Access token lifetime:** 5 minutes
   - **Refresh token lifetime:** 90 days

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
- **Client authentication:** Client Secret
- Click **Save**
- Copy and securely store:
  - **Client ID**
  - **Client Secret**

---

## 7. Configure Authorization Server for API Services

1. Go to **Security → API → Authorization Servers**
2. Open the **Default** server
3. Go to **Access Policies → Add Policy**
   - **Name:** `API Services Access Policies`
   - **Description:** `API Services Access Policies`
   - **Assign to:** `API Services Application`

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

✅ Your Okta setup is now ready for:
- SPA authentication
- Backend OBO token exchange
- Secure API access
