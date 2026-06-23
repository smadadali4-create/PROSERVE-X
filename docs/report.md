# ProServe X — Project Report

**Date:** May 2026  
**Version:** 1.0  
**Author:** Project Documentation Team

---

## 1. Executive Summary

**ProServe X** is a Django-based web application that connects customers with local service providers in Pakistan. It serves as a digital marketplace for 6 service categories — Carpenters, Electricians, Painters, Sweepers, Plumbers, and Gardeners. The platform enables provider discovery, online booking, payment tracking, and customer reviews.

Built with Python 3.13 and Django 6.0.5, ProServe X follows the Model-View-Template (MVT) architecture with a SQLite database and a responsive Bootstrap 5 frontend. The system supports two user roles (Customer and Provider) with role-specific dashboards and access controls.

---

## 2. Technology Stack

| Layer | Technology |
|-------|-----------|
| **Backend** | Python 3.13, Django 6.0.5 |
| **Database** | SQLite 3 (development), upgradeable to PostgreSQL (production) |
| **User Model** | Custom `AbstractUser` (`accounts.CustomUser`) with role field |
| **Frontend** | HTML5, CSS3, JavaScript, Bootstrap 5.3, Bootstrap Icons, AOS 2.3 |
| **Forms** | Django ModelForms, `django-crispy-forms` (Bootstrap 4 pack) |
| **Media** | Pillow (image processing), local filesystem storage |
| **Static Assets** | Custom CSS (592 lines), custom JS (12 lines) |
| **Server** | Django development server on port 8080 |

---

## 3. Architecture Overview

### 3.1 Application Structure

The project is organized into 5 Django apps:

| App | Purpose |
|-----|---------|
| `core` | Homepage, search functionality, global context processor |
| `accounts` | User registration, authentication, role management, dashboards |
| `providers` | Service provider profiles, skills, work samples, availability |
| `bookings` | Booking lifecycle, payment processing |
| `reviews` | Customer ratings and feedback |

### 3.2 Data Flow

```
User → Browser → URL Router → View → Model ↔ Database
                                  ↓
                              Template → HTML Response
```

### 3.3 Request Flow

```
Registration → Login → Search Providers → View Profile
    → Book Service → Provider Accepts → Service Completed → Customer Reviews
```

### 3.4 URL Routing

| Prefix | App | Routes |
|--------|-----|--------|
| `/` | `core` | Homepage, search |
| `/accounts/` | `accounts` | Register, login, logout, dashboard |
| `/providers/` | `providers` | Provider list, detail, profile edit |
| `/bookings/` | `bookings` | My bookings, status update, payment, payment panel |
| `/admin/` | `admin` | Django admin interface |

Total: **18 URL routes** across 5 apps.

---

## 4. Database Schema

The system uses **7 database models** with the following entity relationships:

```
CustomUser (1) ──▶ (1) ProviderProfile (if role=provider)
                    ├──▶ (N) Skill
                    ├──▶ (N) WorkImage
                    └──▶ (N) Availability
CustomUser (1) ──▶ (N) Booking (as customer)
CustomUser (1) ──▶ (N) Booking (as provider)
Booking    (1) ──▶ (1) Review
CustomUser (1) ──▶ (N) Review (as customer)
CustomUser (1) ──▶ (N) Review (as provider)
```

### 4.1 Key Models

**CustomUser** — Extends Django's `AbstractUser` with `role` (customer/provider), `phone`, `city` (from 200+ Pakistan cities), and `profile_pic`.

**ProviderProfile** — Links 1:1 to CustomUser. Contains `service_category` (6 types), `description`, `experience_years`, `price_per_hour`, `city`, `is_available`, `average_rating`.

**Booking** — Connects customer and provider. Tracks `service_date`, `service_time`, `status` (pending/accepted/completed/cancelled), `total_amount`, `payment_method` (cash/card/online), `payment_status` (unpaid/paid/refunded).

**Review** — One-to-one with Booking. Contains `rating` (1–5) and `comment` (max 500 chars).

### 4.2 Secondary Models

- **Skill** — Skills associated with a provider profile
- **WorkImage** — Portfolio images uploaded by providers
- **Availability** — Weekly schedule slots (day, start_time, end_time)

---

## 5. Features

### 5.1 Authentication & Roles

- Dual-role registration via radio button selection (Customer / Provider)
- Conditional field: providers select a service category during registration
- Auto-creation of `ProviderProfile` upon provider registration
- `@login_required` decorator on all protected views
- `LOGIN_URL = 'login'`, `LOGIN_REDIRECT_URL = 'dashboard'`, `LOGOUT_REDIRECT_URL = 'home'`

### 5.2 Provider Management

- Profile creation/editing with `ProviderProfileForm`
- Skills, work samples (images with captions), availability schedule
- Average rating auto-calculated from customer reviews
- Toggle availability status

### 5.3 Booking Lifecycle

```
Pending → Accepted → Completed
              ↘
           Cancelled
```

- Customers request bookings with date, time, address, notes, payment method
- Providers accept, complete, or cancel bookings
- Customers make payments (cash/card/online)
- Payment panel with filtering and aggregated totals (earned/pending)

### 5.4 Search & Discovery

- Search by keyword (username), category, and city
- Top 6 providers displayed on homepage, sorted by average rating
- Category-based filtering with navigation pills

### 5.5 Reviews & Ratings

- Customers can review completed bookings
- Rating: 1–5 stars with optional comment
- Average rating recalculated after each review submission
- Displayed on provider detail pages

### 5.6 Dashboards

| Role | Content |
|------|---------|
| **Customer** | Booking history, stats (total bookings, completed, pending, cancelled) |
| **Provider** | Booking requests, earnings summary, reviews received |

---

## 6. UI & Templates

The frontend consists of **13 HTML templates** extending a master `base.html`:

| Template | Purpose |
|----------|---------|
| `base.html` | Master layout: navbar, messages, footer |
| `index.html` | Homepage with hero, category cards, top providers |
| `login.html` | Login form |
| `register.html` | Registration with role selection cards |
| `dashboard.html` | Customer dashboard |
| `provider_dashboard.html` | Provider dashboard |
| `provider_list.html` | Provider listing with category filter |
| `provider_detail.html` | Full profile with booking & review forms |
| `my_bookings.html` | Booking list with role-aware actions |
| `payment.html` | Payment form |
| `payment_panel.html` | Payment history with filters |
| `edit_profile.html` | Provider profile editor |
| `search_results.html` | Search form + results grid |

### Design Highlights

- **Dark theme** with indigo/purple gradient hero section
- **Glassmorphism effects** on cards and navigation
- **Hover animations** on provider and category cards
- **Responsive breakpoints** for mobile, tablet, and desktop
- **Custom scrollbar** styling
- **Role-aware navigation**: conditional "Edit Profile" for providers, role-specific dashboard links

### Context Processor

`core.context_processors.categories_processor` injects all 6 service categories into every template's context as `{{ categories }}`.

---

## 7. Deployment

### Development Setup

```bash
python -m venv venv
venv\Scripts\activate
pip install django crispy-bootstrap4 pillow
python manage.py migrate
python manage.py runserver 8080
```

### Production Checklist

- [ ] Set `DEBUG = False`
- [ ] Generate a new `SECRET_KEY` (use environment variable)
- [ ] Configure `ALLOWED_HOSTS`
- [ ] Switch from SQLite to PostgreSQL
- [ ] Serve static files via Whitenoise or CDN
- [ ] Serve media files via cloud storage (S3, Cloudinary)
- [ ] Enable HTTPS
- [ ] Run `python manage.py migrate`
- [ ] Run `python manage.py collectstatic`
- [ ] Create superuser for admin access

### Testing

```bash
python manage.py test accounts providers bookings reviews core
```

---

## 8. Summary Statistics

| Category | Count |
|----------|-------|
| Django apps | 5 |
| Models | 7 |
| URL routes | 18 |
| View functions | 12 |
| HTML templates | 13 |
| Service categories | 6 |
| Pakistan cities in choices | 200+ |
| Static CSS | 592 lines |
| Launch scripts | 2 (`.bat`, `.vbs`) |
| Python version | 3.13 |
| Django version | 6.0.5 |

---

## 9. Conclusion

ProServe X is a fully functional service marketplace platform with a clean MVT architecture, role-based access control, and a complete booking-to-payment-to-review workflow. The codebase is well-organized into 5 focused Django apps with clear separation of concerns. The frontend delivers a polished user experience with Bootstrap 5 theming, while the backend provides robust data modeling and form validation.

The platform is ready for production deployment with minor configuration changes (database upgrade, static file serving, HTTPS) and is extensible for additional features such as real-time notifications, admin analytics, or mobile API endpoints.

---

*End of Report*
