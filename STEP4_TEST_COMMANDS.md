# PinDoc – Step 4 Testing Commands

This document contains all commands used to test **Step-4 (Document Upload API)**.

Server assumed running at:

```
http://localhost:3001
```

Authentication uses **AWS Cognito JWT Bearer Token**.

---

# 1. Get Cognito Token

Login through the Cognito hosted UI and copy the **ID Token** from the browser URL.

Example:

```
https://app/#id_token=TOKEN_HERE
```

Save it in a variable.

```bash
export TOKEN="PASTE_COGNITO_ID_TOKEN_HERE"
```

---

# 2. Health Check (API Running)

```bash
curl http://localhost:3001/health
```

Expected response:

```json
{
  "status": "ok"
}
```

---

# 3. Test Authentication Endpoint

Verify token and user mapping.

```bash
curl http://localhost:3001/me \
  -H "Authorization: Bearer $TOKEN"
```

Expected response:

```json
{
  "userId": "...",
  "cognitoSub": "...",
  "email": "..."
}
```

---

# 4. Upload Document

Upload a PDF file to PinDoc.

```bash
curl -X POST http://localhost:3001/documents/upload \
  -H "Authorization: Bearer $TOKEN" \
  -F "file=@/home/swapo/projects/pindoc/apps/api/test.pdf"
```

Expected response:

```json
{
  "id": "document_id",
  "originalName": "test.pdf",
  "mimeType": "application/pdf",
  "sizeBytes": 13264,
  "createdAt": "2026-03-12T19:32:02.074Z"
}
```

---

# 5. List Uploaded Documents

Check all uploaded documents.

```bash
curl http://localhost:3001/documents \
  -H "Authorization: Bearer $TOKEN"
```

Expected response:

```json
[
  {
    "id": "...",
    "originalName": "test.pdf",
    "mimeType": "application/pdf",
    "sizeBytes": 13264,
    "createdAt": "..."
  }
]
```

---

# 6. Download Document

Download the uploaded document.

```bash
curl -X GET http://localhost:3001/documents/DOCUMENT_ID/download \
  -H "Authorization: Bearer $TOKEN" \
  -o downloaded.pdf
```

Expected result:

```
downloaded.pdf saved locally
```

---

# 7. Delete Document (Optional)

```bash
curl -X DELETE http://localhost:3001/documents/DOCUMENT_ID \
  -H "Authorization: Bearer $TOKEN"
```

Expected response:

```json
{
  "success": true
}
```

---

# Notes

- Authentication is handled using **AWS Cognito JWT**
- All requests must include:

```
Authorization: Bearer TOKEN
```

- Supported upload types (MVP)

```
PDF
```

- File uploads are stored outside the API service (secure storage layer).

---

# Step-4 Completion Criteria

Step-4 is complete when:

- JWT authentication works
- `/me` endpoint returns user
- file upload works
- document stored
- metadata saved in database
- documents can be listed
- documents can be downloaded

```
Step-4 Status: COMPLETE
```
