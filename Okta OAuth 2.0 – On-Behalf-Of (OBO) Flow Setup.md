# Okta OAuth 2.0 – On-Behalf-Of (OBO) Flow Setup

This repository documents the complete setup for **OAuth 2.0 SSO / IDP authentication using Okta**, including an **On-Behalf-Of (OBO) token exchange flow**.

This setup is typically used when:
- A **frontend (SPA)** authenticates a user via Okta
- A **backend service** securely calls APIs **on behalf of the authenticated user**

---


## 1. Create OIDC Single-Page Application (Frontend)

This application handles **user login and SSO**.

### Steps

1. Go to **Admin Console → Applications → Applications**
2. Click **Create App Integration**
3. Select:
   - **Sign-in method:** OIDC – OpenID Connect
   - **Application type:** Single-Page Application
4. Click **Next**
5. Configure:
   - **App name:** frontend-spa
   - **Assignments:** Allow everyone in your organization to access
6. Add **Redirect URIs**, for example:
   ```
   http://localhost:3000/login/callback
   https://your-domain.com/login/callback
   ```
7. Click **Save**

### After Creation

- Copy the **Client ID**
- Store it securely

---

## 2. Configure Authorization Server (SPA Access Policy)

Navigate to:

```
Security → API → Authorization Servers → Default (or custom)
```

### Add Access Policy

- Policy name: `SPA Access Policy`
- Grant types:
  - Authorization Code
  - Refresh Token
- Scopes:
  - openid
  - profile
  - email

---

## 3. Create API Service Application (Backend)

Used for **token exchange and backend API access**.

### Steps

1. Go to **Applications → Create App Integration**
2. Select **API Services**
3. App name: `backend-api-service`
4. Save

### After Creation

- Copy **Client ID** and **Client Secret**
- Store securely

---

## 4. Configure Custom Scopes

Create the following scopes:

| Scope | Description |
|------|------------|
| mcp.read | Read access |
| mcp.write | Write access |

---

## 5. Client Credentials Policy

- Policy name: `MCP Client Credentials Policy`
- Grant type: Client Credentials
- Scopes: `mcp.read`, `mcp.write`

---

## 6. On-Behalf-Of (OBO) Flow

1. Frontend authenticates user
2. Backend exchanges token using:
   ```
   grant_type=urn:ietf:params:oauth:grant-type:token-exchange
   ```
3. Backend receives new access token
4. Calls protected APIs

---

## Security Best Practices

- Never expose client secrets in frontend
- Use HTTPS
- Rotate secrets
- Use least privilege scopes

---

## Maintainer

Madan S Shetty
