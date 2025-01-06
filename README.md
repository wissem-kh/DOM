# DOM

           html
  <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Shopping Cart</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div id="cart">
        <h1>Your Shopping Cart</h1>
        <div class="cart-item">
            <p class="item-name">Item 1</p>
            <button class="like-button">❤️</button>
            <p class="item-price">$10</p>
            <button class="decrease">-</button>
            <input class="quantity" type="text" value="1" readonly>
            <button class="increase">+</button>
            <button class="remove">Remove</button>
        </div>
        <div class="cart-item">
            <p class="item-name">Item 2</p>
            <button class="like-button">❤️</button>
            <p class="item-price">$15</p>
            <button class="decrease">-</button>
            <input class="quantity" type="text" value="1" readonly>
            <button class="increase">+</button>
            <button class="remove">Remove</button>
        </div>
        <p>Total Price: $<span id="total-price">25</span></p>
    </div>

    <script src="script.js"></script>
</body>
</html>





       css
  body {
    font-family: Arial, sans-serif;
}

#cart {
    width: 80%;
    margin: auto;
}

.cart-item {
    display: flex;
    align-items: center;
    margin-bottom: 20px;
}

.cart-item p {
    margin: 0 10px;
}

button {
    padding: 5px 10px;
    margin: 0 5px;
    cursor: pointer;
}

.like-button.liked {
    color: red;
}

input.quantity {
    width: 30px;
    text-align: center;
}



    js
document.addEventListener('DOMContentLoaded', () => {
    const cart = document.getElementById('cart');
    const totalPriceElement = document.getElementById('total-price');

    // Function to update the total price
    function updateTotalPrice() {
        let totalPrice = 0;
        const items = cart.querySelectorAll('.cart-item');

        items.forEach(item => {
            const price = parseFloat(item.querySelector('.item-price').innerText.replace('$', ''));
            const quantity = parseInt(item.querySelector('.quantity').value);
            totalPrice += price * quantity;
        });

        totalPriceElement.innerText = totalPrice.toFixed(2);
    }

    // Event delegation for increase, decrease, remove, and like actions
    cart.addEventListener('click', (event) => {
        const target = event.target;

        // Increase quantity
        if (target.classList.contains('increase')) {
            const quantityInput = target.previousElementSibling;
            quantityInput.value = parseInt(quantityInput.value) + 1;
            updateTotalPrice();
        }

        // Decrease quantity
        if (target.classList.contains('decrease')) {
            const quantityInput = target.nextElementSibling;
            if (parseInt(quantityInput.value) > 1) {
                quantityInput.value = parseInt(quantityInput.value) - 1;
                updateTotalPrice();
            }
        }

        // Remove item
        if (target.classList.contains('remove')) {
            target.closest('.cart-item').remove();
            updateTotalPrice();
        }

        // Like item
        if (target.classList.contains('like-button')) {
            target.classList.toggle('liked');
        }
    });

    // Initial total price calculation
    updateTotalPrice();
});
