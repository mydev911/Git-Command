# PinDoc – Step 3: AWS Cognito Authentication Testing

This document describes how to test the **PinDoc authentication system** using AWS Cognito and the backend API.

---

# 1. Start the Backend Server

Navigate to the PinDoc project root and start the development server.

```bash
cd ~/projects/pindoc
pnpm dev
```

Expected output:

```
Database connection established
Pindoc API listening on http://localhost:3001
```

This confirms:

- Backend server running
- Fastify initialized
- Database connected
- API available on port **3001**

---

# 2. Test Health Endpoint

Open a **second terminal window** and run:

```bash
curl http://localhost:3001/health
```

Expected response:

```json
{"status":"ok"}
```

This verifies:

- Backend server is running
- API routing works
- Server responding correctly

---

# 3. Test Protected Endpoint (Without Token)

Run:

```bash
curl http://localhost:3001/me
```

Expected response:

```json
{"error":"Unauthorized"}
```

This confirms:

- Authentication middleware is active
- Protected endpoints require authentication
- JWT token must be provided

---

# 4. Login with AWS Cognito

Open the Cognito hosted login page in a browser.

```
https://us-east-2bnamb8ggb.auth.us-east-2.amazoncognito.com/login?client_id=6ci6vn85qd2s0537r9nv5tk1vq&response_type=token&scope=openid+email+profile&redirect_uri=http://localhost:3000
```

After successful login Cognito redirects to:

```
http://localhost:3000/#id_token=eyJ...LONGTOKEN...&access_token=...
```

---

# 5. Copy the ID Token

From the redirected URL copy the **ID Token only**.

Example URL:

```
http://localhost:3000/#id_token=eyJabc123XYZ&access_token=ABC&expires_in=3600
```

Copy ONLY:

```
eyJabc123XYZ
```

Do NOT copy:

```
&access_token
&expires_in
```

---

# 6. Test Authenticated Endpoint

Run the following command with the ID token:

```bash
curl http://localhost:3001/me \
-H "Authorization: Bearer YOUR_ID_TOKEN"
```

Example:

```bash
curl http://localhost:3001/me \
-H "Authorization: Bearer eyJraWQiOiJ..."
```

---

# 7. Expected Successful Response

```json
{
  "userId": "clxxxx",
  "cognitoSub": "xxxxxxxx",
  "email": "user@example.com"
}
```

This confirms:

- Cognito token validated
- JWT signature verified
- User identity extracted
- Authentication middleware working

---

# 8. Authentication Flow

```
User
 ↓
Cognito Hosted Login
 ↓
Cognito returns ID Token
 ↓
Token sent to PinDoc API
 ↓
JWT verified by backend
 ↓
User returned from database
```

---

# 9. Components Verified

Step-3 confirms that the following systems work correctly:

- AWS Cognito Hosted Login
- OAuth authentication flow
- JWT token validation
- Fastify authentication plugin
- Prisma database connection
- Protected API routes

---

# 10. Step-3 Status

Authentication setup is **fully operational**.

Verified features:

- Cognito login
- Token generation
- Backend JWT verification
- Protected endpoint access

---

# Next Development Step

Upcoming backend work:

```
Step 4
Document Upload API
S3 Integration
PDF Processing
Document Storage
```

This will allow users to upload and process documents inside the PinDoc platform.

---
