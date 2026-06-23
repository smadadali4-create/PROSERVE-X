# Deployment

## Prerequisites

- Python 3.13+
- pip
- A hosting platform (Render, PythonAnywhere, Railway, etc.)

## Production Settings

Before deploying, update `proservex/proservex/settings.py`:

```python
DEBUG = False
ALLOWED_HOSTS = ['your-domain.com', 'www.your-domain.com']
SECRET_KEY = 'generate-a-new-secret-key'  # Use environment variable
```

Environment variables are recommended for sensitive values:

```python
import os
SECRET_KEY = os.environ.get('DJANGO_SECRET_KEY')
DEBUG = os.environ.get('DJANGO_DEBUG', 'False') == 'True'
```

## Collect Static Files

```bash
python manage.py collectstatic
```

## Static & Media Files

In production, serve static/media files via:
- **Whitenoise** (for static files)
- **Cloud storage** (S3, Cloudinary) for media uploads

Add whitenoise to `MIDDLEWARE`:
```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'whitenoise.middleware.WhiteNoiseMiddleware',  # after security
    # ...
]
```

## Database

For production, switch from SQLite to PostgreSQL:

```python
import dj_database_url
DATABASES = {
    'default': dj_database_url.config(default=os.environ.get('DATABASE_URL'))
}
```

## Deployment Checklist

- [ ] `DEBUG = False`
- [ ] Generate new `SECRET_KEY`
- [ ] Set `ALLOWED_HOSTS`
- [ ] Configure production database
- [ ] Set up static/media file serving
- [ ] Enable HTTPS
- [ ] Run `python manage.py migrate`
- [ ] Run `python manage.py collectstatic`
- [ ] Create superuser for admin access

## Testing

```bash
python manage.py test accounts providers bookings reviews core
```
