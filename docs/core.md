# Core App

Location: `proservex/core/`

Handles the homepage, search functionality, and provides global template context.

## Views

### `home_view` (`views.py:4`)

| Route | Method | Description |
|-------|--------|-------------|
| `/` | GET | Renders homepage with service categories and top 6 providers sorted by `average_rating` descending |

### `search_view` (`views.py:17`)

| Route | Method | Description |
|-------|--------|-------------|
| `/search/` | GET | Searches providers by username, category, and/or city |

Search parameters are passed as query string parameters (`query`, `category`, `city`).

## URLs (`urls.py`)

| Pattern | View Name | URL Name |
|---------|-----------|----------|
| `` | `home_view` | `home` |
| `search/` | `search_view` | `search` |

## Context Processors

### `categories_processor` (`context_processors.py:3`)

Makes the `SERVICE_CATEGORIES` tuple from `providers.models` available to all templates as `categories`.

## Templates Served

| Template | View |
|----------|------|
| `index.html` | `home_view` |
| `search_results.html` | `search_view` |
