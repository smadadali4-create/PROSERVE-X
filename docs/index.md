# ProServe X

ProServe X is a Django 6.0 web application that connects customers with local service providers (carpenters, electricians, painters, sweepers, plumbers, gardeners).

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Backend | Python 3.13, Django 6.0.5 |
| Database | SQLite3 |
| Frontend | HTML5, CSS3, JavaScript, Bootstrap 5.3, AOS 2.3 |
| Other | django-crispy-forms, Pillow |

## Project Structure

```
proservex/
├── accounts/          # User authentication & roles
├── bookings/          # Booking & payment system
├── core/              # Homepage & search
├── providers/         # Service provider profiles
├── reviews/           # Ratings & feedback
├── proservex/         # Project settings & configuration
├── templates/         # HTML templates
├── static/            # Static assets (CSS, JS)
├── media/             # User-uploaded images
├── staticfiles/       # Collected static files
├── manage.py          # Django management entry point
└── db.sqlite3         # SQLite database
```

## Setup

### Prerequisites

- Python 3.13+
- pip

### Installation

```bash
# Clone the repository
cd proservex

# Create and activate virtual environment
python -m venv venv
venv\Scripts\activate   # Windows
# source venv/bin/activate   # macOS/Linux

# Install dependencies
pip install django crispy-bootstrap4 pillow

# Run migrations
python manage.py migrate

# Create superuser
python manage.py createsuperuser

# Start development server
python manage.py runserver
```

### Key Configuration

| Setting | Value |
|---------|-------|
| `AUTH_USER_MODEL` | `accounts.CustomUser` |
| `LOGIN_REDIRECT_URL` | `dashboard` |
| `LOGOUT_REDIRECT_URL` | `home` |
| `LOGIN_URL` | `login` |
| `CRISPY_TEMPLATE_PACK` | `bootstrap4` |

## Features

- Dual-role authentication (Customer / Provider)
- 6 service categories
- Provider profiles with skills, work samples, and availability
- Booking system with lifecycle management
- Payment tracking (cash, card, online)
- Rating & review system
- Customer & Provider dashboards
- Search by category, name, city
- Django admin panel
- Responsive UI with Bootstrap 5
