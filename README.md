# HEU Digital Library Management System

A simple web-based library management application built with React JS (Vite) and Node.js + Express.js. The backend stores all records in a local JSON file, so SQLite, better-sqlite3, and native build tools are not needed.

## Main features
- Librarian and student sign-in
- Dashboard statistics
- Book catalog: add, view, edit, delete, and search books
- Catalog categories, authors, publishers, and ebook catalog section
- Member management for student and librarian accounts
- Borrowing and book return management
- Circulation history and overdue status
- Persistent JSON storage in `backend/data/library-data.json`
- HEU school seal and responsive sidebar navigation

## Requirements
- Node.js (LTS recommended)
- npm
- Visual Studio Code

## Installation and run
Open the project folder in VS Code. Use two separate terminals.

### Terminal 1 — Backend
```bash
cd backend
npm install
node server.js
```
Backend API: http://localhost:5000/api
Health check: http://localhost:5000/api/health

### Terminal 2 — Frontend
```bash
cd frontend
npm install
npm run dev
```
Open the Vite URL printed in the terminal, usually http://localhost:5173.

## Demo accounts
- Librarian: `librarian@library.local` / `Admin123!`
- Student: `student@library.local` / `Student123!`

## Data storage
The backend automatically creates `backend/data/library-data.json` on first start. It contains users, books, and borrowing records. Keep this file to preserve your records. Back it up regularly. Do not store real sensitive personal information in this classroom demo without adding appropriate security and deployment controls.

## Project structure
```text
digital-library-management-system/
  backend/
    data/                 # created automatically; JSON records
    database.js           # JSON storage functions (not SQL)
    server.js              # Express API and business logic
    package.json
  frontend/
    public/heu-logo.png
    src/App.jsx
    src/App.css
    src/main.jsx
    package.json
  README.md
```


## Deployment (GitHub + Render + Vercel)

This project remains in one GitHub repository. Deploy the backend and frontend as
separate services from their folders; no existing API routes or data features were
removed by the visual update.

### 1. Push the project to GitHub
Upload/push the complete `digital-library-management-system` folder to one repository.
Keep both `backend` and `frontend` folders in the repository.

### 2. Deploy the backend on Render
- Create a **New Web Service** and connect the GitHub repository.
- Set **Root Directory** to `backend`.
- Build Command: `npm install`
- Start Command: `npm start`
- Add environment variables:
  - `JWT_SECRET`: a long, private random value
  - `CLIENT_ORIGIN`: your deployed Vercel frontend URL (for example `https://your-app.vercel.app`)
- Deploy. Confirm `https://YOUR-RENDER-SERVICE.onrender.com/api/health` returns JSON with `status: "ok"`.

A `render.yaml` Blueprint is included as a convenience; you can also enter the settings manually.

### 3. Deploy the frontend on Vercel
- Import the same GitHub repository into Vercel.
- Set **Root Directory** to `frontend`.
- Framework Preset: **Vite**.
- Build Command: `npm run build`
- Output Directory: `dist`
- Add environment variable `VITE_API_URL` with the full backend API base, ending in `/api`:
  `https://YOUR-RENDER-SERVICE.onrender.com/api`
- Deploy or redeploy after adding the environment variable.

### 4. Connect the two deployments
Copy the final Vercel domain into Render's `CLIENT_ORIGIN` value exactly (include `https://`, no trailing slash), then redeploy the Render service. If you use a Vercel preview domain too, add the appropriate origin handling in the backend before using preview deployments.

### Important data-storage note
The backend currently stores records in a JSON file on the service filesystem. Some hosting plans/services may use an ephemeral filesystem, so records can be lost when the service restarts or redeploys unless persistent storage is configured. For long-term real use, move the data to a managed database or attach supported persistent storage.
