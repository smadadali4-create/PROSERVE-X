# Providers App

Location: `proservex/providers/`

Manages service provider profiles, skills, work images, and availability.

## Constants

### `SERVICE_CATEGORIES` (`models.py:5`)

```python
[
    ('carpenter', 'Carpenter'),
    ('electrician', 'Electrician'),
    ('painter', 'Painter'),
    ('sweeper', 'Sweeper'),
    ('plumber', 'Plumber'),
    ('gardener', 'Gardener'),
]
```

## Models

### `ProviderProfile` (`models.py:9`)

| Field | Type | Details |
|-------|------|---------|
| `user` | OneToOneField (CustomUser) | Links to user account |
| `service_category` | CharField | From SERVICE_CATEGORIES, blank allowed |
| `description` | TextField | Max 1000 chars |
| `experience_years` | PositiveIntegerField | Years of experience |
| `price_per_hour` | DecimalField | Service rate |
| `city` | CharField | Service location |
| `is_available` | BooleanField | Default: True |
| `created_at` | DateTimeField | Auto-set on creation |
| `average_rating` | DecimalField | 3 digits, 2 decimal places, default 0 |

### `Skill` (`models.py:28`)

| Field | Type |
|-------|------|
| `profile` | ForeignKey (ProviderProfile) |
| `name` | CharField |

### `WorkImage` (`models.py:33`)

| Field | Type |
|-------|------|
| `profile` | ForeignKey (ProviderProfile) |
| `image` | ImageField |
| `caption` | CharField |

### `Availability` (`models.py:38`)

| Field | Type |
|-------|------|
| `profile` | ForeignKey (ProviderProfile) |
| `day` | CharField |
| `start_time` | TimeField |
| `end_time` | TimeField |

## Views

| View | Route | Method | Description |
|------|-------|--------|-------------|
| `provider_list` | `/` or `/category/<str:category>/` | GET | Lists providers, filters by category |
| `provider_detail` | `/<int:pk>/` | GET/POST | Shows profile, handles booking & review submission |
| `edit_profile` | `/edit/` | GET/POST | Creates/edits provider profile |

### `provider_detail` Behavior

- GET: displays provider info, reviews, booking form, review form
- POST with booking: creates a booking (customer only)
- POST with review: creates a review, recalculates `average_rating`

## Forms

### `ProviderProfileForm` (`forms.py:4`)

Fields: `service_category`, `description`, `experience_years`, `price_per_hour`, `city`, `is_available`. Textarea widget for description.

## URLs (`urls.py`)

| Pattern | View Name | URL Name |
|---------|-----------|----------|
| `` | `provider_list` | `provider_list` |
| `category/<str:category>/` | `provider_list` | `provider_by_category` |
| `<int:pk>/` | `provider_detail` | `provider_detail` |
| `edit/` | `edit_profile` | `edit_profile` |
