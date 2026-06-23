# Architecture

## Application Flow

```
User Registration
       ↓
     Login
       ↓
  Search Service
       ↓
 View Providers
       ↓
  Book Service
       ↓
Provider Accepts
       ↓
Service Completed
       ↓
Customer Reviews
```

## URL Routing

| Prefix | App | Purpose |
|--------|-----|---------|
| `/` | `core` | Homepage, search |
| `/accounts/` | `accounts` | Registration, login, logout, dashboard |
| `/providers/` | `providers` | Provider listing, detail, profile edit |
| `/bookings/` | `bookings` | Booking management, payments |
| `/admin/` | `django.contrib.admin` | Admin panel |

## Request Flow

```
Browser → URL → View → Model → Template → Response
                     ↕
                  Database
```

## Context Processors

`core.context_processors.categories_processor` makes `SERVICE_CATEGORIES` available in all templates.

## Static & Media Files

| Directory | Purpose | Config |
|-----------|---------|--------|
| `static/` | Source static files | `STATICFILES_DIRS` |
| `staticfiles/` | Collected static files | `STATIC_ROOT` |
| `media/` | User uploads | `MEDIA_ROOT` |

## Settings

Full configuration in `proservex/proservex/settings.py`:
- Custom user model (`accounts.CustomUser`)
- SQLite database
- Static files served from `/static/`
- Media files served from `/media/` (debug mode)
