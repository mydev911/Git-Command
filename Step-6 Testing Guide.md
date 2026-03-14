PinDoc Step-6 Testing Guide
Purpose
This document is the full testing checklist for Step-6 of PinDoc.
Step-6 is the point where the web frontend, Cognito login, and documents API must work together.
This guide is written so the full Step-6 flow can be tested end-to-end and repeated later.
---
What Step-6 is supposed to verify
Step-6 should prove that all of these work together:
Frontend runs on `http://localhost:3000`
API runs on `http://localhost:3001`
Database is running and API can connect to it
Cognito Hosted UI login works
`/auth/callback` receives the code from Cognito
Frontend exchanges the code for tokens
`pindoc_id_token` is stored in browser localStorage
Dashboard loads after login
Frontend can call protected API routes using `Authorization: Bearer <token>`
`GET /documents` works for the logged-in user
`POST /documents/upload` works for PDF upload
Uploaded document appears in document list
---
Required local services
You need these running:
Docker Desktop
PostgreSQL container via Docker Compose
API server
Web server
---
Terminal setup
Use three terminals.
Terminal 1 — Database
Run from project root:
```bash
cd ~/projects/pindoc
docker compose up -d
```
Check containers:
```bash
docker compose ps
```
Expected:
database container is Up
---
Terminal 2 — API
Run:
```bash
cd ~/projects/pindoc/apps/api
pnpm dev
```
Expected log should include something like:
```text
Database connection established
PinDoc API listening on http://localhost:3001
```
Health check:
```bash
curl http://localhost:3001/health
```
Expected:
```json
{"status":"ok"}
```
---
Terminal 3 — Web
Run:
```bash
cd ~/projects/pindoc/apps/web
pnpm dev
```
Expected output includes:
```text
Local: http://localhost:3000
```
---
Required env checks
apps/web/.env.local
Verify:
```env
NEXT_PUBLIC_API_BASE_URL=http://localhost:3001
NEXT_PUBLIC_COGNITO_DOMAIN=https://YOUR_COGNITO_DOMAIN.auth.us-east-2.amazoncognito.com
NEXT_PUBLIC_COGNITO_CLIENT_ID=YOUR_NEW_PUBLIC_CLIENT_ID
NEXT_PUBLIC_COGNITO_REDIRECT_URI=http://localhost:3000/auth/callback
NEXT_PUBLIC_COGNITO_LOGOUT_URI=http://localhost:3000
NEXT_PUBLIC_COGNITO_SCOPES=openid email profile
```
apps/api/.env
Verify:
```env
DATABASE_URL=postgresql://postgres:postgres@localhost:5432/pindoc
UPLOAD_DIR=./uploads
COGNITO_REGION=us-east-2
COGNITO_USER_POOL_ID=YOUR_USER_POOL_ID
COGNITO_APP_CLIENT_ID=YOUR_NEW_PUBLIC_CLIENT_ID
COGNITO_ISSUER=https://cognito-idp.us-east-2.amazonaws.com/YOUR_USER_POOL_ID
```
Important
The following two values must match the same new public app client:
`NEXT_PUBLIC_COGNITO_CLIENT_ID`
`COGNITO_APP_CLIENT_ID`
If they do not match, login will succeed but `/documents` will return `401 Unauthorized`.
---
Cognito configuration checklist
Inside AWS Cognito app client settings, verify:
Allowed callback URL
```text
http://localhost:3000/auth/callback
```
Allowed sign-out URL
```text
http://localhost:3000
```
Identity provider
Cognito user pool
OAuth 2.0 grant type
Authorization code grant
Scopes
openid
email
profile
App client type
public client / no client secret
Do not use
implicit grant
client secret for this browser flow
---
Pre-test browser cleanup
Before each clean login test, clear browser auth storage.
Open browser console on `http://localhost:3000` and run:
```javascript
localStorage.clear()
sessionStorage.clear()
location.href = "/"
```
This prevents old or expired tokens from interfering with the test.
---
Step-6 manual test flow
Test 1 — Landing page loads
Open:
```text
http://localhost:3000
```
Expected:
page shows PinDoc
page shows Sign in button
Pass if:
page loads without error
---
Test 2 — Sign in redirects to Cognito
Click:
```text
Sign in
```
Expected:
browser redirects to Cognito Hosted UI login page
Pass if:
Cognito login page appears
Fail if:
redirect mismatch
invalid scope
invalid client
no authorization code received
---
Test 3 — Cognito returns to callback page
After successful login, expected redirect:
```text
http://localhost:3000/auth/callback?code=...
```
Pass if:
callback page receives `code`
no callback error is shown
---
Test 4 — Code exchange succeeds
The callback page should exchange the authorization code for tokens.
Expected:
no token exchange error
browser goes to `/dashboard`
If needed, inspect network requests and look for:
```text
/oauth2/token
```
Expected:
status `200`
Pass if:
no "Token exchange failed" error
redirect to dashboard happens
---
Test 5 — Token is stored in localStorage
After login completes and dashboard loads, open browser console and run:
```javascript
localStorage.getItem("pindoc_id_token")
```
Expected:
a long JWT token string
not `null`
Pass if:
token exists
Fail if:
result is `null`
---
Test 6 — Dashboard loads
Expected route:
```text
http://localhost:3000/dashboard
```
Expected UI:
heading `Dashboard`
link `Upload PDF`
button `Sign out`
Pass if:
dashboard stays on screen
no immediate redirect back to `/`
Fail if:
dashboard flashes briefly and returns to sign-in page
That usually means:
`/documents` returned `401`
frontend cleared token and redirected to `/`
---
Test 7 — API accepts the fresh token
Get the fresh token from browser console:
```javascript
localStorage.getItem("pindoc_id_token")
```
Then in terminal test the protected route:
```bash
curl -i http://localhost:3001/documents -H "Authorization: Bearer PASTE_FRESH_TOKEN_HERE"
```
Expected:
`HTTP/1.1 200 OK`
response body likely `[]`
Pass if:
`200 OK`
Fail if:
`401 Unauthorized`
Common reasons for failure:
expired token
API using old Cognito app client ID
mismatched issuer / user pool ID
wrong token copied
---
Test 8 — Same protected route in Postman
Create request:
```text
GET http://localhost:3001/documents
```
In Postman:
Authorization tab: No Auth
Headers tab:
`Authorization: Bearer YOUR_TOKEN`
Expected:
`200 OK`
`[]` if no documents yet
Pass if:
works in Postman
This is important because it proves backend auth works independently of frontend rendering.
---
Test 9 — Browser dashboard document fetch
When dashboard loads, it should call:
```text
GET http://localhost:3001/documents
```
Expected:
request succeeds
empty state shows: `No documents yet.` if nothing uploaded
Pass if:
dashboard stays logged in
no red error message
Fail if:
browser shows `Failed to fetch`
browser redirects to sign-in
API logs `401`
---
Test 10 — Upload PDF from browser
Open:
```text
http://localhost:3000/upload
```
Choose a PDF and submit.
Expected:
upload succeeds
no unauthorized error
response returns document metadata
app redirects or allows navigation back to dashboard
Pass if:
PDF upload completes successfully
Fail if:
`401`
`Only PDF files are allowed`
`No file uploaded`
connection refused
---
Test 11 — Upload PDF in Postman
Request:
```text
POST http://localhost:3001/documents/upload
```
Postman setup:
Authorization tab: No Auth
Headers tab:
`Authorization: Bearer YOUR_TOKEN`
Body:
choose `form-data`
add field:
```text
Key: file
Type: File
Value: choose a PDF
```
Expected:
`200 OK`
JSON response with document metadata
Pass if:
upload works in Postman
This proves backend upload works even if browser upload UI still has issues.
---
Test 12 — Uploaded document appears in list
After upload, test again:
```text
GET http://localhost:3001/documents
```
Expected:
uploaded PDF appears in response
Also refresh browser dashboard.
Expected:
document appears in dashboard list with:
`originalName`
`sizeBytes`
`createdAt`
Pass if:
uploaded file appears in list
---
Postman request set for Step-6
Request A — Health
```text
GET http://localhost:3001/health
```
Expected:
```json
{"status":"ok"}
```
Request B — List documents
```text
GET http://localhost:3001/documents
```
Headers:
```text
Authorization: Bearer YOUR_TOKEN
```
Expected:
```json
[]
```
or an array of documents.
Request C — Upload document
```text
POST http://localhost:3001/documents/upload
```
Headers:
```text
Authorization: Bearer YOUR_TOKEN
```
Body:
`form-data`
`file` = PDF file
Expected:
uploaded document JSON
---
Common Step-6 failures and exact meaning
1. `redirect_mismatch`
Meaning:
Cognito callback URL does not match frontend callback URL
Fix:
add `http://localhost:3000/auth/callback` to allowed callback URLs
---
2. `invalid_scope`
Meaning:
frontend scope string is wrong
Fix:
Use:
```env
NEXT_PUBLIC_COGNITO_SCOPES=openid email profile
```
Not:
```env
NEXT_PUBLIC_COGNITO_SCOPES=openid+email+profile
```
---
3. `invalid_client_secret`
Meaning:
Cognito app client has a secret but frontend uses PKCE
Fix:
use a public app client
no client secret
---
4. `No authorization code received`
Meaning:
frontend expects code flow but Cognito is configured for a different flow
Fix:
use Authorization code grant
not implicit grant
---
5. `Failed to fetch`
Meaning:
Usually one of:
API is not running
database is down so API crashed
wrong API base URL
browser cannot reach API
temporary frontend fetch failure
---
6. `401 Unauthorized` right after login
Meaning:
Frontend logged in, but first protected API request failed.
Most common reasons:
expired token
API still using old Cognito app client ID
issuer/user pool mismatch
token invalid
---
7. Token exists but browser returns to sign-in page
Meaning:
Frontend is intentionally doing this on `401`.
Current code behavior:
API returns `401`
frontend clears token
frontend redirects to `/`
---
8. `JWTExpired`
Meaning:
token in localStorage is old
browser kept expired token
API correctly rejected it
Fix:
clear localStorage
sign in again
test quickly with fresh token
---
9. Prisma cannot connect to database
Error example:
```text
Can't reach database server at localhost:5432
```
Meaning:
database container is not running
Fix:
```bash
cd ~/projects/pindoc
docker compose up -d
```
---
Recommended debug commands
Check DB
```bash
cd ~/projects/pindoc
docker compose ps
```
Start DB
```bash
cd ~/projects/pindoc
docker compose up -d
```
Start API
```bash
cd ~/projects/pindoc/apps/api
pnpm dev
```
Start web
```bash
cd ~/projects/pindoc/apps/web
pnpm dev
```
API health
```bash
curl http://localhost:3001/health
```
Protected route with token
```bash
curl -i http://localhost:3001/documents -H "Authorization: Bearer PASTE_TOKEN_HERE"
```
Check web client ID
```bash
cd ~/projects/pindoc/apps/web
grep NEXT_PUBLIC_COGNITO_CLIENT_ID .env.local
```
Check API client ID
```bash
cd ~/projects/pindoc/apps/api
grep COGNITO_APP_CLIENT_ID .env
```
Compare full API env
```bash
cd ~/projects/pindoc/apps/api
cat .env
```
Compare full web env
```bash
cd ~/projects/pindoc/apps/web
cat .env.local
```
---
Browser console commands for Step-6 testing
Clear auth storage
```javascript
localStorage.clear()
sessionStorage.clear()
location.href = "/"
```
Check current token
```javascript
localStorage.getItem("pindoc_id_token")
```
Check current page
```javascript
window.location.href
```
Check localStorage keys
```javascript
Object.keys(localStorage)
```
---
Step-6 pass criteria
Step-6 can be considered fully passed when all of these are true:
landing page loads
sign-in redirects to Cognito
callback receives code
token exchange succeeds
token is stored in localStorage
dashboard stays loaded
browser can call `GET /documents`
Postman can call `GET /documents`
browser upload works
Postman upload works
uploaded document appears in list
---
Practical sign-off version
If you want a short sign-off, Step-6 is complete when you can do this successfully:
Start DB
Start API
Start web
Sign in through Cognito
Reach dashboard and stay there
`GET /documents` returns `200`
Upload a PDF
Refresh dashboard and see the uploaded file
---
Suggested GitHub note
You can store this file in your repo as:
```text
docs/step-6-testing-guide.md
```
or
```text
STEP-6-TESTING.md
```
---
Final note
During Step-6, the most common confusion comes from auth state becoming stale.  
If anything behaves strangely, always do this first:
```javascript
localStorage.clear()
sessionStorage.clear()
location.href = "/"
```
Then sign in again with a fresh session.
