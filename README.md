# Issue Tracker

> A clone of the [Taiga](https://taiga.io/) Issue Tracker, developed as a project for the **ASW** (Aplicacions i Serveis Web) course at the **FIB, UPC Barcelona**.

**Live app:** https://issuetracker-ff8u.onrender.com/  
**Group ID:** it115

---

## Tech Stack

| Layer | Technology |
|---|---|
| Backend framework | Django 6.0.3 |
| Database | PostgreSQL 17 |
| Authentication | django-allauth 65.15.0 — GitHub OAuth |
| REST API | JsonResponse (no DRF) — Richardson Maturity Level 2 |
| File storage (dev) | Django FileSystemStorage |
| File storage (prod) | Google Cloud Storage |
| Frontend | Next.js 16 · React 19 · TypeScript · Tailwind CSS v4 (separate repo) |

---

## Features

- **Issues** — full CRUD: create, list, edit, delete
- **Inline editing** — update subject, description, status, assignee, type, severity, priority, deadline, and tags directly from the detail view
- **Comments** — add, edit, and delete comments on issues
- **Attachments** — upload and remove files attached to issues
- **Watchers** — subscribe/unsubscribe to issue updates
- **Filters, search & sorting** — filter by type, severity, priority, status, and assignee; full-text search on subject/description; sortable columns
- **Bulk create** — create multiple issues at once from a text list
- **Activity log** — automatic history of changes to issue fields
- **User profile** — bio, avatar upload, personal API key, list of assigned issues and comments
- **Settings** — full CRUD for Statuses, Priorities, Types, Severities, Tags, and Due Dates; drag-style reordering (`move-up` / `move-down`); `is_default` flag; closed-state toggle for Statuses
- **GitHub OAuth** — sign in with GitHub via django-allauth
- **REST API** — Richardson Level 2, authenticated with an API key in the `Authorization` header
- **OpenAPI / Swagger** — full API documentation in `api/api.yml`

---

## Project Structure

```
issueTracker/
├── api/
│   └── api.yml                  # OpenAPI 3.0 specification
├── docker-services/
│   ├── docker-compose.yml       # PostgreSQL service
│   └── .env.example             # Environment variables for Docker
├── src/
│   ├── issueTracker/            # Django project settings
│   │   ├── settings.py
│   │   └── urls.py
│   ├── issues/                  # Main Django app
│   │   ├── models.py            # Issue, Comment, Attachment, Profile, Status, Priority, IssueType, Severity, Tag, DueDate
│   │   ├── controllers/
│   │   │   ├── controllers_api.py   # API response functions (JsonResponse)
│   │   │   └── controllers_web.py   # Web (HTML template) response functions
│   │   ├── dispatchers.py       # Routes requests to web or API controller based on Accept/Content-Type
│   │   ├── helpers.py           # Shared business logic (filtering, creation helpers)
│   │   ├── views.py             # Additional view functions
│   │   └── templates/           # Django HTML templates
│   ├── .env.example             # Environment variables for local backend
│   └── manage.py
├── requirements.txt
└── render.yaml                  # Render.com deployment config
```

---

## Prerequisites

- Python 3.12+
- Docker & Docker Compose
- Node.js 18+ *(only for the frontend)*

---

## Local Setup — Backend

**1. Clone the repository**

```bash
git clone <repo-url>
cd issueTracker
```

**2. Set up environment files**

```bash
cp src/.env.example src/.env
cp docker-services/.env.example docker-services/.env
# Adjust values if needed (defaults work out of the box)
```

**3. Create and activate a Python virtual environment**

```bash
python3 -m venv venv
source venv/bin/activate
```

**4. Install dependencies**

```bash
pip install -r requirements.txt
```

**5. Start the PostgreSQL container**

```bash
sudo docker compose --project-directory docker-services/ up -d
```

**6. Run migrations and start the development server**

```bash
cd src
python manage.py migrate
python manage.py runserver
```

The app will be available at http://localhost:8000.

**7. Configure GitHub OAuth**

To enable GitHub login locally:

1. Go to http://localhost:8000/admin/ and log in with your superuser account.
2. Under **Sites**, create a site with domain `localhost:8000`.
3. Under **Social Applications**, create a new entry:
   - Provider: `GitHub`
   - Client ID / Secret Key: credentials from your [GitHub OAuth App](https://github.com/settings/developers)
   - Site: `localhost:8000`

---

## Local Setup — Frontend

The frontend lives in a separate repository.

**1. Clone the frontend repo and install dependencies**

```bash
git clone <frontend-repo-url>
cd <frontend-folder>
npm install
```

**2. Configure the API base URL**

```bash
# Create .env.local in the frontend root
echo "NEXT_PUBLIC_API_BASE_URL=http://localhost:8000" > .env.local
```

**3. Start the development server**

```bash
npm run dev
```

The frontend will be available at http://localhost:3000.

---

## API Documentation

The full API specification is at `api/api.yml` (OpenAPI 3.0).

To browse it interactively, paste the contents into **[Swagger Editor](https://editor.swagger.io/)**.

**Authentication:** include your personal API key in every request:

```
Authorization: <your-api-key>
```

Your API key is shown in your user profile page.

