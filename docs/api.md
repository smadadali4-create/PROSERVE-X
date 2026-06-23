# API Endpoints

All routes are server-rendered Django views (no REST API). Testing can be done via the included `proservex.http` file (VS Code REST Client format).

## Core

| Method | URL | Description |
|--------|-----|-------------|
| GET | `/` | Homepage |
| GET | `/search/?query=&category=&city=` | Search providers |

## Accounts

| Method | URL | Description |
|--------|-----|-------------|
| GET | `/accounts/register/` | Registration page |
| POST | `/accounts/register/` | Submit registration |
| GET | `/accounts/login/` | Login page |
| POST | `/accounts/login/` | Submit login |
| GET | `/accounts/logout/` | Logout |
| GET | `/accounts/dashboard/` | User dashboard |

## Providers

| Method | URL | Description |
|--------|-----|-------------|
| GET | `/providers/` | All providers |
| GET | `/providers/category/<str:category>/` | Filtered by category |
| GET | `/providers/<int:pk>/` | Provider detail |
| POST | `/providers/<int:pk>/` | Submit booking or review |
| GET | `/providers/edit/` | Edit profile form |
| POST | `/providers/edit/` | Save profile edits |

## Bookings

| Method | URL | Description |
|--------|-----|-------------|
| GET | `/bookings/` | My bookings (role-aware) |
| GET | `/bookings/update/<int:pk>/<str:status>/` | Update booking status |
| POST | `/bookings/pay/<int:pk>/` | Make payment |
| GET | `/bookings/payments/` | Payment panel |

## Admin

| Method | URL | Description |
|--------|-----|-------------|
| GET/POST | `/admin/` | Django admin interface |
