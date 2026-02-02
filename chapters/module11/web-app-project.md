# Web Application Example

## Project Overview

Examine a web-based application **Coffee Shop Management System** with Flask and PostgreSQL.

**Features:**
- Product management (CRUD)
- Customer management
- Order tracking
- Sales reports

---

## Project Structure

```
coffee_shop_app/
├── app.py              # Main application
├── config.py           # Configuration
├── models.py           # Database models
├── templates/
│   ├── base.html       # Base template
│   ├── index.html      # Home page
│   ├── products/
│   │   ├── list.html
│   │   ├── detail.html
│   │   └── form.html
│   ├── customers/
│   │   └── ...
│   └── orders/
│       └── ...
├── static/
│   ├── css/
│   │   └── style.css
│   └── js/
└── requirements.txt
```

---

## Configuration

`config.py`:
```python
import os

class Config:
    SECRET_KEY = os.environ.get('SECRET_KEY') or 'dev-secret-key'
    SQLALCHEMY_DATABASE_URI = os.environ.get('DATABASE_URL') or \
        'postgresql://postgres:password@localhost/coffee_shop_db'
    SQLALCHEMY_TRACK_MODIFICATIONS = False
```

---

## Models

`models.py`:
```python
from flask_sqlalchemy import SQLAlchemy
from datetime import datetime

db = SQLAlchemy()

class Customer(db.Model):
    __tablename__ = 'customers'
    customer_id = db.Column(db.Integer, primary_key=True)
    first_name = db.Column(db.String(50), nullable=False)
    last_name = db.Column(db.String(50), nullable=False)
    email = db.Column(db.String(100), unique=True, nullable=False)
    loyalty_points = db.Column(db.Integer, default=0)
    orders = db.relationship('Order', backref='customer', lazy=True)

class Product(db.Model):
    __tablename__ = 'products'
    product_id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(100), nullable=False)
    category = db.Column(db.String(50))
    price = db.Column(db.Numeric(10, 2), nullable=False)
    description = db.Column(db.Text)
    is_available = db.Column(db.Boolean, default=True)

class Order(db.Model):
    __tablename__ = 'orders'
    order_id = db.Column(db.Integer, primary_key=True)
    customer_id = db.Column(db.Integer, db.ForeignKey('customers.customer_id'))
    order_date = db.Column(db.DateTime, default=datetime.utcnow)
    total = db.Column(db.Numeric(10, 2))
    status = db.Column(db.String(20), default='pending')
    items = db.relationship('OrderItem', backref='order', lazy=True)

class OrderItem(db.Model):
    __tablename__ = 'order_items'
    order_id = db.Column(db.Integer, db.ForeignKey('orders.order_id'), primary_key=True)
    product_id = db.Column(db.Integer, db.ForeignKey('products.product_id'), primary_key=True)
    quantity = db.Column(db.Integer, nullable=False)
    unit_price = db.Column(db.Numeric(10, 2), nullable=False)
    product = db.relationship('Product')
```

---

## Base Template

`templates/base.html`:
```html
<!DOCTYPE html>
<html>
<head>
    <title>{% block title %}Coffee Shop{% endblock %}</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='css/style.css') }}">
</head>
<body>
    <nav>
        <a href="{{ url_for('index') }}">Home</a>
        <a href="{{ url_for('product_list') }}">Products</a>
        <a href="{{ url_for('customer_list') }}">Customers</a>
        <a href="{{ url_for('order_list') }}">Orders</a>
    </nav>
    
    <main>
        {% with messages = get_flashed_messages(with_categories=true) %}
            {% if messages %}
                {% for category, message in messages %}
                    <div class="alert alert-{{ category }}">{{ message }}</div>
                {% endfor %}
            {% endif %}
        {% endwith %}
        
        {% block content %}{% endblock %}
    </main>
</body>
</html>
```

---

## Main Application

`app.py`:
```python
from flask import Flask, render_template, request, redirect, url_for, flash
from config import Config
from models import db, Customer, Product, Order, OrderItem

app = Flask(__name__)
app.config.from_object(Config)
db.init_app(app)

# Create tables
with app.app_context():
    db.create_all()

@app.route('/')
def index():
    product_count = Product.query.count()
    customer_count = Customer.query.count()
    recent_orders = Order.query.order_by(Order.order_date.desc()).limit(5).all()
    return render_template('index.html', 
                           product_count=product_count,
                           customer_count=customer_count,
                           recent_orders=recent_orders)

# --- PRODUCTS ---
@app.route('/products')
def product_list():
    products = Product.query.all()
    return render_template('products/list.html', products=products)

@app.route('/products/<int:id>')
def product_detail(id):
    product = Product.query.get_or_404(id)
    return render_template('products/detail.html', product=product)

@app.route('/products/new', methods=['GET', 'POST'])
def product_new():
    if request.method == 'POST':
        product = Product(
            name=request.form['name'],
            category=request.form['category'],
            price=request.form['price'],
            description=request.form.get('description', '')
        )
        db.session.add(product)
        db.session.commit()
        flash('Product created successfully!', 'success')
        return redirect(url_for('product_list'))
    return render_template('products/form.html')

@app.route('/products/<int:id>/edit', methods=['GET', 'POST'])
def product_edit(id):
    product = Product.query.get_or_404(id)
    if request.method == 'POST':
        product.name = request.form['name']
        product.category = request.form['category']
        product.price = request.form['price']
        product.description = request.form.get('description', '')
        db.session.commit()
        flash('Product updated!', 'success')
        return redirect(url_for('product_detail', id=id))
    return render_template('products/form.html', product=product)

@app.route('/products/<int:id>/delete', methods=['POST'])
def product_delete(id):
    product = Product.query.get_or_404(id)
    db.session.delete(product)
    db.session.commit()
    flash('Product deleted!', 'info')
    return redirect(url_for('product_list'))

# --- CUSTOMERS (similar pattern) ---
# --- ORDERS (similar pattern) ---

if __name__ == '__main__':
    app.run(debug=True)
```

---

## Best Practices

### Input Validation
```python
@app.route('/products/new', methods=['POST'])
def product_new():
    name = request.form.get('name', '').strip()
    if not name:
        flash('Name is required', 'error')
        return redirect(url_for('product_new'))
    
    try:
        price = float(request.form['price'])
        if price <= 0:
            raise ValueError
    except ValueError:
        flash('Invalid price', 'error')
        return redirect(url_for('product_new'))
    
    # Continue with valid data...
```

### Error Handling
```python
@app.errorhandler(404)
def not_found(e):
    return render_template('errors/404.html'), 404

@app.errorhandler(500)
def server_error(e):
    return render_template('errors/500.html'), 500
```

### Security
```python
# CSRF Protection (use Flask-WTF)
from flask_wtf.csrf import CSRFProtect
csrf = CSRFProtect(app)

# SQL Injection Prevention
# Always use ORM or parameterized queries
# NEVER: f"SELECT * FROM users WHERE name = '{user_input}'"
```

---

## Running the Application

```bash
# Set environment variables
export FLASK_APP=app.py
export FLASK_ENV=development

# Run
flask run

# Or
python app.py
```

---

## Summary

**Project Components:**
- Configuration management
- Database models with relationships
- CRUD operations for all entities
- Templates with inheritance
- Flash messages for feedback
- Error handling
- Input validation

**Key Patterns:**
- Model-View-Controller (MVC)
- Create → List → Detail → Edit → Delete
- Form handling with POST/Redirect/GET

---

**Next Module:** [Review & Consolidation](../module12/overview.md)
