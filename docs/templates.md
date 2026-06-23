# Templates

Location: `proservex/templates/`

All templates extend `base.html` and use Bootstrap 5 with AOS animations.

## Template List

| Template | Purpose | View(s) |
|----------|---------|---------|
| `base.html` | Master layout (navbar, messages, footer) | All |
| `index.html` | Homepage (hero, categories, top providers) | `core.views.home_view` |
| `login.html` | Login form (username, password) | `accounts.views.login_view` |
| `register.html` | Registration with role selection cards | `accounts.views.register_view` |
| `dashboard.html` | Customer dashboard (bookings, stats) | `accounts.views.dashboard_view` |
| `provider_dashboard.html` | Provider dashboard (requests, earnings, reviews) | `accounts.views.dashboard_view` |
| `provider_list.html` | Provider listing with category filter | `providers.views.provider_list` |
| `provider_detail.html` | Full provider profile (info, skills, booking, reviews) | `providers.views.provider_detail` |
| `my_bookings.html` | Unified booking list (role-aware actions) | `bookings.views.my_bookings` |
| `payment.html` | Single booking payment page | `bookings.views.make_payment` |
| `payment_panel.html` | Payment history with filters & totals | `bookings.views.payment_panel` |
| `edit_profile.html` | Provider profile edit form | `providers.views.edit_profile` |
| `search_results.html` | Search form + results grid | `core.views.search_view` |

## base.html Sections

- **Navbar**: dark blur effect, responsive, links to all sections
- **Messages**: Django messages framework display area
- **Footer**: site links, social, copyright

## Styling

Custom CSS in `static/css/style.css`:
- CSS custom properties (indigo/purple gradient theme)
- Glassmorphism effects
- Hover animations on cards
- Responsive breakpoints
- Custom scrollbar
