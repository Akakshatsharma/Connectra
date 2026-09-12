# Connectra

Connectra is a Django-powered social media web application that lets users sign up, build a profile, share image-based posts with captions, follow other users, like posts, and discover new content through an explore feed and search. It's a full-stack Django project built from the ground up — custom authentication, a relational data model for posts/likes/follows, server-rendered templates, and a production deployment on Render.

**Live demo:** [social-media-17h9.onrender.com](https://social-media-17h9.onrender.com/loginn/?next=/)
**Repository:** [github.com/Akakshatsharma/Connectra](https://github.com/Akakshatsharma/Connectra)

> Note: the live instance runs on Render's free tier, so the first request after a period of inactivity may take a few seconds to spin up.

---

## ✨ Features

- **User registration & authentication** — sign up with a username, email, and password (`django.contrib.auth`), with duplicate-user handling on signup.
- **Login / logout** — session-based authentication guarding all core views with `@login_required`.
- **Personalized home feed** — the home page shows posts from the logged-in user and the accounts they follow, newest first.
- **Image posts with captions** — upload an image and a caption to create a post (handled via `Pillow` and Django's `ImageField`).
- **Like / unlike posts** — toggle a like on any post; the like count updates immediately.
- **User profiles** — each user has a profile with a profile picture, bio, and location.
- **Edit profile** — update profile picture, bio, and location from a modal on the profile page.
- **Follow / unfollow** — follow or unfollow other users directly from their profile, with live follower/following counts.
- **Explore page** — browse every post on the platform, not just posts from followed accounts.
- **Search** — search for users and posts (by caption) from a global search modal.
- **Delete posts** — post owners can delete their own posts from their profile.
- **Responsive UI** — built with Bootstrap (grid, modals, cards) and Font Awesome icons for a clean, mobile-friendly layout.

---

## 🛠️ Tech Stack

| Technology | Purpose |
|---|---|
| Python | Backend programming language |
| Django 4.2 | Web framework (MVT architecture, ORM, auth) |
| SQLite | Default relational database |
| Pillow | Image handling for post and profile images |
| Gunicorn | Production WSGI server |
| WhiteNoise | Serves compressed static files in production |
| HTML5 | Page structure (Django templates) |
| CSS3 | Custom styling (`static/css/app.css`) |
| Bootstrap | Responsive layout, grid, modals, and components |
| Font Awesome | Icon set used across the sidebar and UI |
| Render | Cloud hosting / deployment platform |

---

## 📂 Project Structure

```text
Connectra/
├── media/
│   ├── post_images/           # Uploaded post images
│   └── profile_images/        # Uploaded profile pictures
├── socialmedia/                # Project configuration
│   ├── settings.py
│   ├── urls.py
│   ├── asgi.py
│   └── wsgi.py
├── static/
│   ├── css/
│   │   └── app.css
│   └── js/
│       └── app.js
├── templates/
│   ├── main.html               # Home feed
│   ├── explore.html            # Explore feed
│   ├── profile.html            # User profile page
│   ├── edit_profile.html       # Edit profile modal
│   ├── profile_upload.html     # Create-post modal (profile page)
│   ├── modal.html              # Create-post modal (home page)
│   ├── search.html             # Search modal
│   ├── search_user.html        # Search results page
│   ├── loginn.html             # Login page
│   └── signup.html             # Signup page
├── userauth/                   # Main application
│   ├── migrations/
│   ├── models.py                # Profile, Post, LikePost, Followers
│   ├── views.py                 # Auth, feed, posts, likes, follow, search
│   ├── urls.py
│   ├── admin.py
│   └── tests.py
├── manage.py
├── requirements.txt
└── .gitignore
```

---

## 🗄️ Data Model

The `userauth` app defines four models:

- **Profile** — one-to-one style link to Django's `User`, storing `bio`, `location`, and `profileimg`.
- **Post** — a UUID-keyed post with `user` (username), `image`, `caption`, `created_at`, and `no_of_likes`.
- **LikePost** — tracks which username liked which `post_id`, used to toggle likes on/off.
- **Followers** — a simple `follower` → `user` mapping used to compute followers, following, and feed visibility.

---

## 🔐 Authentication Flow

1. **Sign up** (`/signup/`) creates a `User` and an associated `Profile` in one step, then logs the user in.
2. **Log in** (`/loginn/`) authenticates against Django's built-in auth system and redirects to the home feed.
3. All content routes (home, explore, profile, upload, likes, follow, search, delete) are protected with `@login_required` and redirect anonymous users back to `/loginn/`.
4. **Log out** (`/logoutt/`) ends the session and returns to the login page.

---

## 🚀 Getting Started

### Prerequisites
- Python 3.10+
- pip

### Installation

```bash
# Clone the repository
git clone https://github.com/Akakshatsharma/Connectra.git
cd Connectra

# Create and activate a virtual environment
python -m venv venv
source venv/bin/activate   # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Apply migrations
python manage.py migrate

# Create a superuser (optional, for /admin)
python manage.py createsuperuser

# Run the development server
python manage.py runserver
```

Visit `http://127.0.0.1:8000/` and sign up for a new account to get started.

### Environment variables

The app reads the following from the environment, with safe local defaults:

| Variable | Purpose | Default |
|---|---|---|
| `SECRET_KEY` | Django secret key | Falls back to a built-in dev key |
| `DEBUG` | Enables/disables debug mode | `False` |

---

## ☁️ Deployment

Connectra is deployed on **Render** as a Django web service:

- **Gunicorn** serves the application in production.
- **WhiteNoise** serves compressed static assets without needing a separate static file host.
- `ALLOWED_HOSTS` is configured for `.onrender.com` as well as local development.

---

## 🔭 Possible Improvements

Ideas for extending the project further:

- Move from SQLite to PostgreSQL for production-grade persistence.
- Add pagination/infinite scroll to the home and explore feeds.
- Add comments on posts.
- Add real-time notifications for likes and follows.
- Add automated test coverage (`userauth/tests.py` is currently a placeholder).

---

## 👤 Author

**Akakshat Sharma**
GitHub: [@Akakshatsharma](https://github.com/Akakshatsharma)

---

## 📄 License

No license file is currently included in this repository. Add one (e.g. MIT) if you intend for others to reuse this code.
