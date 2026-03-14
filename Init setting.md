### to open project from ubutu to cursor.
```
cd ~/projects/pindoc
explorer.exe .
```
### Pull from github to local/cursor
```
git pull
```

cd ~/projects/pindoc/apps/api
Ctrl+C
pnpm dev

####
curl http://localhost:3001/documents \
  -H "Authorization: Bearer YOUR_TOKEN"
```
allowlist (add pnpm create to allowlist)    
Run ( run this time only)
or skip
```
```
NEXT_PUBLIC_API_BASE_URL=http://localhost:3001
NEXT_PUBLIC_COGNITO_DOMAIN=https://us-east-2bnamb8ggb.auth.us-east-2.amazoncognito.com
NEXT_PUBLIC_COGNITO_CLIENT_ID=6ci6vn85qd2s0537r9nv5tk1vq
NEXT_PUBLIC_COGNITO_REDIRECT_URI=http://localhost:3000/auth/callback
NEXT_PUBLIC_COGNITO_LOGOUT_URI=http://localhost:3000/
NEXT_PUBLIC_COGNITO_SCOPES=openid+email+profile
```
### How to start database if when colse
```
docker --version
docker compose version
```
```
cd ~/projects/pindoc
docker compose up -d
```
```
docker compose ps
```
### Start API again
```
cd ~/projects/pindoc/apps/api
pnpm dev
```
```
curl http://localhost:3001/health
```
```
cd ~/projects/pindoc/apps/web
pnpm dev
```
```
http://localhost:3000
```

```
curl http://localhost:3001/health
```
### Get token from web Console
```
copy(localStorage.getItem("pindoc_id_token"))
```
```
console.log(localStorage.getItem("pindoc_id_token"))
```
```
cd ~/projects/pindoc/apps/api
grep COGNITO_APP_CLIENT_ID .env
````
```
cd ~/projects/pindoc/apps/web
grep NEXT_PUBLIC_COGNITO_CLIENT_ID .env.local
```
### Cursor 
```
Allowlist = add pnpm prisma to allowlist
Run = this time only
```
### First verify database state
```
cd ~/projects/pindoc/apps/api
pnpm prisma studio
```
### check Upload status on wsl
```
cd ~/projects/pindoc/apps/api
pnpm exec ts-node src/workers/runExtractionWorker.ts
```
