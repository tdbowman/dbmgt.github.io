# Flask Web Framework Basics

## What is Flask?

**Flask** is a lightweight Python web framework. It's simple to learn and powerful enough for real applications.

---

## Installation

```bash
pip install flask flask-sqlalchemy psycopg2-binary
```

---

## Hello World

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
    return 'Hello, World!'

if __name__ == '__main__':
    app.run(debug=True)
```

Run it:
```bash
python app.py
```

Visit: http://localhost:5000

---

## Routes and Views

```python
# Basic route
@app.route('/')
def home():
    return 'Home Page'

# Route with variable
@app.route('/product/<int:product_id>')
def product(product_id):
    return f'Product {product_id}'

# Multiple methods
@app.route('/submit', methods=['GET', 'POST'])
def submit():
    if request.method == 'POST':
        return 'Form submitted'
    return 'Show form'
```

---

## Templates (Jinja2)

Create `templates/products.html`:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Products</title>
</head>
<body>
    <h1>{{ title }}</h1>
    <ul>
    {% for product in products %}
        <li>{{ product.name }} - ${{ product.price }}</li>
    {% endfor %}
    </ul>
</body>
</html>
```

Use it:

```python
from flask import render_template

@app.route('/products')
def products():
    product_list = [
        {'name': 'Coffee', 'price': 3.50},
        {'name': 'Muffin', 'price': 2.50}
    ]
    return render_template('products.html', title='Our Products', products=product_list)
```

---

## Connecting to PostgreSQL

```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'postgresql://postgres:password@localhost/coffee_shop_db'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

db = SQLAlchemy(app)

# Define model
class Product(db.Model):
    __tablename__ = 'products'
    product_id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(100), nullable=False)
    price = db.Column(db.Numeric(10, 2), nullable=False)

# Use in route
@app.route('/products')
def products():
    all_products = Product.query.all()
    return render_template('products.html', products=all_products)
```

---

## CRUD Operations

### Read (List and Detail)

```python
@app.route('/products')
def product_list():
    products = Product.query.all()
    return render_template('products/list.html', products=products)

@app.route('/products/<int:id>')
def product_detail(id):
    product = Product.query.get_or_404(id)
    return render_template('products/detail.html', product=product)
```

### Create

```python
from flask import request, redirect, url_for

@app.route('/products/new', methods=['GET', 'POST'])
def product_new():
    if request.method == 'POST':
        product = Product(
            name=request.form['name'],
            price=request.form['price']
        )
        db.session.add(product)
        db.session.commit()
        return redirect(url_for('product_list'))
    return render_template('products/form.html')
```

### Update

```python
@app.route('/products/<int:id>/edit', methods=['GET', 'POST'])
def product_edit(id):
    product = Product.query.get_or_404(id)
    if request.method == 'POST':
        product.name = request.form['name']
        product.price = request.form['price']
        db.session.commit()
        return redirect(url_for('product_detail', id=id))
    return render_template('products/form.html', product=product)
```

### Delete

```python
@app.route('/products/<int:id>/delete', methods=['POST'])
def product_delete(id):
    product = Product.query.get_or_404(id)
    db.session.delete(product)
    db.session.commit()
    return redirect(url_for('product_list'))
```

---

## HTML Form Template

`templates/products/form.html`:

```html
<!DOCTYPE html>
<html>
<head>
    <title>{% if product %}Edit{% else %}New{% endif %} Product</title>
</head>
<body>
    <h1>{% if product %}Edit{% else %}New{% endif %} Product</h1>
    
    <form method="POST">
        <div>
            <label>Name:</label>
            <input type="text" name="name" value="{{ product.name if product else '' }}" required>
        </div>
        <div>
            <label>Price:</label>
            <input type="number" step="0.01" name="price" value="{{ product.price if product else '' }}" required>
        </div>
        <button type="submit">Save</button>
    </form>
    
    <a href="{{ url_for('product_list') }}">Back to List</a>
</body>
</html>
```

---

## Complete Mini Application

```python
from flask import Flask, render_template, request, redirect, url_for
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'postgresql://postgres:password@localhost/coffee_shop_db'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
db = SQLAlchemy(app)

class Product(db.Model):
    __tablename__ = 'products'
    product_id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(100), nullable=False)
    price = db.Column(db.Numeric(10, 2), nullable=False)

@app.route('/')
def index():
    return redirect(url_for('product_list'))

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
        product = Product(name=request.form['name'], price=request.form['price'])
        db.session.add(product)
        db.session.commit()
        return redirect(url_for('product_list'))
    return render_template('products/form.html')

@app.route('/products/<int:id>/edit', methods=['GET', 'POST'])
def product_edit(id):
    product = Product.query.get_or_404(id)
    if request.method == 'POST':
        product.name = request.form['name']
        product.price = request.form['price']
        db.session.commit()
        return redirect(url_for('product_detail', id=id))
    return render_template('products/form.html', product=product)

@app.route('/products/<int:id>/delete', methods=['POST'])
def product_delete(id):
    product = Product.query.get_or_404(id)
    db.session.delete(product)
    db.session.commit()
    return redirect(url_for('product_list'))

if __name__ == '__main__':
    app.run(debug=True)
```

---

## Summary

| Component | Purpose |
|-----------|---------|
| Routes | URL → function mapping |
| Templates | HTML with dynamic content |
| SQLAlchemy | Database ORM |
| Forms | User input |

**CRUD Pattern:**
- **C**reate: `db.session.add()`
- **R**ead: `Model.query.all()`, `.get()`
- **U**pdate: Modify object, `db.session.commit()`
- **D**elete: `db.session.delete()`

---

**Next Module:** [Web Application Project](../module11/overview.md)
