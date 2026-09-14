# Nook — Rental Property Web App (starter)

A Django starter for a rental-listing site with email/password + Google/Facebook
login, a protected property-detail page, bot protection on signup, and a
profile page. This is stage 1 of the build — foundation + auth + homepage.

## What's built so far
- Custom user (email, phone, date of birth, profile picture)
- Email/password signup & login (via `django-allauth`)
- Google & Facebook social login (needs your API keys — see below)
- Profile page + change-password page
- Cloudflare Turnstile bot protection on signup (server-side verified, not
  just a front-end widget)
- Homepage with a grid of "featured" properties
- Property detail page that **requires login** (redirects to login, then
  returns you to the property afterward)
- Collapsible left sidebar + top navbar with profile avatar/dropdown
- Django admin panel to add properties without building an admin UI yourself

## Not built yet (tell me which to do next)
- WeChat login (needs a China-registered developer account — flag if you have one)
- Search/filter on the homepage
- "My listings" for landlords, favorites/saved homes, messaging
- Production deployment config (Postgres, static file hosting, email sending)

## Setup (run this on your own machine — it needs internet access)

1. **Install Python 3.11+** if you don't have it.

2. **Create a virtual environment** (keeps this project's packages separate
   from everything else on your machine):
   ```bash
   cd rental_app
   python3 -m venv venv
   source venv/bin/activate        # Windows: venv\Scripts\activate
   ```

3. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

4. **Create your `.env` file:**
   ```bash
   cp .env.example .env
   ```
   Generate a real secret key and paste it in:
   ```bash
   python -c "from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())"
   ```
   Leave the Google/Facebook/Turnstile fields blank for now — the app works
   without them (social buttons just won't work yet, and Turnstile uses
   Cloudflare's test keys which always pass).

5. **Create the database tables:**
   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

6. **Create an admin account** (so you can log into `/admin/` and add
   properties):
   ```bash
   python manage.py createsuperuser
   ```

7. **Run the server:**
   ```bash
   python manage.py runserver
   ```
   Visit http://127.0.0.1:8000

8. **Add some properties:** go to http://127.0.0.1:8000/admin/, log in with
   your superuser account, add a few `Property` entries, and check "is
   featured" so they show up on the homepage.

## Wiring up Google/Facebook login (optional, when ready)
1. Google: create OAuth credentials at
   https://console.cloud.google.com/apis/credentials — set the redirect URI
   to `http://127.0.0.1:8000/accounts/google/login/callback/`
2. Facebook: create an app at https://developers.facebook.com/apps — set the
   redirect URI to `http://127.0.0.1:8000/accounts/facebook/login/callback/`
3. Paste the client ID/secret pairs into `.env`, restart the server.

## Wiring up real Turnstile keys (before going live)
Get free keys at https://dash.cloudflare.com/?to=/:account/turnstile and put
them in `.env`. The test keys in `.env.example` always pass — fine for
development, not for production.

## Project structure
```
rental_app/
  config/            # settings, root urls
  apps/
    accounts/        # custom user model, signup form, profile views
    listings/        # Property model, homepage, detail view
    core/            # shared context processors
  templates/         # base.html (sidebar/topbar) + allauth overrides
  static/css/        # site styling
```
