# Database Models

## Entity Relationship

```
CustomUser
    ├── ProviderProfile (1:1)
    │       ├── Skill (1:N)
    │       ├── WorkImage (1:N)
    │       └── Availability (1:N)
    ├── Booking (as customer)
    └── Booking (as provider)
            └── Review (1:1)
```

## `accounts.CustomUser`

| Column | Type | Constraints |
|--------|------|-------------|
| id | AutoField | PK |
| password | VARCHAR(128) | |
| last_login | DateTime | nullable |
| is_superuser | bool | |
| username | VARCHAR(150) | unique |
| first_name | VARCHAR(150) | |
| last_name | VARCHAR(150) | |
| email | VARCHAR(254) | |
| is_staff | bool | |
| is_active | bool | |
| date_joined | DateTime | |
| role | VARCHAR(20) | customer / provider |
| phone | VARCHAR(15) | |
| city | VARCHAR(100) | |
| profile_pic | VARCHAR(100) | file path |

## `providers.ProviderProfile`

| Column | Type | Constraints |
|--------|------|-------------|
| id | AutoField | PK |
| user_id | Integer | FK → CustomUser, unique |
| service_category | VARCHAR(50) | nullable |
| description | TEXT | max 1000 |
| experience_years | Integer | unsigned |
| price_per_hour | Decimal | |
| city | VARCHAR(100) | |
| is_available | bool | default True |
| created_at | DateTime | |
| average_rating | Decimal(3,2) | default 0 |

## `providers.Skill`

| Column | Type | Constraints |
|--------|------|-------------|
| id | AutoField | PK |
| profile_id | Integer | FK → ProviderProfile |
| name | VARCHAR(100) | |

## `providers.WorkImage`

| Column | Type | Constraints |
|--------|------|-------------|
| id | AutoField | PK |
| profile_id | Integer | FK → ProviderProfile |
| image | VARCHAR(100) | file path |
| caption | VARCHAR(200) | |

## `providers.Availability`

| Column | Type | Constraints |
|--------|------|-------------|
| id | AutoField | PK |
| profile_id | Integer | FK → ProviderProfile |
| day | VARCHAR(20) | |
| start_time | Time | |
| end_time | Time | |

## `bookings.Booking`

| Column | Type | Constraints |
|--------|------|-------------|
| id | AutoField | PK |
| customer_id | Integer | FK → CustomUser |
| provider_id | Integer | FK → CustomUser |
| service_date | Date | |
| service_time | Time | |
| status | VARCHAR(20) | pending/accepted/completed/cancelled |
| address | TEXT | |
| notes | TEXT | blank |
| total_amount | Decimal | default 0 |
| payment_method | VARCHAR(20) | cash/card/online |
| payment_status | VARCHAR(20) | unpaid/paid/refunded |
| created_at | DateTime | |
| updated_at | DateTime | |

## `reviews.Review`

| Column | Type | Constraints |
|--------|------|-------------|
| id | AutoField | PK |
| booking_id | Integer | FK → Booking, unique |
| customer_id | Integer | FK → CustomUser |
| provider_id | Integer | FK → CustomUser |
| rating | SmallInteger | 1-5 |
| comment | TEXT | max 500, blank |
| created_at | DateTime | |
