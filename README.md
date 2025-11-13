# Conference Article Management Platform

A **full-stack web application** for managing conference submissions, articles, and peer reviews.  
Includes a **custom REST API**, **JWT authentication**, and **role-based access control (RBAC)**.

---

## Deployment

This project is deployed using a hybrid architecture:

**Frontend** → hosted on Vercel

**Backend API** → hosted on a Linux Server, secured with Nginx reverse proxy, HTTPS (Let's Encrypt) and UFW firewall

#### [Production Link](https://proiect-tw-skibidi-rizzlers.vercel.app)

## Test Users

For demo purposes, the platform can be tested using the following accounts.  
Each user has a different role so that all flows (author, reviewer, conference organizer) can be tested easily.

| Role                 | Username     | Password |
|----------------------|--------------|----------|
| Conference Organizer | test         | 1234     |
| Reviewer             | test1        | 1234     |
|                      | test2        | 1234     |
| Author               | test3        | 1234     |
|                      | test4        | 1234     |

### How to use the test accounts

1. Go to the login page of the deployed app
2. Log in with one of the accounts above (username + password).
3. Try different flows:
   - Login as **Organizer** → create a conference.
   - Login as **Author** → register to a conference and submit an article.
   - Login as **Reviewer** → add reviews, change review status, approve articles.

## Features

### Authentication & Authorization
- Login & signup using JSON Web Tokens (JWT)
- Tokens handled securely
- Protected backend routes with server-side validation
- Frontend guards based on user session and role

### Role-Based Access Control (RBAC)

| Role        | Capabilities |
|-------------|--------------|
| **Author**  | Creates and edits own articles, views reviews, responds to reviewer feedback |
| **Reviewer**| Leaves reviews on articles, approves/rejects review status, marks article as ready |
| **Organizer**   | Manages conferences, articles, users, and roles |

RBAC is enforced on:
- Backend (middleware checking JWT + user role)
- Frontend (conditional rendering of UI actions)

---

## Articles & Review Workflow

1. **Author** submits article to a selected conference.
2. **Reviewer** submits reviews that include:
   - review header
   - comments
   - current review status: `opened` → `in_progress` → `closed`
3. An article can be **approved** only when **all reviews are closed**.
4. After approval, the article is available to anyone registered to the conference.

---

## Architecture

### Frontend
- React (functional components + hooks)
- Redux Toolkit for global state (user session, auth, role)
- Custom hooks:  
  `useGetArticle`, `useAddArticle`, `useEditArticle`, `useEditReview`
- Full UI access control based on role

### Backend
- Node.js + Express.js
- Custom REST API
- JWT authentication middleware
- RBAC authorization
- PostgreSQL database with relational schema

```
/client/proiect                   → React frontend
  /src
    /pages                → Pages (Article, WriteArticle, Login...)
    /components           → UI Components
    /hooks                → Custom fetch hooks (useAddArticle, ...)
/server                   → Node.js backend (custom REST API)
  /routes
  /controllers
  /middleware
/database                 → PostgreSQL schema / migrations
```

---

## REST API Endpoints (Core)

### Authentication
| Method | Endpoint        | Description |
|--------|------------------|-------------|
| POST   | `/auth/signup`   | Register user |
| POST   | `/auth/login`    | Login & receive JWT |

### Articles
| Method | Endpoint                     | Access   | Description |
|--------|------------------------------|----------|-------------|
| POST   | `/conferences/:id/articles`  | Author   | Create an article |
| GET    | `/articles/:id`              | Auth     | Get article details |
| PATCH  | `/articles/:id`              | Author   | Edit article |
| PATCH  | `/articles/:id/approve`      | Reviewer/Admin | Approve article (only if all reviews are closed) |

### Reviews
| Method | Endpoint                  | Access     | Description |
|--------|---------------------------|------------|-------------|
| POST   | `/articles/:id/reviews`   | Reviewer   | Add review to article |
| PATCH  | `/reviews/:id`            | Reviewer   | Update review status |

---

## Local Development Setup

### Requirements
Before running the project locally, make sure you have:

- **Node.js** ≥ 18
- **npm** or **yarn**

### Clone the repository

```sh
git clone https://github.com/alexandruvasu69/proiect-tw-skibidi-rizzlers.git
cd proiect-tw-skibidi-rizzlers
```

### Install dependencies

- **Frontend**
```
cd client/proiect
npm install
```

- **Backend**
```
cd server
npm install
```

### Environment Variables
Create a .env file in each directory (/client/proiect and /server) and add:
- **Frontend**
```
VITE_API=http://localhost:3000
```
- **Backend**
```
PORT=3000
TOKEN_SECRET=secret_token
```

### Start the development servers
- **Start backend (Express API)**
```
cd server
npm run dev
```

- **Start frontend**
```
cd client/proiect
npm run dev
```






