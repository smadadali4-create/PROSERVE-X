# Bookings App

Location: `proservex/bookings/`

Manages service bookings, status transitions, and payment processing.

## Constants

### `STATUS_CHOICES` (`models.py:4`)

```python
[('pending', 'Pending'), ('accepted', 'Accepted'),
 ('completed', 'Completed'), ('cancelled', 'Cancelled')]
```

### `PAYMENT_METHOD_CHOICES` (`models.py:5`)

```python
[('cash', 'Cash'), ('card', 'Card'), ('online', 'Online Transfer')]
```

### `PAYMENT_STATUS_CHOICES` (`models.py:6`)

```python
[('unpaid', 'Unpaid'), ('paid', 'Paid'), ('refunded', 'Refunded')]
```

## Model: `Booking` (`models.py:8`)

| Field | Type | Details |
|-------|------|---------|
| `customer` | ForeignKey (CustomUser) | `related_name='bookings_as_customer'` |
| `provider` | ForeignKey (CustomUser) | `related_name='bookings_as_provider'` |
| `service_date` | DateField | Scheduled service date |
| `service_time` | TimeField | Scheduled service time |
| `status` | CharField | Default: `pending` |
| `address` | TextField | Service address |
| `notes` | TextField | Optional, blank allowed |
| `total_amount` | DecimalField | Default: 0 |
| `payment_method` | CharField | Default: `cash` |
| `payment_status` | CharField | Default: `unpaid` |
| `created_at` | DateTimeField | Auto-set |
| `updated_at` | DateTimeField | Auto-updated |

## Forms

### `BookingForm` (`forms.py:4`)

Fields: `service_date` (date picker), `service_time` (time picker), `address` (textarea), `notes` (textarea), `payment_method` (select).

## Views

| View | Route | Method | Description |
|------|-------|--------|-------------|
| `my_bookings` | `/` | GET | Lists bookings for current user (role-aware) |
| `update_booking_status` | `/update/<int:pk>/<str:status>/` | GET | Provider: accept/complete/cancel booking |
| `make_payment` | `/pay/<int:pk>/` | POST | Customer: mark booking as paid |
| `payment_panel` | `/payments/` | GET | Transaction history with filtering |

### Role-Aware Booking List

- **Provider**: sees incoming booking requests
- **Customer**: sees their outgoing bookings

### Payment Panel

Features: filter by payment status, totals for earned and pending amounts.

## URLs (`urls.py`)

| Pattern | View Name | URL Name |
|---------|-----------|----------|
| `` | `my_bookings` | `my_bookings` |
| `update/<int:pk>/<str:status>/` | `update_booking_status` | (unnamed) |
| `pay/<int:pk>/` | `make_payment` | `make_payment` |
| `payments/` | `payment_panel` | `payment_panel` |

## Booking Lifecycle

```
Pending → Accepted → Completed
                ↘
             Cancelled
```
