# BizFlow — Business Manager

BizFlow is a lightweight, single-file business management app for small businesses. It runs entirely in the browser: no backend, no build step, no dependencies. Open `Bizflow.html` and start managing customers, products, orders, and stock.

## Features

- **Dashboard**: Revenue, order count, customer count, and low-stock alerts at a glance, plus a sales overview chart and a recent orders table.
- **Customers**: Add and edit customers (name, phone, email). Tracks each customer's order count and total spend. Includes live search.
- **Products**: Add and edit products with name, SKU, category, price, cost, stock, and a low-stock threshold. Includes live search and stock-level progress bars.
- **Orders**: Create orders by selecting a customer, product, quantity, status, and payment state. Orders are auto-numbered (`ORD-1001`, `ORD-1002`, ...) and searchable.
- **Inventory**: Stock levels for every product, with items at or below their threshold highlighted.
- **Analytics**: Revenue from completed orders, completed order count, average order value, and total inventory value (at cost).
- **Settings**: Set your business name and display currency (INR, USD, EUR, GBP).
- **Responsive UI**: Dark theme, with a collapsible sidebar on mobile.

## Getting Started

https://priyanshjain08.github.io/BizFlow/

## How It Works

### Data storage

All data is saved in your browser's `localStorage` under the key `bizflow2`. This means:

- Data persists across page reloads and browser restarts.
- Data is **private to your browser and device** and is not synced anywhere.
- Clearing site data or using a private/incognito window will lose or hide your data.

On first launch, the app loads sample data (3 customers, 4 products, 4 orders) so you can explore right away. To reset to the sample data, run this in the browser console and reload:

```js
localStorage.removeItem("bizflow2");
```

### Business rules

| Rule | Behavior |
| --- | --- |
| Low stock | A product is "low stock" when `stock <= low-stock threshold` |
| Stock deduction | Stock is reduced only when an order is created with status **Completed** |
| Customer stats | A customer's order count and total spent update only for **Completed** orders |
| Order validation | Quantity must be at least 1 and cannot exceed available stock |
| Required fields | Customers need a name; products need a name and SKU |
| Dashboard revenue | Sum of all orders except those with status "Cancelled" |
| Analytics revenue | Sum of **Completed** orders only |
| Inventory value | Sum of `cost × stock` across all products |

### Order statuses and payments

- **Status**: Pending, Processing, Completed
- **Payment**: Unpaid, Paid, Partially paid

## Project Structure

Everything lives in one file:

```
Bizflow.html
├── <style>   Theme, layout, and responsive CSS
├── <body>    App shell (sidebar, top bar, content area, modal, toast)
└── <script>  State, localStorage persistence, page renderers, and modals
```

Key pieces of the script:

- `state`: the in-memory data object (`customers`, `products`, `orders`, `settings`)
- `save()`: writes `state` to `localStorage`
- `setPage()`: simple client-side router for the sidebar pages
- `customerModal` / `productModal` / `orderModal`: add and edit forms
- `esc()`: HTML-escapes user input before rendering

## Customizing

- **Seed data**: Edit the `initial` object in the script.
- **Theme colors**: Change the CSS variables in `:root` (`--accent`, `--bg`, `--panel`, etc.).
- **Currencies**: Add options to the currency `<select>` in `settings()`. Formatting uses `Intl.NumberFormat` with the `en-IN` locale.
- **Storage key**: Change `bizflow2` if you want a fresh data store or to run multiple instances on the same origin.

## Known Limitations

- The dashboard "Sales overview" chart shows static demo values, not real sales data.
- Orders contain a single product line each, and orders cannot be edited or deleted once created.
- Customers and products can be edited but not deleted.
- The "Cancelled" status is recognized in revenue calculations but cannot be selected when creating an order.
- Orders reference customers by name, so renaming a customer does not update their existing orders.
- The currency dropdown on the Settings page always displays INR on load, even if a different currency was saved (the saved value is still applied across the app).
- There is no data export/import, authentication, or multi-user support.

## Ideas for Future Work

- Real sales chart driven by order dates
- Multi-item orders, order editing, and cancellation with stock restoration
- Delete actions for customers and products
- CSV / JSON export and import for backups
- Optional backend sync for multi-device use

## License

Add a license of your choice (e.g., MIT) before distributing.
