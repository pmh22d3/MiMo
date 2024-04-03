# lovele
online shop
from flask import Flask, render_template, request, redirect, url_for
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///shop.db'
db = SQLAlchemy(app)

# Product Model
class Product(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(100), nullable=False)
    price = db.Column(db.Float, nullable=False)
    image = db.Column(db.String(100), nullable=False)

    def __repr__(self):
        return f'<Product {self.name}>'

# adding some products
db.create_all()
products = [
    {
        'name': 'Product 1',
        'price': 10.00,
        'image': 'product1.jpg'
    },
    {
        'name': 'Product 2',
        'price': 20.00,
        'image': 'product2.jpg'
    },
    {
        'name': 'Product 3',
        'price': 30.00,
        'image': 'product3.jpg'
    }
]

for product in products:
    new_product = Product(name=product['name'], price=product['price'], image=product['image'])
    db.session.add(new_product)
    db.session.commit()

@app.route('/')
def index():
    products = Product.query.all()
    return render_template('index.html', products=products)

if __name__ == '__main__':
    app.run(debug=True)
    <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Online Shop</title>
</head>
<body>
    <h1>Products</h1>
    <div class="products">
        {% for product in products %}
            <div class="product">
                <img src="/static/{{ product.image }}" alt="Product Image" width="200">
                <h2>{{ product.name }}</h2>
                <p>Price: ₹ {{ product.price }}</p>
            </div>
        {% endfor %}
    </div>
</body>
</html>    
