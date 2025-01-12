# jarolGarcia.github.io
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Productos - Tienda INOVA</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #2c3e50;
            color: #ecf0f1;
        }
        header {
            background-color: #1abc9c;
            color: white;
            padding: 1.5rem;
            text-align: center;
            font-size: 1.8rem;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
        }
        .container {
            margin: 3rem auto;
            padding: 2rem;
            max-width: 1200px;
            background-color: #34495e;
            border-radius: 12px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
        }
        .actions {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 1.5rem;
        }
        h2 {
            font-size: 1.8rem;
        }
        button {
            padding: 0.8rem 1.5rem;
            background-color: #2980b9;
            color: white;
            border: none;
            cursor: pointer;
            border-radius: 8px;
            font-size: 1rem;
            transition: background-color 0.3s;
        }
        button:hover {
            background-color: #3498db;
        }
        .product-card {
            display: flex;
            flex-direction: column;
            background-color: #1abc9c;
            border-radius: 8px;
            padding: 1.5rem;
            margin: 1rem;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
            color: #fff;
            text-align: center;
            width: 220px;
            transition: transform 0.3s ease;
        }
        .product-card:hover {
            transform: scale(1.05);
        }
        .product-card img {
            width: 100%;
            height: auto;
            border-radius: 6px;
        }
        .product-info {
            margin-top: 1rem;
        }
        .product-info h3 {
            font-size: 1.5rem;
            margin: 0.5rem 0;
        }
        .product-info p {
            font-size: 1rem;
            margin: 0.3rem 0;
        }
        .product-info select {
            width: 100%;
            padding: 0.5rem;
            border-radius: 4px;
            border: 1px solid #7f8c8d;
            margin-bottom: 1rem;
        }
        .product-info input[type="number"] {
            padding: 0.5rem;
            width: 60px;
            text-align: center;
            border-radius: 4px;
            border: 1px solid #7f8c8d;
        }
        .product-card button {
            background-color: #2980b9;
            color: white;
            padding: 0.8rem;
            border-radius: 8px;
            border: none;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        .product-card button:hover {
            background-color: #3498db;
        }
        .product-list {
            display: flex;
            flex-wrap: wrap;
            justify-content: space-between;
            margin-top: 2rem;
        }
        .actions button {
            padding: 0.8rem 2rem;
        }
        #cart-container {
            margin-top: 3rem;
            padding-top: 2rem;
            border-top: 2px solid #7f8c8d;
        }
        #cart-container table {
            border: 1px solid #7f8c8d;
            margin-top: 1rem;
        }
        #total {
            font-size: 1.5rem;
            font-weight: bold;
            margin-top: 1rem;
            text-align: right;
            color: #1abc9c;
        }
        .action-btns button {
            padding: 0.6rem;
            background-color: #e74c3c;
            border-radius: 4px;
            color: white;
            border: none;
        }
        .action-btns button:hover {
            background-color: #c0392b;
        }
    </style>
</head>
<body>
    <header>Productos - Tienda INOVA</header>
    <div class="container">
        <div class="actions">
            <h2>Lista de Productos</h2>
            <button onclick="window.location.href='carrito.html'">Ver Carrito</button>
        </div>
        <div class="product-list" id="product-list">
            <!-- Los productos se cargarán dinámicamente en tarjetas -->
        </div>

        <div id="cart-container">
            <h2>Carrito</h2>
            <table>
                <thead>
                    <tr>
                        <th>Nombre</th>
                        <th>Color</th>
                        <th>Talla</th>
                        <th>Cantidad</th>
                        <th>Precio Unitario</th>
                        <th>Total</th>
                        <th>Imagen</th>
                        <th>Acción</th>
                    </tr>
                </thead>
                <tbody id="cart-table">
                    <!-- Los productos del carrito se mostrarán aquí -->
                </tbody>
            </table>
            <div id="total">
                Total: $0
            </div>
            <button onclick="window.location.href='carrito.html'">Ver Carrito</button>
        </div>
    </div>

    <script>
        // Función para cargar los productos desde localStorage
        function fetchProducts() {
            const products = JSON.parse(localStorage.getItem('products')) || [];
            const productList = document.getElementById('product-list');
            productList.innerHTML = '';
            products.forEach((product, index) => {
                const colors = product.colors.split(',');
                const sizes = product.sizes.split(',');

                const colorOptions = colors.map(color => `<option value="${color}">${color}</option>`).join('');
                const sizeOptions = sizes.map(size => `<option value="${size}">${size}</option>`).join('');

                const productCard = document.createElement('div');
                productCard.classList.add('product-card');
                productCard.innerHTML = `
                    <img src="${product.images[0] || 'default-image.jpg'}" alt="Imagen de ${product.name}">
                    <div class="product-info">
                        <h3>${product.name}</h3>
                        <p><strong>Precio:</strong> $${product.priceUnit}</p>
                        <p><strong>Cantidad Disponible:</strong> ${product.quantity}</p>
                        <select>${colorOptions}</select>
                        <select>${sizeOptions}</select>
                        <input type="number" min="1" max="${product.quantity}" value="1">
                        <button onclick="addToCart(${index})">Agregar al carrito</button>
                    </div>
                `;
                productList.appendChild(productCard);
            });
        }

        // Función para agregar un producto al carrito
        function addToCart(index) {
            const products = JSON.parse(localStorage.getItem('products')) || [];
            const product = products[index];

            const selectedColor = document.querySelector(`#product-list .product-card:nth-child(${index + 1}) select:first-of-type`).value;
            const selectedSize = document.querySelector(`#product-list .product-card:nth-child(${index + 1}) select:last-of-type`).value;
            const quantityInput = document.querySelector(`#product-list .product-card:nth-child(${index + 1}) input[type="number"]`);
            const quantity = parseInt(quantityInput.value);

            const cart = JSON.parse(localStorage.getItem('cart')) || [];
            cart.push({ 
                name: product.name, 
                selectedColor, 
                selectedSize, 
                quantity,
                price: product.priceUnit,
                image: product.images[0] || 'default-image.jpg'  // Usamos la primera imagen del producto
            });
            localStorage.setItem('cart', JSON.stringify(cart));
            
            alert(`${product.name} ha sido agregado al carrito.`);
            updateCart();
        }

        // Función para actualizar el carrito en la interfaz y mostrar el total
        function updateCart() {
            const cart = JSON.parse(localStorage.getItem('cart')) || [];
            const cartTable = document.getElementById('cart-table');
            const totalElement = document.getElementById('total');
            cartTable.innerHTML = '';

            let total = 0;

            cart.forEach((item, index) => {
                const itemTotal = item.price * item.quantity;
                total += itemTotal;
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${item.name}</td>
                    <td>${item.selectedColor}</td>
                    <td>${item.selectedSize}</td>
                    <td>${item.quantity}</td>
                    <td>$${item.price}</td>
                    <td>$${itemTotal.toFixed(2)}</td>
                    <td><img src="${item.image}" alt="Imagen del producto" style="width: 50px; height: 50px;"></td>
                    <td><button class="action-btns" onclick="removeFromCart(${index})">Eliminar</button></td>
                `;
                cartTable.appendChild(row);
            });

            totalElement.textContent = `Total: $${total.toFixed(2)}`;
        }

        // Función para eliminar un producto del carrito
        function removeFromCart(index) {
            const cart = JSON.parse(localStorage.getItem('cart')) || [];
            cart.splice(index, 1);
            localStorage.setItem('cart', JSON.stringify(cart));
            updateCart();
        }

        // Inicializar productos y carrito
        fetchProducts();
        updateCart();
    </script>
</body>
</html>
