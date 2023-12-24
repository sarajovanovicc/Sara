<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title> Sara </title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 30px;
        }

        #app {
            max-width: 600px;
            margin: 0 auto;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 30px;
        }

        th, td {
            border: 1px solid #ddd;
            padding: 8px;
            text-align: left;
        }

        th {
            background-color: #f2f2f2;
        }

        p {
            margin-top: 10px;
        }

        #total-price {
            font-weight: bold;
        }

        .quantity-buttons {
            display: flex;
        }

        .quantity-buttons button {
            margin: 0 5px;
        }

        #add-item-form {
            margin-top: 20px;
        }

        #add-item-form input {
            margin-right: 10px;
        }
    </style>
</head>
<body>
    <div id="app">
        <table id="cart-table">
            <thead>
                <tr>
                    <th>Naziv</th>
                    <th>Cena</th>
                    <th>Kolicina</th>
                    <th>Ukupna cena</th>
                </tr>
            </thead>
            <tbody></tbody>
        </table>
        <p>Total: <span id="total-price">0</span></p>

        <form id="add-item-form">
            <label for="new-item-name">Naziv:</label>
            <input type="text" id="new-item-name" required>
            <label for="new-item-price">Cena:</label>
            <input type="number" id="new-item-price" min="0" required>
            <label for="new-item-quantity">Kolicina:</label>
            <input type="number" id="new-item-quantity" min="0" required>
            <button type="button" onclick="addItem()">Dodaj</button>
        </form>
    </div>

    <script>
        document.addEventListener("DOMContentLoaded", function () {
            var items = [
                { "naziv": "Chair", "komada": 1, "cena": 233 },
                { "naziv": "Car", "komada": 3, "cena": 324 },
                { "naziv": "Computer", "komada": 2, "cena": 319 },
                { "naziv": "Chair", "komada": 3, "cena": 405 },
                { "naziv": "Pizza", "komada": 3, "cena": 121 },
                { "naziv": "Chips", "komada": 3, "cena": 58 },
                { "naziv": "Table", "komada": 2, "cena": 324 },
                { "naziv": "Sausages", "komada": 3, "cena": 204 },
                { "naziv": "Pants", "komada": 3, "cena": 335 },
                { "naziv": "Table", "komada": 1, "cena": 350 }
            ];

            function renderTable() {
                var tbody = document.querySelector("#cart-table tbody");
                tbody.innerHTML = "";

                var totalPrice = 0;

                items.forEach(function (item, index) {
                    var row = tbody.insertRow();
                    var cellProduct = row.insertCell(0);
                    var cellPrice = row.insertCell(1);
                    var cellQuantity = row.insertCell(2);
                    var cellTotal = row.insertCell(3);
                    var quantityButtons = document.createElement('div');

                    cellProduct.textContent = item.naziv;
                    cellPrice.textContent = item.cena.toFixed(2);

                    var quantityInput = document.createElement('input');
                    quantityInput.type = 'number';
                    quantityInput.value = item.komada;
                    quantityInput.min = 0;
                    quantityInput.addEventListener('change', function () {

                        items[index].komada = parseInt(quantityInput.value);
                        renderTable();
                    });

                    var decreaseButton = document.createElement('button');
                    decreaseButton.textContent = '-';
                    decreaseButton.addEventListener('click', function () {
                        if (item.komada > 0) {
                            items[index].komada--;
                            renderTable();
                        }
                    });

                    var increaseButton = document.createElement('button');
                    increaseButton.textContent = '+';
                    increaseButton.addEventListener('click', function () {
                        items[index].komada++;
                        renderTable();
                    });

                    quantityButtons.appendChild(decreaseButton);
                    quantityButtons.appendChild(quantityInput);
                    quantityButtons.appendChild(increaseButton);

                    cellQuantity.appendChild(quantityButtons);

                    var total = item.cena * item.komada;
                    cellTotal.textContent = total.toFixed(2);
                    totalPrice += total;
                });

                document.querySelector("#total-price").textContent = totalPrice.toFixed(2);
            }

            window.addItem = function () {
                var newItemName = document.getElementById('new-item-name').value;
                var newItemPrice = parseFloat(document.getElementById('new-item-price').value);
                var newItemQuantity = parseInt(document.getElementById('new-item-quantity').value);

                if (newItemName && !isNaN(newItemPrice) && !isNaN(newItemQuantity) && newItemPrice >= 0 && newItemQuantity >= 0) {

                    items.push({ "naziv": newItemName, "komada": newItemQuantity, "cena": newItemPrice });

                    renderTable();
                
                    document.getElementById('add-item-form').reset();
                } else {
                    alert('Molimo vas da unesete ispravne podatke.');
                }
            };

            renderTable();
        });
    </script>
</body>
</html>
