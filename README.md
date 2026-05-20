<div align="center">

# ✨ SHEGLOW

### *Your ultimate beauty destination — Skincare & Makeup, all in one place.*

[![Python](https://img.shields.io/badge/Python-3.10+-blue?style=flat-square&logo=python)](https://python.org)
[![Flask](https://img.shields.io/badge/Flask-3.x-black?style=flat-square&logo=flask)](https://flask.palletsprojects.com)
[![SQLAlchemy](https://img.shields.io/badge/SQLAlchemy-ORM-red?style=flat-square)](https://www.sqlalchemy.org)
[![SQL Server](https://img.shields.io/badge/Database-SQL%20Server-blue?style=flat-square&logo=microsoftsqlserver)](https://www.microsoft.com/en-us/sql-server)
[![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE)

</div>

---

## 📌 Overview

**SheGlow** is a full-stack e-commerce web application built with **Flask** for beauty enthusiasts. The platform offers a seamless shopping experience for **skincare** and **makeup** products — from browsing and wishlisting to checkout and order tracking. It includes a complete admin panel for product and order management, along with customer-facing features like reviews, recently viewed items, and multi-currency display.

---

## 🌟 Features

### 🛍️ Shopping Experience
- **Product Catalog** — Browse all products or filter by category (Skincare / Makeup)
- **Sorting** — Sort products by Name, Price, or Rating (ascending/descending)
- **Search** — Live autocomplete search with instant suggestions
- **Product Details** — Full product page with description, brand, rating, stock, and photo
- **Recently Viewed** — Tracks the last 5 products a logged-in user viewed

### 🛒 Cart & Checkout
- Add, update, or remove items from cart
- Stock quantity validation before adding to cart
- Checkout with name, phone, and delivery address
- Automatic order creation with order ID on completion
- Fixed shipping cost applied at checkout

### ❤️ Wishlist
- Toggle products in/out of wishlist with a single click (AJAX)
- View full wishlist page
- Persisted per-user in the database

### 👤 User Accounts
- Registration with full validation (name, email, phone format, password match)
- Login / Logout with session management (7-day persistent session)
- Forgot Password — reset via email lookup
- Dashboard — view profile info, order history, and update delivery address

### 📦 Order Management
- Order tracking by order ID (real-time status via API endpoint)
- Auto-complete orders after 2 days from order date
- Return & exchange request sends a confirmation email to the customer

### 🔐 Admin Panel
- Dedicated admin route `/admin`
- Add new products via form (ProductForm with full validation)
- View all orders and manage product inventory

### 📄 Informational Pages
- About Us
- Privacy Policy
- Shipping Policy
- Refund Policy (with return/exchange email request form)

---

## 🗂️ Project Structure

```
SheGlow/
│
├── can.py                  # Main application — models, forms, and all routes
│
├── static/
│   ├── styles.css          # Global stylesheet
│   ├── scripts.js          # Frontend JS (AJAX for wishlist, search suggestions)
│   └── images/             # Product images, banners, brand assets, flags
│
├── templates/
│   ├── base.html           # Base layout with navbar, promo bar, footer
│   ├── index.html          # Homepage with hero banners and featured products
│   ├── product.html        # Full product listing page
│   ├── product_details.html# Single product view with reviews
│   ├── makeup.html         # Category-filtered product page
│   ├── search.html         # Search results page
│   ├── add_to_cart_page.html  # Shopping cart
│   ├── checkout.html       # Checkout page
│   ├── dashboard.html      # User account dashboard
│   ├── wishlist.html       # User wishlist
│   ├── order-tracking.html # Order tracking page
│   ├── admin.html          # Admin panel
│   ├── login.html          # Login page
│   ├── create.html         # Registration page
│   ├── forgot_password.html# Password reset page
│   ├── about.html          # About us
│   ├── privacy-policy.html # Privacy policy
│   ├── shipping-policy.html# Shipping policy
│   └── refund-policy.html  # Refund & return policy
│
└── project_new.bak         # SQL Server database backup
```

---

## 🧱 Database Models

| Model            | Table              | Description                                      |
|------------------|--------------------|--------------------------------------------------|
| `Client`         | `client`           | Registered users with address and phone          |
| `Admin`          | `adminn`           | Admin accounts for managing the store            |
| `Product`        | `Product`          | Products with name, price, stock, brand, category|
| `Order`          | `orders`           | Customer orders with status and date             |
| `OrderDetails`   | `OrderDetails`     | Line items per order (product, qty, price)       |
| `CartItem`       | `cart_items`       | Active cart items per user                       |
| `Wishlist`       | `wishlist`         | Saved products per user                          |
| `Review`         | `reviews`          | Product reviews with rating and timestamp        |
| `RecentlyViewed` | `recently_viewed`  | Last 5 products viewed per user                  |

---

## ⚙️ Installation & Setup

### Prerequisites

- Python 3.10+
- Microsoft SQL Server (local instance)
- ODBC Driver 17 for SQL Server
- A Gmail account (for return/exchange email notifications)

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/sheglow.git
cd sheglow
```

### 2. Create & Activate a Virtual Environment

```bash
python -m venv venv

# Windows
venv\Scripts\activate

# macOS/Linux
source venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install flask flask_sqlalchemy flask_wtf wtforms pyodbc sqlalchemy
```

### 4. Restore the Database

Restore `project_new.bak` to your local SQL Server instance using SQL Server Management Studio (SSMS):

1. Open SSMS and connect to your server
2. Right-click **Databases** → **Restore Database**
3. Select **Device** → browse to `project_new.bak`
4. Click **OK** to restore

### 5. Configure the Database Connection

In `can.py`, update the connection string to match your SQL Server setup:

```python
app.config['SQLALCHEMY_DATABASE_URI'] = (
    'mssql+pyodbc://@YOUR_SERVER_NAME/project_new?'
    'driver=ODBC+Driver+17+for+SQL+Server&Trusted_Connection=yes'
)
```

Replace `YOUR_SERVER_NAME` with your actual SQL Server instance name (e.g., `localhost` or `DESKTOP-XXXXX\SQLEXPRESS`).

### 6. Set a Secret Key

Replace the placeholder secret key in `can.py`:

```python
app.config['SECRET_KEY'] = 'your-strong-secret-key-here'
```

> ⚠️ **Never commit your real secret key to version control.**

### 7. Run the Application

```bash
python can.py
```

The app will start on **http://localhost:122**

---

## 🔑 Key Routes

| Route | Method | Description |
|---|---|---|
| `/` | GET | Homepage with banners and featured products |
| `/product` | GET | All products (sortable) |
| `/product/<category>` | GET | Products filtered by category |
| `/product/<id>` | GET, POST | Product details + reviews |
| `/search` | GET | Search results |
| `/search_suggestions` | GET | Live search suggestions (JSON) |
| `/create` | GET, POST | Register new account |
| `/login` | GET, POST | User login |
| `/logout` | GET | Logout |
| `/forgot_password` | GET, POST | Reset password |
| `/dashboard` | GET | User dashboard + order history |
| `/wishlist` | GET | View wishlist |
| `/wishlist/toggle/<id>` | POST | Add/remove from wishlist (AJAX) |
| `/add_to_cart/<id>` | POST | Add product to cart |
| `/add_to_cart_page` | GET | View cart |
| `/checkout` | GET | Checkout summary |
| `/complete_order` | POST | Place order |
| `/order-tracking` | GET | Order tracking page |
| `/order-tracking/<id>` | GET | Track specific order (JSON) |
| `/update_address` | POST | Update delivery address (JSON) |
| `/request_return_or_exchange` | POST | Send return/exchange email |
| `/admin` | GET | Admin panel |
| `/refund-policy` | GET | Refund policy |
| `/shipping-policy` | GET | Shipping policy |
| `/privacy-policy` | GET | Privacy policy |
| `/about` | GET | About page |

---

## 📬 Email Notifications

SheGlow sends a confirmation email to the customer when they submit a return or exchange request. This uses Gmail SMTP with App Passwords.

To configure your own sender email:

1. Go to your Google Account → Security → App Passwords
2. Generate a password for "Mail"
3. Update these values in `can.py`:

```python
sender_email = "youremail@gmail.com"
sender_password = "your-app-password"
```

> ⚠️ **Do not hardcode credentials in production.** Use environment variables instead.

---

## 🔒 Security Notes

> The following are known issues to address before deploying to production:

- **Passwords are stored in plaintext** — replace with `werkzeug.security.generate_password_hash` / `check_password_hash`
- **Secret key is hardcoded** — move to environment variables or a `.env` file
- **Email credentials are hardcoded** — use `os.environ` or a secrets manager
- **CSRF protection** — ensure `Flask-WTF` CSRF tokens are active on all forms

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Backend | Python, Flask |
| ORM | Flask-SQLAlchemy |
| Database | Microsoft SQL Server (via pyodbc) |
| Forms & Validation | Flask-WTF, WTForms |
| Frontend | HTML5, CSS3, Vanilla JavaScript |
| Email | smtplib (Gmail SMTP) |
| Session Management | Flask sessions (7-day persistent) |

---

## 🙌 Contributors

Built with 💖 by the **SheGlow Team**.

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).

---

<div align="center">
  <em>SHE<strong>GLOW</strong> — Because you deserve to glow. ✨</em>
</div>
