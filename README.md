<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cardápio Interativo</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://kit.fontawesome.com/a076d05399.js" crossorigin="anonymous"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f3f4f6;
        }
        .scroll-container::-webkit-scrollbar {
            display: none;
        }
        .scroll-container {
            -ms-overflow-style: none;
            scrollbar-width: none;
        }
    </style>
</head>
<body class="bg-gray-100 min-h-screen flex flex-col items-center">

    <div class="w-full max-w-lg bg-white rounded-xl shadow-lg my-8 p-6 flex flex-col">

        <!-- Header -->
        <h1 class="text-3xl font-bold text-center text-gray-800 mb-2">Cardápio</h1>
        <p class="text-sm text-center text-gray-500 mb-6">Pedidos simplificados para a sua mesa.</p>

        <!-- Table Number Input -->
        <div class="mb-6">
            <label for="tableNumber" class="text-sm font-medium text-gray-700">Número da Mesa:</label>
            <input type="text" id="tableNumber" placeholder="Ex: 5" class="mt-1 block w-full px-4 py-2 bg-gray-50 border border-gray-300 rounded-lg shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500 transition duration-300">
        </div>

        <!-- Categories -->
        <div class="relative mb-6">
            <div id="category-tabs" class="flex overflow-x-auto scroll-container border-b-2 border-gray-200">
                <button data-category="entradas" class="category-tab text-gray-700 font-semibold py-2 px-4 whitespace-nowrap border-b-2 border-transparent transition duration-300 hover:border-blue-500 focus:outline-none">Entradas</button>
                <button data-category="pratos-principais" class="category-tab text-gray-700 font-semibold py-2 px-4 whitespace-nowrap border-b-2 border-transparent transition duration-300 hover:border-blue-500 focus:outline-none">Pratos Principais</button>
                <button data-category="bebidas" class="category-tab text-gray-700 font-semibold py-2 px-4 whitespace-nowrap border-b-2 border-transparent transition duration-300 hover:border-blue-500 focus:outline-none">Bebidas</button>
                <button data-category="sobremesas" class="category-tab text-gray-700 font-semibold py-2 px-4 whitespace-nowrap border-b-2 border-transparent transition duration-300 hover:border-blue-500 focus:outline-none">Sobremesas</button>
            </div>
            <div class="absolute bottom-0 left-0 w-full h-1 bg-gradient-to-r from-transparent via-gray-200 to-transparent"></div>
        </div>

        <!-- Menu Sections -->
        <div id="menu-sections" class="flex-grow">
            <!-- Entradas -->
            <div id="entradas-section" data-category="entradas" class="menu-section">
                <h2 class="text-2xl font-semibold text-gray-800 mb-4">Entradas</h2>
                <!-- Items for this category -->
            </div>

            <!-- Pratos Principais -->
            <div id="pratos-principais-section" data-category="pratos-principais" class="menu-section hidden">
                <h2 class="text-2xl font-semibold text-gray-800 mb-4">Pratos Principais</h2>
                <!-- Items for this category -->
            </div>

            <!-- Bebidas -->
            <div id="bebidas-section" data-category="bebidas" class="menu-section hidden">
                <h2 class="text-2xl font-semibold text-gray-800 mb-4">Bebidas</h2>
                <!-- Items for this category -->
            </div>

            <!-- Sobremesas -->
            <div id="sobremesas-section" data-category="sobremesas" class="menu-section hidden">
                <h2 class="text-2xl font-semibold text-gray-800 mb-4">Sobremesas</h2>
                <!-- Items for this category -->
            </div>
        </div>

        <!-- Order Summary & WhatsApp Button -->
        <div id="order-summary" class="bg-gray-50 rounded-lg p-4 mt-6">
            <h3 class="text-xl font-semibold text-gray-800 mb-2">Resumo do Pedido</h3>
            <p id="table-number-summary" class="text-gray-600 mb-2"></p>
            <ul id="summary-list" class="space-y-2 text-gray-700"></ul>
            <p id="empty-cart-message" class="text-gray-500 italic">Nenhum item adicionado.</p>
        </div>
        
        <button id="whatsapp-button" class="w-full mt-6 py-3 bg-green-500 text-white font-bold rounded-lg shadow-md hover:bg-green-600 transition duration-300 transform hover:scale-105 focus:outline-none focus:ring-2 focus:ring-green-500 focus:ring-opacity-50">
            Enviar Pedido via WhatsApp
        </button>

        <!-- Message Box -->
        <div id="message-box" class="fixed inset-0 bg-gray-800 bg-opacity-75 flex items-center justify-center p-4 z-50 hidden">
            <div class="bg-white rounded-lg p-6 shadow-xl max-w-xs w-full text-center">
                <p id="message-text" class="text-lg font-semibold text-gray-800 mb-4"></p>
                <button id="close-message-box" class="px-4 py-2 bg-blue-500 text-white rounded-lg hover:bg-blue-600 transition">OK</button>
            </div>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {

            const menu = {
                'entradas': [
                    { name: 'Bruschetta', description: 'Pão italiano, tomate, manjericão', price: 18.00 },
                    { name: 'Salada Caesar', description: 'Alface, frango, parmesão, croutons', price: 22.00 },
                    { name: 'Bolinho de Bacalhau', description: 'Porção com 6 unidades', price: 25.00 }
                ],
                'pratos-principais': [
                    { name: 'Filé à Parmegiana', description: 'Filé, molho, queijo, arroz, fritas', price: 45.00 },
                    { name: 'Risoto de Cogumelos', description: 'Arroz arbório, cogumelos frescos', price: 38.00 },
                    { name: 'Peixe Grelhado', description: 'Peixe do dia, legumes, arroz', price: 42.00 }
                ],
                'bebidas': [
                    { name: 'Água Mineral', description: 'Com ou sem gás', price: 5.00 },
                    { name: 'Refrigerante', description: 'Lata (diversos sabores)', price: 7.00 },
                    { name: 'Suco Natural', description: 'Laranja, limão ou abacaxi', price: 9.00 }
                ],
                'sobremesas': [
                    { name: 'Pudim de Leite', description: 'Tradicional', price: 12.00 },
                    { name: 'Mousse de Chocolate', description: 'Chocolate meio amargo', price: 14.00 },
                    { name: 'Frutas da Estação', description: 'Porção individual', price: 10.00 }
                ]
            };

            const order = {};
            const tableNumberInput = document.getElementById('tableNumber');
            const tableNumberSummary = document.getElementById('table-number-summary');
            const summaryList = document.getElementById('summary-list');
            const emptyCartMessage = document.getElementById('empty-cart-message');
            const whatsappButton = document.getElementById('whatsapp-button');
            const messageBox = document.getElementById('message-box');
            const messageText = document.getElementById('message-text');
            const closeMessageBox = document.getElementById('close-message-box');

            // Function to show a custom message box
            function showMessage(text) {
                messageText.textContent = text;
                messageBox.classList.remove('hidden');
            }

            // Function to close the custom message box
            closeMessageBox.addEventListener('click', () => {
                messageBox.classList.add('hidden');
            });

            // Function to render a single menu item
            function renderMenuItem(item) {
                const itemDiv = document.createElement('div');
                itemDiv.className = 'bg-white rounded-lg p-4 shadow mb-4';
                itemDiv.innerHTML = `
                    <div class="flex items-center justify-between mb-2">
                        <h3 class="font-semibold text-gray-800 text-lg">${item.name}</h3>
                        <span class="text-blue-600 font-bold">R$ ${item.price.toFixed(2)}</span>
                    </div>
                    <p class="text-sm text-gray-500 mb-4">${item.description}</p>
                    <div class="flex items-center justify-between">
                        <div class="flex items-center space-x-2">
                            <button class="quantity-btn decrement bg-gray-200 text-gray-700 w-8 h-8 rounded-full font-bold transition hover:bg-gray-300">-</button>
                            <span class="quantity text-xl font-medium">0</span>
                            <button class="quantity-btn increment bg-gray-200 text-gray-700 w-8 h-8 rounded-full font-bold transition hover:bg-gray-300">+</button>
                        </div>
                        <button class="add-to-order-btn bg-blue-500 text-white font-semibold py-2 px-4 rounded-lg shadow-sm hover:bg-blue-600 transition">Adicionar</button>
                    </div>
                `;

                const quantitySpan = itemDiv.querySelector('.quantity');
                const decrementBtn = itemDiv.querySelector('.decrement');
                const incrementBtn = itemDiv.querySelector('.increment');
                const addBtn = itemDiv.querySelector('.add-to-order-btn');

                // Handle quantity changes
                incrementBtn.addEventListener('click', () => {
                    let quantity = parseInt(quantitySpan.textContent);
                    quantitySpan.textContent = quantity + 1;
                });

                decrementBtn.addEventListener('click', () => {
                    let quantity = parseInt(quantitySpan.textContent);
                    if (quantity > 0) {
                        quantitySpan.textContent = quantity - 1;
                    }
                });

                // Handle adding item to order
                addBtn.addEventListener('click', () => {
                    const quantity = parseInt(quantitySpan.textContent);
                    if (quantity > 0) {
                        const existingItem = order[item.name];
                        if (existingItem) {
                            existingItem.quantity += quantity;
                        } else {
                            order[item.name] = { ...item, quantity };
                        }
                        updateOrderSummary();
                        showMessage(`${item.name} (${quantity}x) adicionado ao pedido!`);
                        quantitySpan.textContent = '0'; // Reset quantity after adding
                    } else {
                        showMessage('Selecione uma quantidade maior que zero.');
                    }
                });

                return itemDiv;
            }

            // Function to render all items for a given category
            function renderMenuSection(category) {
                const section = document.getElementById(`${category}-section`);
                section.innerHTML = `<h2 class="text-2xl font-semibold text-gray-800 mb-4">${section.getAttribute('data-category').replace('-', ' ').replace(/\b\w/g, l => l.toUpperCase())}</h2>`;
                menu[category].forEach(item => {
                    section.appendChild(renderMenuItem(item));
                });
            }

            // Update order summary on screen
            function updateOrderSummary() {
                summaryList.innerHTML = '';
                let hasItems = false;
                for (const itemName in order) {
                    if (order[itemName].quantity > 0) {
                        hasItems = true;
                        const li = document.createElement('li');
                        li.className = 'flex items-center justify-between text-gray-700';
                        li.innerHTML = `
                            <span>${order[itemName].quantity}x ${order[itemName].name}</span>
                            <button class="remove-item-btn text-red-500 hover:text-red-700 transition" data-item-name="${itemName}">
                                <i class="fas fa-trash-alt"></i>
                            </button>
                        `;
                        summaryList.appendChild(li);
                    }
                }
                emptyCartMessage.classList.toggle('hidden', hasItems);
            }

            // Event listener to remove item from summary
            document.getElementById('order-summary').addEventListener('click', (event) => {
                if (event.target.closest('.remove-item-btn')) {
                    const itemName = event.target.closest('.remove-item-btn').getAttribute('data-item-name');
                    delete order[itemName];
                    updateOrderSummary();
                }
            });

            // Handle category tab clicks
            document.querySelectorAll('.category-tab').forEach(tab => {
                tab.addEventListener('click', () => {
                    document.querySelectorAll('.category-tab').forEach(t => {
                        t.classList.remove('border-blue-500');
                        t.classList.add('border-transparent');
                    });
                    document.querySelectorAll('.menu-section').forEach(section => section.classList.add('hidden'));

                    const selectedCategory = tab.getAttribute('data-category');
                    tab.classList.remove('border-transparent');
                    tab.classList.add('border-blue-500');
                    document.getElementById(`${selectedCategory}-section`).classList.remove('hidden');
                });
            });

            // Handle WhatsApp button click
            whatsappButton.addEventListener('click', () => {
                const tableNumber = tableNumberInput.value.trim();
                if (!tableNumber) {
                    showMessage('Por favor, preencha o número da mesa.');
                    return;
                }

                if (Object.keys(order).length === 0) {
                    showMessage('Seu pedido está vazio.');
                    return;
                }

                let message = `*Novo Pedido*\n\n*Mesa:* ${tableNumber}\n\n*Itens:*\n`;
                for (const itemName in order) {
                    if (order[itemName].quantity > 0) {
                        message += `- ${order[itemName].quantity}x ${order[itemName].name}\n`;
                    }
                }
                message += `\nObrigado!`;

                // URL encode the message and create the WhatsApp link
                const whatsappUrl = `https://wa.me/5521983382711text=${encodeURIComponent(message)}`;
                window.open(whatsappUrl, '_blank');
            });

            // Initial rendering and state setup
            tableNumberInput.addEventListener('input', () => {
                const tableNumber = tableNumberInput.value.trim();
                tableNumberSummary.textContent = tableNumber ? `Mesa: ${tableNumber}` : '';
            });

            // Initial render of the first category
            renderMenuSection('entradas');
            document.querySelector('.category-tab').classList.add('border-blue-500');
            updateOrderSummary();
        });
    </script>

</body>
</html>

