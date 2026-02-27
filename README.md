# 📚 PageVault — Fully Automated Free Open Library

> **No Registration. No Sign Up. No Sign In. Ever.**
> Users upload books directly on the website — everything is automated behind the scenes.

[![Deploy Frontend](https://img.shields.io/badge/Frontend-GitHub%20Pages-black?logo=github)](https://pages.github.com)
[![Deploy Backend](https://img.shields.io/badge/Backend-Render.com-46E3B7?logo=render)](https://render.com)
[![License](https://img.shields.io/badge/License-MIT-gold)](LICENSE)

---

## 🏗️ How the System Works

```
User fills form & drops PDF/EPUB on website
           ↓
  Frontend sends to Backend (Render.com)
           ↓
  Backend commits file to GitHub via API
           ↓
  Backend updates books.json in GitHub
           ↓
  GitHub Pages auto-deploys (~30 seconds)
           ↓
  Book is LIVE in the library for everyone
```

**No GitHub account needed by the uploader.
No pull requests. No approval queue. Fully automatic.**

---

## 📦 Project Structure

```
pagevault/
│
├── frontend/
│   └── index.html          ← The entire website (deploy to GitHub Pages)
│
├── backend/
│   ├── server.js           ← Node.js/Express backend (deploy to Render.com)
│   ├── package.json        ← Dependencies
│   ├── .env.example        ← Environment variable template
│   └── .gitignore
│
├── books/                  ← Book files live here (auto-managed by backend)
├── books.json              ← Book catalog (auto-managed by backend)
└── README.md
```

---

## 🚀 COMPLETE SETUP GUIDE

### ═══ PART 1: Set Up the GitHub Repository ═══

#### Step 1 — Create a GitHub Account
Go to [github.com/signup](https://github.com/signup) if you don't have one (free).

#### Step 2 — Create the Repository
1. Click **"+"** → **"New repository"**
2. Name: `pagevault`
3. Visibility: **Public** (required for free GitHub Pages)
4. ✅ Check "Add a README file" (or leave unchecked — we'll upload ours)
5. Click **"Create repository"**

#### Step 3 — Upload the Frontend Files
1. In your new repo, click **"Add file"** → **"Upload files"**
2. Upload: `frontend/index.html` (rename to just `index.html` when uploading)
3. Also upload: `books.json` (the empty array: `[]`)
4. Create a folder: type `books/.gitkeep` in the file name field → commit
5. Click **"Commit changes"**

#### Step 4 — Enable GitHub Pages
1. Go to your repo → **Settings** tab
2. Left sidebar → click **Pages**
3. Under **Source** → select **"Deploy from a branch"**
4. Branch: `main` · Folder: `/ (root)`
5. Click **Save**

✅ Your site will be live at: `https://YOUR_USERNAME.github.io/pagevault`
(Takes 1–3 minutes for first deployment)

---

### ═══ PART 2: Create a GitHub Personal Access Token ═══

The backend needs this token to auto-commit files to your repo.

1. Go to [github.com/settings/tokens/new](https://github.com/settings/tokens/new)
   - Or: GitHub → Profile picture → Settings → Developer Settings → Personal access tokens → Tokens (classic) → Generate new token
2. Note: `PageVault Backend`
3. Expiration: **No expiration** (or set a long expiry)
4. Scopes — check these:
   - ✅ `repo` (gives full repo access — simplest option)
   - OR specifically: ✅ `public_repo` (enough for public repos)
5. Click **"Generate token"**
6. **COPY THE TOKEN NOW** — you won't see it again!

Token looks like: `ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`

---

### ═══ PART 3: Deploy the Backend to Render.com ═══

Render.com offers a **free tier** that is perfect for this backend.

#### Step 1 — Create a Render Account
Go to [render.com](https://render.com) → Sign up with your GitHub account (free).

#### Step 2 — Create a New Backend Repository
The backend needs its own GitHub repo (separate from the frontend).

1. Create a new GitHub repo: `pagevault-backend`
2. Upload the contents of the `backend/` folder:
   - `server.js`
   - `package.json`
3. **DO NOT upload `.env`** — it contains your secret token!

#### Step 3 — Create a Web Service on Render
1. In Render dashboard → click **"New +"** → **"Web Service"**
2. Connect your GitHub account if prompted
3. Select your `pagevault-backend` repository
4. Fill in the settings:

| Setting | Value |
|---------|-------|
| **Name** | `pagevault-backend` |
| **Region** | Closest to you |
| **Branch** | `main` |
| **Runtime** | `Node` |
| **Build Command** | `npm install` |
| **Start Command** | `node server.js` |
| **Instance Type** | **Free** |

5. Click **"Advanced"** to add environment variables

#### Step 4 — Add Environment Variables in Render
Click **"Add Environment Variable"** for each:

| Key | Value |
|-----|-------|
| `GITHUB_TOKEN` | `ghp_your_token_from_step_2` |
| `GITHUB_OWNER` | `your_github_username` |
| `GITHUB_REPO` | `pagevault` |
| `GITHUB_BRANCH` | `main` |
| `ADMIN_SECRET` | `make-up-a-long-random-string-here` |
| `ALLOWED_ORIGINS` | `https://YOUR_USERNAME.github.io` |

6. Click **"Create Web Service"**

Render will build and deploy your backend. It takes 2–5 minutes.

Your backend URL will be: `https://pagevault-backend.onrender.com`

> ⚠️ **Free tier note:** On the free tier, Render spins down your service after 15 minutes of inactivity. The first request after inactivity takes ~30 seconds to wake up. For a library this is fine — uploads just take a moment longer after idle periods.

---

### ═══ PART 4: Connect Frontend to Backend ═══

1. Open `index.html` in a text editor
2. Find this section near the bottom (inside the `<script>` tag):

```javascript
const CONFIG = {
  BACKEND_URL: 'https://YOUR_BACKEND_URL.onrender.com',
  GITHUB_OWNER: 'YOUR_USERNAME',
  GITHUB_REPO: 'pagevault',
  GITHUB_BRANCH: 'main'
};
```

3. Replace:
   - `YOUR_BACKEND_URL` → your Render service name (e.g. `pagevault-backend`)
   - `YOUR_USERNAME` → your GitHub username
4. Also search for `YOUR_USERNAME` in the HTML and replace all occurrences with your GitHub username
5. Commit the updated `index.html` to your GitHub Pages repo

---

### ═══ PART 5: Test Everything ═══

1. Visit your GitHub Pages site: `https://YOUR_USERNAME.github.io/pagevault`
2. The **server status indicator** in the upload section should show **"✓ Server Online · Ready to Upload"**
3. Fill in the upload form with a test book
4. Click **"⚡ Upload to Library"**
5. Watch the progress bar — it should complete in 10–30 seconds
6. After ~30 seconds, **refresh the page** — your book should appear in the library!

---

## 🌟 Features

### Frontend
- 🔍 **Live search** — title, author, genre, description, language
- 🏷️ **Auto-generated genre filters** — built from data automatically
- ↕️ **Sort** — newest, oldest, A–Z title, A–Z author
- 📥 **Download** — saves PDF/EPUB to device
- 👁️ **Read online** — opens in browser tab
- 📤 **Drag & drop upload** — supports PDF and EPUB up to 50 MB
- 📊 **Live stats** — book count and genre count update automatically
- 📱 **Fully responsive** — mobile, tablet, desktop
- ♿ **Accessible** — ARIA labels, keyboard navigation, semantic HTML

### Backend
- ⚡ **Auto-commit to GitHub** — no manual steps required
- 📚 **Auto-updates books.json** — catalog is always current
- 🛡️ **File validation** — only PDF/EPUB, max 50 MB
- 🌐 **CORS configured** — only your domain can upload
- 🗑️ **Admin delete endpoint** — remove books if needed
- 🔄 **Duplicate file handling** — updates existing files safely
- 💓 **Health check endpoint** — frontend shows server status live

---

## 📖 How to Add Books Manually (Optional)

If you prefer to add books without the upload form, you can edit `books.json` directly on GitHub:

1. Go to your repo → click `books.json`
2. Click the **pencil (edit)** icon
3. Add an entry to the array:

```json
{
  "id": 13,
  "title": "Book Title",
  "author": "Author Name",
  "genre": "Fiction",
  "year": 1900,
  "language": "English",
  "size": "1.2 MB",
  "format": "PDF",
  "description": "Short description of the book.",
  "file": "books/book-filename.pdf",
  "color": "#9c3d2e"
}
```

4. Commit → site updates in ~30 seconds
5. Also upload the book file to the `books/` folder

---

## 🗑️ Deleting a Book (Admin)

Send a DELETE request to the backend with your admin secret:

```bash
curl -X DELETE \
  https://pagevault-backend.onrender.com/api/books/5 \
  -H "Authorization: Bearer YOUR_ADMIN_SECRET"
```

Replace `5` with the book's `id` and `YOUR_ADMIN_SECRET` with the value you set.

---

## 🔧 Local Development

### Run the backend locally

```bash
cd backend
cp .env.example .env
# Edit .env with your values
npm install
npm run dev   # uses nodemon for auto-restart
```

Backend will run at `http://localhost:3001`

### Test the frontend locally

Open `frontend/index.html` in a browser.
Change `BACKEND_URL` in the CONFIG to `http://localhost:3001` for local testing.

Or use a simple server:
```bash
cd frontend
npx serve .
```

---

## 🔐 Security Notes

- Your GitHub token is stored **only** in Render's environment variables — never in the code or repo
- CORS is configured to only accept requests from your GitHub Pages domain
- The admin delete endpoint requires `Authorization: Bearer <ADMIN_SECRET>`
- Files are validated for type (PDF/EPUB only) and size (50 MB max) on the server
- User input is HTML-escaped on the frontend to prevent XSS

---

## 🌍 Custom Domain (Optional)

1. Buy a domain from Namecheap, Cloudflare, etc.
2. In GitHub Pages settings → add your custom domain
3. Follow GitHub's [DNS configuration guide](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site)
4. Update `ALLOWED_ORIGINS` in Render to include your new domain

---

## ⚖️ Copyright Policy

PageVault only hosts legally free-to-distribute books:

- ✅ Public domain (generally pre-1928 in the US)
- ✅ Creative Commons licensed
- ✅ Author-granted permission
- ❌ Modern copyrighted works without permission

**Free book sources:**
| Source | Description |
|--------|-------------|
| [Project Gutenberg](https://www.gutenberg.org) | 70,000+ public domain books |
| [Standard Ebooks](https://standardebooks.org) | Beautifully edited public domain |
| [Open Library](https://openlibrary.org) | Borrowable + public domain |
| [Internet Archive](https://archive.org/details/texts) | Millions of texts |
| [ManyBooks](https://manybooks.net) | Public domain in many formats |

---

## 💰 Cost Breakdown

| Service | Cost |
|---------|------|
| GitHub (repo + Pages) | **Free** |
| Render.com (backend, free tier) | **Free** |
| Domain name (optional) | ~$10/year |
| **Total** | **$0** |

---

## 📄 License

MIT License — free to use, fork, modify, and deploy.

---

*PageVault — A library where knowledge flows freely. No Registration. No Sign Up. No Sign In.*
