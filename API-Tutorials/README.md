# Foundational Concepts: ISC API Authentication

Before running the lab, let's review the key concepts behind SailPoint ISC API authentication.

---

## What is a Personal Access Token (PAT)?
A **Personal Access Token (PAT)** is a secure alternative to using usernames and passwords for API access.  
- **Client ID:** Public identifier for the application  
- **Client Secret:** Confidential key used to obtain an access token  
- PATs are created in the ISC UI under **Admin → Personal Access Tokens**.

**Screenshot Placeholder:**  
![Personal Access Token Creation](./images/pat-creation.png)  
*Create a PAT in the ISC UI (blur your secret before committing screenshots).*

---

## OAuth 2.0 Client Credentials Flow
SailPoint ISC uses the **OAuth 2.0 Client Credentials Flow** for machine-to-machine authentication:

1. **Request Token** → Send `client_id` + `client_secret` to ISC's token endpoint  
2. **Receive Access Token** → ISC returns a time-limited `access_token`  
3. **Use Access Token** → Include it in the `Authorization: Bearer <token>` header for API calls  

**Reference Diagram:**  
![OAuth 2.0 Client Credentials Flow](https://learn.microsoft.com/en-us/entra/identity-platform/media/v2-oauth2-client-creds-grant-flow/diagram.png)  
*Figure 1: OAuth 2.0 Client Credentials Flow ([source](https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth2-client-creds-grant-flow?utm_source=chatgpt.com)).*

---

## SailPoint ISC Authentication Flow
SailPoint provides a visual representation of how PATs lead to access tokens for API calls:

![SailPoint ISC API Authentication Flow](https://developer.sailpoint.com/docs/api/images/auth-flow.png)  
*Figure 2: SailPoint ISC API Authentication Flow ([source](https://developer.sailpoint.com/docs/api/authentication/?utm_source=chatgpt.com)).*

---

## Anatomy of an API Request
Every API call typically includes:  
- **Method:** `GET`, `POST`, `PUT`, or `DELETE`  
- **Headers:** Authentication and content type information  
- **Body:** JSON payload for `POST` or `PUT` requests  
- **Response:** JSON object with data or status  

Example request:
```bash
curl -H "Authorization: Bearer <ACCESS_TOKEN>" \
     "https://<tenant>.api.identitynow.com/v3/public-identities?limit=1"
```

Example response:
```json
{
  "id": "2c91808365bd1f4e0165c1f4e6d72e3d",
  "displayName": "John Doe",
  "email": "john.doe@example.com"
}
```

---

## Common API Status Codes

| Code | Meaning                  | Example Cause                  |
|------|---------------------------|--------------------------------|
| 200  | Success                   | API call completed successfully |
| 400  | Bad Request                | Syntax error in request body    |
| 401  | Unauthorized               | Invalid or expired token        |
| 403  | Forbidden                  | Missing permissions             |
| 404  | Not Found                  | Resource does not exist         |
| 429  | Too Many Requests           | Rate limit exceeded             |
| 500  | Internal Server Error       | Server-side failure             |

---

# Lab 01 – Authentication & First API Call (SailPoint ISC)

Now that we've covered the essentials, let's authenticate to ISC and make our first API call.

---

## Prerequisites
- Admin access to an ISC tenant  
- A valid **Personal Access Token** (Client ID + Secret)  
- Tenant base URL: `https://<your-tenant>.api.identitynow.com`  
- Terminal with `curl` or Postman installed  

---

## Step 1: Discover OAuth Endpoints
Run this command to discover your tenant's OAuth endpoints:
```bash
curl "https://<your-tenant>.api.identitynow.com/oauth/info"
```

**Expected Output:** JSON with `tokenEndpoint`, `issuer`, etc.  

**Screenshot Placeholder:**  
![OAuth Info Endpoint Output](./images/oauth-info-endpoint.png)

---

## Step 2: Request an Access Token
Use your PAT credentials to request an access token:
```bash
curl -X POST "https://<your-tenant>.api.identitynow.com/oauth/token" \
  -d "grant_type=client_credentials" \
  -d "client_id=<CLIENT_ID>" \
  -d "client_secret=<CLIENT_SECRET>"
```

**Expected Output:** JSON with `access_token`, `token_type`, `expires_in`.  

**Screenshot Placeholder:**  
![Access Token JSON Response](./images/token-response.png)

---

## Step 3: Make Your First API Call
Use the access token to call the **Public Identities** endpoint:
```bash
curl -H "Authorization: Bearer <ACCESS_TOKEN>" \
  "https://<your-tenant>.api.identitynow.com/v3/public-identities?limit=1"
```

**Expected Output:** JSON with a single identity object:
```json
{
  "id": "2c91808365bd1f4e0165c1f4e6d72e3d",
  "displayName": "John Doe",
  "email": "john.doe@example.com"
}
```

**Screenshot Placeholders:**  
![Identity API Response](./images/identity-api-response.png)  
![Identity in ISC UI](./images/identity-ui-compare.png)

---

## Validation Checklist
- [ ] Access token successfully retrieved  
- [ ] API call returns valid identity JSON  
- [ ] Identity data matches what appears in ISC UI  

---

## Pitfalls & Tips
- **Rate Limits:** 100 requests per token per 10 seconds → handle `429 Too Many Requests` with backoff logic  
- **Token Expiry:** Refresh or regenerate PATs when they expire or are revoked  
- **Secrets:** Never commit `client_secret` to GitHub  

---

## GitHub Folder Structure
```
/API-Tutorials
  /01-authentication
    README.md
    /images
      pat-creation.png
      oauth-info-endpoint.png
      token-response.png
      identity-api-response.png
      identity-ui-compare.png
```

---

# References
1. Microsoft Entra ID – [OAuth 2.0 Client Credentials Flow](https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth2-client-creds-grant-flow?utm_source=chatgpt.com)  
2. SailPoint Developer Docs – [API Authentication](https://developer.sailpoint.com/docs/api/authentication/?utm_source=chatgpt.com)  
3. SailPoint Developer Docs – [Getting Started with ISC APIs](https://developer.sailpoint.com/docs/api/getting-started/?utm_source=chatgpt.com)  
