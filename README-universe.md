# How to Use This Project as a Template

A step-by-step guide to clone, customize, and deploy your own full-stack web app using this repo as a starting point.

---

## What You Get

- A **Node.js/Express** backend deployed as a Vercel serverless function
- A **React** frontend (Vite) deployed as a static site on Vercel
- **GitHub Actions** CI/CD that lints, tests, builds, and deploys automatically on every push to `main`

---

## Step 1 — Fork or Clone

```bash
git clone https://github.com/fnywayz/vercel-deploy-starter.git
cd vercel-deploy-starter
```

---

## Step 2 — Customize the Backend

Edit `backend/src/app.js`. Replace the single `GET /api/hello` route with your own routes:

```js
app.get("/api/users", (req, res) => {
  res.json([{ id: 1, name: "Alice" }]);
});
```

Add more dependencies as needed:

```bash
cd backend
npm install <package-name>
```

---

## Step 3 — Customize the Frontend

Edit `frontend/src/App.jsx`. Replace the "Hello Vercel !" heading with your own UI:

```jsx
function App() {
  return <h1>My App</h1>;
}
export default App;
```

Install additional frontend packages:

```bash
cd frontend
npm install <package-name>
```

---

## Step 4 — Set Up Vercel

1. Create an account at https://vercel.com
2. Create **two** new projects in the Vercel dashboard:
   - One named `my-backend` (or anything)
   - One named `my-frontend`
3. Link each directory to its Vercel project:

```bash
cd backend
npx vercel link   # select the backend project you created

cd ../frontend
npx vercel link   # select the frontend project you created
```

This creates a `.vercel/` directory in each folder with the project link.

---

## Step 5 — Set Up GitHub Actions

1. Go to your GitHub repo → **Settings → Secrets and variables → Actions**
2. Create a new secret:
   - **Name:** `VERCEL_TOKEN`
   - **Value:** go to https://vercel.com/account/tokens → Create Token → copy it

---

## Step 6 — Push and Deploy

```bash
git add .
git commit -m "first deploy"
git push origin main
```

GitHub Actions will:
1. Run CI (install, lint, test, build)
2. Run CD (deploy backend + frontend to Vercel)

---

## Step 7 — Add a Custom Domain (Optional)

In the Vercel dashboard, go to your frontend project → **Settings → Domains** → add your domain.

---

## Project Structure Reference

```
.
├── backend/
│   ├── src/app.js          # Your API routes go here
│   ├── package.json
│   └── vercel.json         # Routes all requests to app.js
├── frontend/
│   ├── src/App.jsx         # Your React UI goes here
│   ├── src/index.jsx       # React entry point
│   ├── index.html          # Vite HTML template
│   ├── package.json
│   ├── vercel.json         # SPA rewrite rules
│   └── vite.config.js      # Vite configuration
├── .github/workflows/
│   ├── CI.yaml             # Lint + Test + Build
│   └── CD.yaml             # Deploy to Vercel
└── README.md
```

---

## Tips

- **Add a database:** Install a client like `pg` or `mongoose` in `backend/`. Use environment variables in Vercel for connection strings (Settings → Environment Variables).
- **Add auth:** Use a library like `next-auth` or `passport` in the backend.
- **Change the frontend framework:** Swap React for Vue, Svelte, etc. Update `vite.config.js` and `package.json` accordingly.
- **Run locally:** Start both the backend and frontend in separate terminals. Point `frontend/.env` to `http://localhost:3000/api`.
- **Add tests:** Replace the placeholder test script in `package.json` with Jest, Vitest, or any test runner.

---

## Alternative Stacks

This template uses Node.js/Express + React, but you can adapt it for other stacks:

### Backend Options (on Vercel)

| Language | Framework | Vercel Runtime | Notes |
|----------|-----------|----------------|-------|
| **Node.js** | Express, Fastify, Koa | `@vercel/node` | Native support, this repo |
| **Python** | Flask, FastAPI | `@vercel/python` | Replace `app.js` with `app.py` |
| **Python** | Django | `@vercel/python` | Needs `vercel.json` config for ASGI |
| **Java** | Spring Boot | `@vercel/java` | Requires Maven/Gradle build |
| **Go** | Gin, Echo, Fiber | `@vercel/go` | Single binary deployment |
| **Rust** | Actix, Axum | `@vercel/rust` | Requires Cargo build |
| **Ruby** | Sinatra, Rails | `@vercel/ruby` | Needs Gemfile |

### Frontend Options (on Vercel)

| Framework | Package | Notes |
|-----------|---------|-------|
| **React** | `react`, `react-dom` | This repo uses Vite + React |
| **Vue** | `vue`, `@vitejs/plugin-vue` | Swap `@vitejs/plugin-react` for Vue plugin |
| **Svelte** | `svelte`, `@sveltejs/vite-plugin-svelte` | Use SvelteKit or plain Vite |
| **Angular** | `@angular/cli` | Vercel supports Angular natively |
| **Plain HTML** | None | Just static files, simplest option |
| **Next.js** | `next` | Vercel's own framework, first-class support |
| **Nuxt** | `nuxt` | Vue meta-framework, Vercel supported |

### Quick Swap Guide

**To use Python + Flask instead of Node + Express:**

1. Replace `backend/src/app.js` with `backend/app.py`:
   ```python
   from flask import Flask, jsonify
   from flask_cors import CORS

   app = Flask(__name__)
   CORS(app)

   @app.route("/api/hello")
   def hello():
       return jsonify({"message": "Hello Vercel !"})

   if __name__ == "__main__":
       app.run(port=3000)
   ```
2. Add `requirements.txt`:
   ```
   flask
   flask-cors
   ```
3. Update `backend/vercel.json`:
   ```json
   {
     "builds": [
       { "src": "app.py", "use": "@vercel/python" }
     ],
     "routes": [
       { "src": "/(.*)", "dest": "app.py" }
     ]
   }
   ```

**To use Vue instead of React:**

1. Replace `frontend/src/App.jsx` with `frontend/src/App.vue`:
   ```vue
   <template>
     <h1>Hello Vercel !</h1>
   </template>
   ```
2. Update `frontend/src/index.jsx` → `frontend/src/main.js`:
   ```js
   import { createApp } from 'vue'
   import App from './App.vue'
   createApp(App).mount('#app')
   ```
3. Update `vite.config.js`:
   ```js
   import vue from '@vitejs/plugin-vue'
   export default defineConfig({ plugins: [vue()] })
   ```
4. Update `package.json`: replace `react`/`react-dom` with `vue`, replace `@vitejs/plugin-react` with `@vitejs/plugin-vue`
