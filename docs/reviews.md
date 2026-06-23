# Reviews App

Location: `proservex/reviews/`

Handles customer ratings and feedback for completed services.

## Model: `Review` (`models.py:5`)

| Field | Type | Details |
|-------|------|---------|
| `booking` | OneToOneField (Booking) | `related_name='review'` |
| `customer` | ForeignKey (CustomUser) | `related_name='reviews_given'` |
| `provider` | ForeignKey (CustomUser) | `related_name='reviews_received'` |
| `rating` | PositiveSmallIntegerField | Choices 1-5 |
| `comment` | TextField | Max 500 chars, blank allowed |
| `created_at` | DateTimeField | Auto-set |

## Forms

### `ReviewForm` (`forms.py:4`)

Fields: `rating` (RadioSelect 1-5), `comment` (Textarea).

## View Logic

Reviews are handled within `providers.views.provider_detail` (`proservex/providers/views.py`), not in a separate view within the reviews app.

### Review Submission Flow

1. Customer submits rating + comment in `provider_detail` view
2. View creates a `Review` record
3. `average_rating` on the `ProviderProfile` is recalculated
4. Reviews appear on the provider detail page

## Admin

`Review` model is registered with Django admin.
