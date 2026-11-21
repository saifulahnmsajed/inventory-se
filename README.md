# StockLine — Complete MERN Inventory Management System (Frontend).

> **University Software Engineering Lab Project**  
> Modern, interactive, multi-role Inventory Management System (IMS) and Storefront frontend built with **React.js, Vite, Tailwind CSS, Lucide Icons, and Recharts**.

[![React](https://img.shields.io/badge/React-18.x-61DAFB?logo=react&logoColor=black)](https://react.dev/)
[![Vite](https://img.shields.io/badge/Vite-6.x-646CFF?logo=vite&logoColor=white)](https://vitejs.dev/)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-3.4-38B2AC?logo=tailwind-css&logoColor=white)](https://tailwindcss.com/)
[![LocalStorage](https://img.shields.io/badge/Storage-Browser_LocalStorage_v1-F7DF1E?logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)
[![License](https://img.shields.io/badge/License-MIT-emerald.svg)](LICENSE)

---

## Table of Contents
1. [Project Overview](#project-overview)
2. [Key Highlights & Architectural Decisions](#key-highlights--architectural-decisions)
3. [Technology Stack](#technology-stack)
4. [Demo Accounts & Role Permissions Matrix](#demo-accounts--role-permissions-matrix)
5. [Getting Started & Installation](#getting-started--installation)
6. [Folder Structure](#folder-structure)
7. [System Modules & Feature Catalog](#system-modules--feature-catalog)
8. [Data Layer & LocalStorage State Persistence](#data-layer--localstorage-state-persistence)
9. [MERN Backend Integration Roadmap (Express + MongoDB)](#mern-backend-integration-roadmap-express--mongodb)
10. [Authors & Academic Disclaimer](#authors--academic-disclaimer)

---

## 1. Project Overview

**StockLine** is an end-to-end Inventory Management & Customer Storefront System developed as a Software Engineering Lab capstone. It simulates a complete business operations suite:

- **Back-Office Management:** Product cataloging, category taxonomy, supplier directory, purchase order procurement, point-of-sale invoicing, manual stock ledger adjustments, customer relationship management, and financial reporting.
- **Customer Storefront:** Public e-commerce catalog, real-time stock availability, cart management, checkout with confetti feedback, and order tracking.
- **Full Interactivity:** Zero dead links, zero empty button handlers. Every modal, search filter, column sort, pagination, CSV export, CSV import, dark mode switch, and transaction workflow is fully operational.

> **Note on Backend Status:**  
> This phase represents the complete, fully functioning frontend application. All backend database operations are simulated client-side via a structured **Services Layer** storing state in browser `LocalStorage`. The architecture enables a seamless, drop-in replacement with Express.js REST API endpoints without rewriting UI components.

---

## 2. Key Highlights & Architectural Decisions

* **Atomic Stock Ledger & Double-Entry Tracking:**  
  Stock is never modified arbitrarily. Every increase or decrease generates an immutable movement log (`purchase_in`, `sale_out`, `adjustment`, `order_hold`, `reversal`) with timestamps, quantities, and remaining balance.
* **Single Stock Deduction Policy for Online Orders:**  
  Storefront customer orders deduct stock **once upon admin confirmation** (`pending` -> `confirmed`) and restore stock **once upon cancellation**. The `stockDeducted` boolean flag prevents duplicate deductions.
* **Referential Integrity Guards:**  
  Products bound to completed sales, received purchases, or open customer orders cannot be deleted; users receive contextual warnings explaining active dependencies.
* **Self-Lockout Protection:**  
  Administrators cannot demote their own active role or deactivate their own session user account.
* **Modular Service Layer:**  
  UI components do not call `localStorage` directly. All operations pass through pure JavaScript service modules (`productService`, `salesService`, etc.) returning Promises, mirroring asynchronous REST API calls.

---

## 3. Technology Stack

| Layer | Technology | Purpose |
| :--- | :--- | :--- |
| **Runtime & Bundler** | [Vite 6](https://vitejs.dev/) | Sub-second Hot Module Replacement (HMR) and optimized build bundling. |
| **UI Framework** | [React 18+](https://react.dev/) (Plain JS) | Declarative component UI and Context API state synchronization. |
| **Routing** | [React Router v6](https://reactrouter.com/) | Client-side routing, protected routes, and role-based access gates. |
| **Styling** | [Tailwind CSS v3](https://tailwindcss.com/) | Bespoke emerald/slate palette, responsive layouts, and class-based dark mode. |
| **Icons** | [Lucide React](https://lucide.dev/) | Clean, consistent vector iconography. |
| **Data Visualizations** | [Recharts](https://recharts.org/) | Responsive SVG charts (Sales velocity, Category distribution, Stock levels). |
| **Date Utilities** | [date-fns](https://date-fns.org/) | Timezone-aware date parsing and formatting. |
| **Micro-Animations** | [canvas-confetti](https://www.npmjs.com/package/canvas-confetti) | Visual celebration on storefront order completion. |
| **Print Styling** | Pure CSS (`@media print`) | Format-ready printable invoices and transaction receipts. |

---

## 4. Demo Accounts & Role Permissions Matrix

The login screen (`/login`) includes a **1-Click Demo Login Panel** to immediately authenticate as any role without manual typing:

| Role | Email | Password | Scope & Privileges |
| :--- | :--- | :--- | :--- |
| **Administrator** | `admin@stockline.io` | `admin123` | Full access across all modules, user administration, system settings, and data reset. |
| **Warehouse Manager** | `manager@stockline.io` | `manager123` | Products, inventory ledger, purchases, suppliers, and operational reports. |
| **Cashier / Sales Staff** | `staff@stockline.io` | `staff123` | Point-of-Sale (POS) invoicing, customer directory, sales receipts, and stock balance lookups. |
| **Storefront Customer** | `customer@stockline.io` | `customer123` | Public shop catalog, shopping cart, checkout, and personal order tracking history (`/my-orders`). |

### Role Permissions Matrix

| Module / Feature | Route | Admin | Manager | Staff | Customer |
| :--- | :--- | :---: | :---: | :---: | :---: |
| Dashboard & KPIs | `/dashboard` | Full | Full | View Only | — |
| Products (CRUD & CSV) | `/products` | Full | Full | View Only | — |
| Category Taxonomy | `/categories` | Full | Full | View Only | — |
| Inventory Ledger & Stock Adjust | `/inventory` | Full | Full | View Only | — |
| Supplier Directory | `/suppliers` | Full | Full | — | — |
| Purchase Orders (Procurement) | `/purchases` | Full | Full | — | — |
| Sales & POS Invoicing | `/sales` | Full | View Only | Create & Print | — |
| Customer Directory | `/customers` | Full | Full | Full | — |
| Storefront Orders (Admin View) | `/orders` | Full | Full | View Only | — |
| Reports & Analytics | `/reports` | Full | Full | — | — |
| User Administration | `/users` | Full | — | — | — |
| System Settings & Hard Reset | `/settings` | Full | — | — | — |
| Storefront Catalog & Cart | `/shop`, `/cart` | Public | Public | Public | Public |
| Order Checkout & History | `/checkout`, `/my-orders` | Yes | Yes | Yes | Yes |

---

## 5. Getting Started & Installation

### Prerequisites
- [Node.js](https://nodejs.org/) version 18.0.0 or higher
- `npm` (bundled with Node.js) or `pnpm`/`yarn`

### Installation Steps

1. **Clone the repository:**
   ```bash
   git clone <repository-url>
   cd "inventory se"
   ```

2. **Install project dependencies:**
   ```bash
   npm install
   ```

3. **Start the development server:**
   ```bash
   npm run dev
   ```
   Open your browser and navigate to `http://localhost:5173`.

4. **Build for production:**
   ```bash
   npm run build
   ```
   Assets will be compiled into the `dist/` directory ready for deployment on Vercel, Netlify, or any static web host.

5. **Preview production build locally:**
   ```bash
   npm run preview
   ```

---

## 6. Folder Structure

```
inventory se/
├── index.html                   # HTML entry point with Inter font & SVG favicon
├── package.json                 # Project dependencies & scripts
├── postcss.config.js            # PostCSS configuration for Tailwind
├── tailwind.config.js           # Tailwind configuration (dark mode, brand colors)
├── vite.config.js               # Vite build configuration
├── src/
│   ├── main.jsx                 # Application bootstrapping & router mounts
│   ├── App.jsx                  # Route definitions & protected route guards
│   ├── index.css                # Global styles, scrollbar styling & print rules
│   │
│   ├── context/                 # Reactive state contexts
│   │   ├── AuthContext.jsx      # Active user session, role matrix, 1-click switch
│   │   └── AppDataContext.jsx   # Synchronized reactive state, cart, toasts, confirm modal
│   │
│   ├── services/                # Asynchronous data services (API-ready abstraction)
│   │   ├── storageService.js    # LocalStorage versioning, seeding, hard reset
│   │   ├── authService.js       # Auth tokens, credentials validation, password simulation
│   │   ├── productService.js    # Product CRUD, SKU uniqueness, CSV import
│   │   ├── inventoryService.js  # Central stock balance & movement audit logging
│   │   ├── salesService.js      # Sales creation, stock validation, reversal
│   │   ├── purchaseService.js   # PO workflow, automated stock intake on receive
│   │   ├── orderService.js      # Storefront orders & single stock deduction logic
│   │   ├── categoryService.js   # Category CRUD with dependency checks
│   │   ├── supplierService.js   # Supplier CRUD with linked purchase checks
│   │   ├── customerService.js   # Customer directory with sales link checks
│   │   ├── reportService.js     # Valuation, sales aggregate, low stock reports
│   │   ├── userService.js       # User accounts & admin self-lockout prevention
│   │   └── settingsService.js   # Business profile, currency, reorder threshold
│   │
│   ├── utils/                   # Reusable calculation & formatting utilities
│   │   ├── formatters.js        # Currency (৳ BDT), dates, stock badges, SKU generator
│   │   ├── calculations.js      # Totals, tax, margin, Recharts aggregations
│   │   └── csv.js               # RFC-4180 compliant CSV export and import parser
│   │
│   ├── data/
│   │   └── seedData.js          # Realistic starter dataset (14 products, 6 categories,
│   │                            # 5 suppliers, 10 customers, purchases, sales, orders)
│   │
│   ├── layouts/                 # Page wrappers
│   │   ├── PublicLayout.jsx     # Header navbar, breadcrumbs, public footer
│   │   └── DashboardLayout.jsx  # Collapsible sidebar, topbar, mobile drawer
│   │
│   ├── routes/
│   │   └── ProtectedRoute.jsx   # Authentication & role-based permission gatekeeper
│   │
│   ├── components/              # Modular UI components
│   │   ├── common/              # Buttons, Badges, Cards, Inputs, Modals, Pagination
│   │   ├── navigation/          # Navbar, Sidebar, TopNav, Breadcrumbs, GlobalSearch
│   │   ├── dashboard/           # KPICards, QuickActions, Recharts charts
│   │   └── sales/               # PrintableReceiptModal (@media print formatted)
│   │
│   └── pages/                   # Application view screens
│       ├── public/              # LandingPage, Features, Privacy, Terms
│       ├── auth/                # Login, Register, ForgotPassword, AccessDenied
│       ├── shop/                # ShopPage, ProductDetail, Cart, Checkout, MyOrders
│       ├── dashboard/           # DashboardOverviewPage (KPIs, Charts, Activity)
│       ├── products/            # ProductListPage, ProductFormPage, ProductDetailPage
│       ├── categories/          # CategoryListPage
│       ├── suppliers/           # SupplierListPage, SupplierDetailPage
│       ├── customers/           # CustomerListPage, CustomerDetailPage
│       ├── purchases/           # PurchaseListPage, PurchaseFormPage, PurchaseDetailPage
│       ├── sales/               # SalesListPage, SalesFormPage, SaleDetailPage
│       ├── inventory/           # InventoryPage (Stock balance & movement ledger)
│       ├── orders/              # OrderListPage (Admin view), AdminOrderDetailPage
│       ├── reports/             # ReportsPage (Valuation, Sales, Low stock, CSV)
│       ├── users/               # UserManagementPage
│       └── settings/            # SettingsPage, ProfilePage
```

---

## 7. System Modules & Feature Catalog

### 7.1 Public & Storefront Experience
- **Landing Page (`/`):** Hero section with interactive CTA, live feature showcase, dynamic statistics preview, and customer testimonials.
- **Storefront Catalog (`/shop`):** Category pills, real-time search, price range filter, in-stock/out-of-stock badges, quick "Add to Cart".
- **Cart & Checkout (`/cart`, `/checkout`):** Item quantity increments, line total computation, delivery fee calculation, shipping details form, and celebratory confetti upon order placement.
- **Order Tracking (`/my-orders`):** Customers view their personal order history and live fulfillment stages (`pending` -> `confirmed` -> `shipped` -> `delivered`).

### 7.2 Core Inventory & Back-Office
- **Real-Time Analytics Dashboard (`/dashboard`):**
  - 6 Key Performance Indicator (KPI) cards: Total Revenue, Inventory Valuation, Products In Catalog, Low Stock Alerts, Total Customers, Pending Orders.
  - Recharts visualizations: Revenue Velocity Line Chart, Inventory Value by Category Donut Chart, Stock Level Distribution Bar Chart.
  - Quick action shortcuts and recent activity feed.
- **Product Management (`/products`):**
  - High-performance data table with search, category filtering, stock status filtering, and column sorting.
  - Complete CRUD with automatic SKU generation, barcode entry, cost price, selling price, reorder alert threshold, and supplier link.
  - Bulk CSV Export and CSV Import modal with file upload simulation.
- **Procurement & Purchase Orders (`/purchases`):**
  - Multi-item Purchase Order creation with supplier selection and cost calculation.
  - Status lifecycle: `draft` -> `received` -> `cancelled`.
  - Automatic inventory stock intake upon marking an order as `received`.
- **Point of Sale (POS) & Sales Invoicing (`/sales`):**
  - Quick sale billing interface with customer lookup, item search, quantity guards (cannot sell more than available on-hand stock), discount rates, and tax calculations.
  - Instant generation of printable, professional tax receipts / invoices with browser print trigger (`@media print`).
- **Stock Movements & Audit Ledger (`/inventory`):**
  - Real-time stock status table (In Stock, Low Stock, Out of Stock).
  - Manual Stock Adjustment modal (Add stock, Reduce stock, Set explicit balance) with mandatory audit reason logging.
  - Complete chronological movement history (`purchase_in`, `sale_out`, `adjustment`).
- **Reports & Business Intelligence (`/reports`):**
  - Inventory Valuation Breakdown by Category.
  - Sales Revenue & Profit Margin Summary.
  - Procurement Expenditure Summary.
  - Low Stock Reorder List with 1-click CSV download.

---

## 8. Data Layer & LocalStorage State Persistence

All demo data is stored under versioned LocalStorage keys prefixed with `stockline_demo_v1_`:

| LocalStorage Key | Description | Initial Count |
| :--- | :--- | :---: |
| `stockline_demo_v1_products` | Product catalog with prices, SKUs, and stock levels | 14 |
| `stockline_demo_v1_categories` | Product category taxonomy | 6 |
| `stockline_demo_v1_suppliers` | Vendor directory with contact records | 5 |
| `stockline_demo_v1_customers` | Retail & wholesale customer database | 10 |
| `stockline_demo_v1_purchases` | Purchase orders & receiving status | 3 |
| `stockline_demo_v1_sales` | POS sales invoices & payment methods | 3 |
| `stockline_demo_v1_movements` | Immutable stock movement ledger | 6 |
| `stockline_demo_v1_orders` | Storefront customer online orders | 3 |
| `stockline_demo_v1_users` | Demo accounts with hashed role credentials | 4 |
| `stockline_demo_v1_settings` | Business profile, currency, and reorder levels | 1 |

### Resetting Demo Data
To reset all records back to their pristine factory state:
1. Navigate to **Settings** (`/settings`) from the admin sidebar.
2. Scroll to the **Danger Zone** at the bottom.
3. Click **Reset All Demo Data** and confirm the prompt.
4. *Alternative:* Open browser DevTools Console (`F12`) and run:
   ```javascript
   localStorage.clear();
   location.reload();
   ```

---

## 9. MERN Backend Integration Roadmap (Express + MongoDB)

StockLine was purposefully built with an asynchronous service layer to make migrating from `LocalStorage` to a real **Node.js + Express.js + MongoDB** backend as straightforward as swapping internal service implementations.

### 9.1 Architecture Transition Diagram

```
CURRENT (Frontend-Only Prototype):
[React UI Components]
       │
       ▼
[React Context (AppDataContext / AuthContext)]
       │
       ▼
[Service Layer (productService.js, salesService.js, etc.)]
       │
       ▼
[storageService.js] ──► Browser LocalStorage (`stockline_demo_v1_*`)


FUTURE (Full MERN Stack):
[React UI Components]
       │
       ▼
[React Context (AppDataContext / AuthContext)]
       │
       ▼
[Service Layer (productService.js, salesService.js, etc.)]
       │ (axios / fetch with JWT Bearer Token)
       ▼
[Express.js REST API Server] (`http://localhost:5000/api/...`)
       │
       ▼
[Mongoose ODM Models]
       │
       ▼
[MongoDB Database] (`mongodb://localhost:27017/stockline_db`)
```

### 9.2 Express.js REST API Endpoint Mapping

| Frontend Service Method | HTTP Verb | Express Route | MongoDB / Mongoose Operation |
| :--- | :---: | :--- | :--- |
| `authService.login()` | `POST` | `/api/auth/login` | Compare bcrypt password hash; sign JWT token |
| `productService.getProducts()` | `GET` | `/api/products` | `Product.find().populate('category supplier')` |
| `productService.createProduct()` | `POST` | `/api/products` | `new Product(req.body).save()` |
| `productService.updateProduct()` | `PUT` | `/api/products/:id` | `Product.findByIdAndUpdate(id, req.body)` |
| `salesService.createSale()` | `POST` | `/api/sales` | MongoDB Transaction: create Sale + deduct Product stock + create Movement |
| `purchaseService.receivePurchase()` | `PUT` | `/api/purchases/:id/receive`| MongoDB Transaction: update PO + increment stock + create Movement |
| `orderService.updateOrderStatus()` | `PUT` | `/api/orders/:id/status` | Update status; handle conditional stock deduction if status becomes `confirmed` |
| `reportService.getInventoryReport()` | `GET` | `/api/reports/inventory` | `Product.aggregate([...])` valuation pipeline |

### 9.3 Recommended MongoDB Mongoose Schemas

#### 1. Product Schema (`models/Product.js`)
```javascript
const mongoose = require('mongoose');

const productSchema = new mongoose.Schema({
  name: { type: String, required: true, trim: true },
  sku: { type: String, required: true, unique: true, uppercase: true },
  barcode: { type: String, default: '' },
  category: { type: mongoose.Schema.Types.ObjectId, ref: 'Category', required: true },
  supplier: { type: mongoose.Schema.Types.ObjectId, ref: 'Supplier' },
  costPrice: { type: Number, required: true, min: 0 },
  sellingPrice: { type: Number, required: true, min: 0 },
  stock: { type: Number, required: true, default: 0, min: 0 },
  minStockLevel: { type: Number, default: 5 },
  unit: { type: String, default: 'pcs' },
  status: { type: String, enum: ['active', 'inactive'], default: 'active' },
  description: { type: String, default: '' },
  image: { type: String, default: '' }
}, { timestamps: true });

module.exports = mongoose.model('Product', productSchema);
```

#### 2. Stock Movement Schema (`models/StockMovement.js`)
```javascript
const mongoose = require('mongoose');

const movementSchema = new mongoose.Schema({
  productId: { type: mongoose.Schema.Types.ObjectId, ref: 'Product', required: true },
  type: { 
    type: String, 
    enum: ['purchase_in', 'sale_out', 'adjustment', 'order_hold', 'reversal'], 
    required: true 
  },
  quantity: { type: Number, required: true }, // Positive for additions, negative for deductions
  balanceAfter: { type: Number, required: true },
  reference: { type: String, required: true }, // e.g. "INV-2026-0001", "PO-2026-0002"
  reason: { type: String, default: '' },
  userId: { type: mongoose.Schema.Types.ObjectId, ref: 'User' }
}, { timestamps: true });

module.exports = mongoose.model('StockMovement', movementSchema);
```

### 9.4 How to Migrate Service Code
To transition any service (e.g. `productService.js`), simply change the implementation to use `fetch` or `axios`:

```javascript
// BEFORE (LocalStorage):
export const getProducts = async () => {
  return storageService.get(STORAGE_KEYS.PRODUCTS) || [];
};

// AFTER (Express REST API):
import axios from 'axios';
const API_URL = import.meta.env.VITE_API_URL || 'http://localhost:5000/api';

export const getProducts = async () => {
  const response = await axios.get(`${API_URL}/products`, {
    headers: { Authorization: `Bearer ${localStorage.getItem('token')}` }
  });
  return response.data;
};
```
Because all frontend UI components already `await` service promises, **no page or component code needs to change** during backend migration!

---

## 10. Authors & Academic Disclaimer

- **Course:** Software Engineering Lab (CSE)
- **Project Name:** StockLine Inventory Management System
- **Year / Semester:** 2026

*Academic Notice: This software is developed for educational purposes to demonstrate software engineering best practices including component modularity, state synchronization, role-based access control, responsive design, and robust data flow architecture.*
# inventory-se
