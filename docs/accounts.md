# Accounts App

Location: `proservex/accounts/`

Handles user authentication, registration, and role management.

## Model: `CustomUser` (`models.py:4`)

Extends `AbstractUser` with additional fields:

| Field | Type | Details |
|-------|------|---------|
| `role` | CharField | `customer` or `provider` (default: `customer`) |
| `phone` | CharField | max 15 chars |
| `city` | CharField | max 100 chars |
| `profile_pic` | ImageField | uploaded to `profile_pics/` |

String representation: `"username (role)"`

## Forms

### `UserRegisterForm` (`forms.py:4`)

Registration form with fields: `username`, `email`, `phone`, `city`, `role` (RadioSelect), `service_category` (shown for providers), `password1`, `password2`.

## Views

| View | Route | Method | Description |
|------|-------|--------|-------------|
| `register_view` | `register/` | GET/POST | Registers user, creates ProviderProfile if role is `provider` |
| `login_view` | `login/` | GET/POST | Authenticates user with username/password |
| `logout_view` | `logout/` | GET | Logs out and redirects to home |
| `dashboard_view` | `dashboard/` | GET | Role-aware dashboard |

### Dashboard Behavior

- **Provider**: shows booking requests, earnings, reviews received
- **Customer**: shows booking history, stats

## URLs (`urls.py`)

| Pattern | View Name | URL Name |
|---------|-----------|----------|
| `register/` | `register_view` | `register` |
| `login/` | `login_view` | `login` |
| `logout/` | `logout_view` | `logout` |
| `dashboard/` | `dashboard_view` | `dashboard` |
