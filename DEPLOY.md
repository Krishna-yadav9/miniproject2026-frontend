# Deploying the ApexStore frontend on Render

Deploy the **backend first** and copy its URL (for example `https://apexstore-backend.onrender.com`).

## Static Site on Render
1. Push this `frontend` folder to a GitHub repository (the `package.json` must be in the repository root, or set Render's *Root Directory* to `frontend`).
2. Render dashboard -> **New -> Static Site** -> select the repository.
3. Settings:
   - **Build Command:** `npm install && npm run build`
   - **Publish Directory:** `dist`
4. **Environment variables:**
   - `VITE_BACKEND_URL` = your backend URL (for example `https://apexstore-backend.onrender.com`)
   - `NODE_VERSION` = `22`
5. Under **Redirects/Rewrites** add: source `/*`, destination `/index.html`, action **Rewrite**.
   (Or use **New -> Blueprint**, which reads `render.yaml`.)
6. Click **Create Static Site**. When it finishes, open the site URL.

## Connect it to the backend
Copy the frontend URL and set it as `CORS_ALLOWED_ORIGINS` on the backend service, then let the backend redeploy.

## If you see "Backend server is not reachable"
- `VITE_BACKEND_URL` is missing or wrong. It must be set **before** the build; after changing it, trigger a **Clear build cache & deploy**.
- The free backend may be asleep: open `<backend-url>/api/health`, wait until it shows `{"status":"UP"}`, then reload the site.
- `CORS_ALLOWED_ORIGINS` on the backend does not match the frontend URL.

## Local development
```
npm install
npm run dev        # http://localhost:3000, proxies /api to http://localhost:8081
```
