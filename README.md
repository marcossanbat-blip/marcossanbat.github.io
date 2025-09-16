<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cardápio Interativo</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');
        
        body {
            font-family: 'Inter', sans-serif;
            background: linear-gradient(135deg, #fece24 0%, #000000 100%);
            min-height: 100vh;
        }
        
        .scroll-container::-webkit-scrollbar {
            display: none;
        }
        
        .scroll-container {
            -ms-overflow-style: none;
            scrollbar-width: none;
        }
        
        .menu-item-card {
            transition: transform 0.3s, box-shadow 0.3s;
            border-left: 4px solid #fece24;
        }
        
        .menu-item-card:hover {
            transform: translateY(-3px);
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.2);
        }
        
        .category-tab.active {
            border-bottom-color: #fece24;
            color: #000;
            background-color: #fece24;
        }
        
        .cart-item {
            animation: fadeIn 0.5s;
            border-left: 3px solid #fece24;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .quantity-btn {
            transition: all 0.2s;
        }
        
        .quantity-btn:hover {
            background-color: #fece24 !important;
            color: #000 !important;
        }
        
        #whatsapp-button {
            background: linear-gradient(to right, #25D366, #128C7E);
        }
        
        #whatsapp-button:hover {
            background: linear-gradient(to right, #128C7E, #25D366);
        }
    </style>
</head>
<body class="min-h-screen flex flex-col items-center py-8 px-4">

    <div class="w-full max-w-2xl bg-white rounded-xl shadow-2xl my-4 p-6 flex flex-col">

        <!-- Header -->
        <div class="text-center mb-6">
            <h1 class="text-3xl font-bold text-gray-800 mb-2"><span class="bg-yellow-400 px-2 py-1 rounded">Cardápio Digital</span></h1>
            <p class="text-sm text-gray-600">Faça seu pedido diretamente pelo cardápio</p>
        </div>

        <!-- Table Number Input -->
        <div class="mb-6 flex items-center bg-black p-3 rounded-lg">
            <i class="fas fa-utensils text-yellow-400 mr-3"></i>
            <div class="flex-grow">
                <label for="tableNumber" class="text-sm font-medium text-white">Número da Mesa:</label>
                <input type="number" id="tableNumber" min="1" placeholder="Ex: 5" class="mt-1 block w-full px-4 py-2 bg-gray-800 text-white border border-gray-700 rounded-lg shadow-sm focus:outline-none focus:ring-2 focus:ring-yellow-400 focus:border-yellow-400">
            </div>
        </div>

        <!-- Categories -->
        <div class="relative mb-6">
            <div id="category-tabs" class="flex overflow-x-auto scroll-container border-b-2 border-gray-200 pb-1">
                <button data-category="entradas" class="category-tab active text-gray-800 font-semibold py-2 px-4 whitespace-nowrap border-b-2 border-transparent transition duration-300 rounded-t-lg">Entradas</button>
                <button data-category="pratos-principais" class="category-tab text-gray-800 font-semibold py-2 px-4 whitespace-nowrap border-b-2 border-transparent transition duration-300 rounded-t-lg">Pratos Principais</button>
                <button data-category="bebidas" class="category-tab text-gray-800 font-semibold py-2 px-4 whitespace-nowrap border-b-2 border-transparent transition duration-300 rounded-t-lg">Bebidas</button>
                <button data-category="sobremesas" class="category-tab text-gray-800 font-semibold py-2 px-4 whitespace-nowrap border-b-2 border-transparent transition duration-300 rounded-t-lg">Sobremesas</button>
            </div>
        </div>

        <!-- Menu Sections -->
        <div id="menu-sections" class="flex-grow">
            <!-- Entradas -->
            <div id="entradas-section" data-category="entradas" class="menu-section grid grid-cols-1 gap-4">
                <!-- Items will be populated by JavaScript -->
            </div>

            <!-- Pratos Principais -->
            <div id="pratos-principais-section" data-category="pratos-principais" class="menu-section hidden grid grid-cols-1 gap-4">
                <!-- Items will be populated by JavaScript -->
            </div>

            <!-- Bebidas -->
            <div id="bebidas-section" data-category="bebidas" class="menu-section hidden grid grid-cols-1 gap-4">
                <!-- Items will be populated by JavaScript -->
            </div>

            <!-- Sobremesas -->
            <div id="sobremesas-section" data-category="sobremesas" class="menu-section hidden grid grid-cols-1 gap-4">
                <!-- Items will be populated by JavaScript -->
            </div>
        </div>

        <!-- Order Summary & WhatsApp Button -->
        <div class="mt-8 sticky bottom-0 bg-white pt-4 pb-2 rounded-t-xl shadow-lg border-t">
            <div id="order-summary" class="bg-gray-100 rounded-lg p-4 mb-4 max-h-64 overflow-y-auto">
                <h3 class="text-xl font-semibold text-gray-800 mb-3 flex items-center">
                    <i class="fas fa-receipt mr-2"></i> Resumo do Pedido
                </h3>
                <p id="table-number-summary" class="text-gray-700 mb-3 bg-yellow-100 py-1 px-3 rounded-md inline-block"></p>
                <ul id="summary-list" class="space-y-3 text-gray-700"></ul>
                <div id="total-container" class="border-t border-gray-300 pt-3 mt-3 hidden">
                    <div class="flex justify-between font-bold text-lg">
                        <span>Total:</span>
                        <span id="total-price">R$ 0,00</span>
                    </div>
                </div>
                <p id="empty-cart-message" class="text-gray-500 italic text-center py-4">Nenhum item adicionado.</p>
            </div>
            
            <button id="whatsapp-button" class="w-full py-3 text-white font-bold rounded-lg shadow-md transition duration-300 transform hover:scale-[1.02] focus:outline-none focus:ring-2 focus:ring-green-500 focus:ring-opacity-50 flex items-center justify-center">
                <i class="fab fa-whatsapp mr-2 text-xl"></i> Enviar Pedido via WhatsApp
            </button>
        </div>

        <!-- Message Box -->
        <div id="message-box" class="fixed inset-0 bg-gray-800 bg-opacity-75 flex items-center justify-center p-4 z-50 hidden">
            <div class="bg-white rounded-lg p-6 shadow-xl max-w-xs w-full text-center">
                <div class="w-12 h-12 bg-yellow-100 rounded-full flex items-center justify-center mx-auto mb-4">
                    <i class="fas fa-info-circle text-yellow-500 text-xl"></i>
                </div>
                <p id="message-text" class="text-lg font-semibold text-gray-800 mb-4"></p>
                <button id="close-message-box" class="px-6 py-2 bg-black text-white rounded-lg hover:bg-gray-800 transition">OK</button>
            </div>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const whatsappNumber = '5521983382711';

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
            const totalContainer = document.getElementById('total-container');
            const totalPriceElement = document.getElementById('total-price');
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

            // Function to format price as Brazilian Real
            function formatPrice(price) {
                return new Intl.NumberFormat('pt-BR', { style: 'currency', currency: 'BRL' }).format(price);
            }

            // Function to render a single menu item
            function renderMenuItem(item, category) {
                const itemDiv = document.createElement('div');
                itemDiv.className = 'menu-item-card bg-white rounded-lg shadow overflow-hidden p-4';
                itemDiv.innerHTML = `
                    <div class="flex items-start justify-between mb-2">
                        <div class="flex-grow">
                            <h3 class="font-semibold text-gray-800 text-lg">${item.name}</h3>
                            <p class="text-sm text-gray-500 mt-1">${item.description}</p>
                        </div>
                        <span class="text-yellow-600 font-bold ml-4">${formatPrice(item.price)}</span>
                    </div>
                    <div class="flex items-center justify-between mt-4">
                        <div class="flex items-center space-x-2">
                            <button class="quantity-btn decrement bg-gray-200 text-gray-700 w-8 h-8 rounded-full font-bold transition hover:bg-gray-300">-</button>
                            <span class="quantity text-xl font-medium w-10 text-center">0</span>
                            <button class="quantity-btn increment bg-gray-200 text-gray-700 w-8 h-8 rounded-full font-bold transition hover:bg-gray-300">+</button>
                        </div>
                    </div>
                `;

                const quantitySpan = itemDiv.querySelector('.quantity');
                const decrementBtn = itemDiv.querySelector('.decrement');
                const incrementBtn = itemDiv.querySelector('.increment');

                // Handle quantity changes
                incrementBtn.addEventListener('click', () => {
                    let quantity = parseInt(quantitySpan.textContent);
                    quantitySpan.textContent = quantity + 1;
                    
                    // Automatically add to order when quantity changes
                    if (quantity + 1 > 0) {
                        if (order[item.name]) {
                            order[item.name].quantity += 1;
                        } else {
                            order[item.name] = { ...item, quantity: 1 };
                        }
                        updateOrderSummary();
                    }
                });

                decrementBtn.addEventListener('click', () => {
                    let quantity = parseInt(quantitySpan.textContent);
                    if (quantity > 0) {
                        quantitySpan.textContent = quantity - 1;
                        
                        // Automatically update order when quantity changes
                        if (order[item.name]) {
                            order[item.name].quantity -= 1;
                            if (order[item.name].quantity === 0) {
                                delete order[item.name];
                            }
                            updateOrderSummary();
                        }
                    }
                });

                return itemDiv;
            }

            // Function to render all items for a given category
            function renderMenuSection(category) {
                const section = document.getElementById(`${category}-section`);
                section.innerHTML = '';
                
                menu[category].forEach((item) => {
                    section.appendChild(renderMenuItem(item, category));
                });
            }

            // Update order summary on screen
            function updateOrderSummary() {
                summaryList.innerHTML = '';
                let hasItems = false;
                let total = 0;
                
                for (const itemName in order) {
                    if (order[itemName].quantity > 0) {
                        hasItems = true;
                        const item = order[itemName];
                        const itemTotal = item.price * item.quantity;
                        total += itemTotal;
                        
                        const li = document.createElement('li');
                        li.className = 'cart-item bg-white p-3 rounded-lg shadow-sm flex justify-between items-center';
                        li.innerHTML = `
                            <div class="flex-grow">
                                <div class="font-medium">${item.quantity}x ${item.name}</div>
                                <div class="text-yellow-600 font-medium mt-1">${formatPrice(itemTotal)}</div>
                            </div>
                            <button class="remove-item-btn text-red-500 hover:text-red-700 transition ml-3" data-item-name="${itemName}">
                                <i class="fas fa-trash-alt"></i>
                            </button>
                        `;
                        summaryList.appendChild(li);
                    }
                }
                
                emptyCartMessage.classList.toggle('hidden', hasItems);
                totalContainer.classList.toggle('hidden', !hasItems);
                
                if (hasItems) {
                    totalPriceElement.textContent = formatPrice(total);
                }
            }

            // Event listener to remove item from summary
            document.getElementById('order-summary').addEventListener('click', (event) => {
                if (event.target.closest('.remove-item-btn')) {
                    const itemName = event.target.closest('.remove-item-btn').getAttribute('data-item-name');
                    delete order[itemName];
                    updateOrderSummary();
                    
                    // Reset quantity in the menu
                    document.querySelectorAll('.menu-item-card').forEach(card => {
                        const itemTitle = card.querySelector('h3').textContent;
                        if (itemTitle === itemName) {
                            card.querySelector('.quantity').textContent = '0';
                        }
                    });
                    
                    showMessage('Item removido do pedido.');
                }
            });

            // Handle category tab clicks
            document.querySelectorAll('.category-tab').forEach(tab => {
                tab.addEventListener('click', () => {
                    document.querySelectorAll('.category-tab').forEach(t => {
                        t.classList.remove('active', 'border-yellow-400', 'bg-yellow-400');
                        t.classList.add('border-transparent');
                    });
                    document.querySelectorAll('.menu-section').forEach(section => section.classList.add('hidden'));

                    const selectedCategory = tab.getAttribute('data-category');
                    tab.classList.add('active');
                    document.getElementById(`${selectedCategory}-section`).classList.remove('hidden');
                });
            });

            // Handle WhatsApp button click
            whatsappButton.addEventListener('click', () => {
                const tableNumber = tableNumberInput.value.trim();
                if (!tableNumber) {
                    showMessage('Por favor, informe o número da mesa.');
                    return;
                }

                if (Object.keys(order).length === 0) {
                    showMessage('Seu pedido está vazio. Adicione itens antes de enviar.');
                    return;
                }

                let message = `*NOVO PEDIDO*\n\n`;
                message += `*Mesa:* ${tableNumber}\n\n`;
                message += `*ITENS DO PEDIDO:*\n`;
                
                let total = 0;
                for (const itemName in order) {
                    const item = order[itemName];
                    const itemTotal = item.price * item.quantity;
                    total += itemTotal;
                    
                    message += `\n${item.quantity}x ${item.name} - ${formatPrice(itemTotal)}`;
                }
                
                message += `\n\n----------------------------\n`;
                message += `*TOTAL: ${formatPrice(total)}*`;
                message += `\n\n_Obrigado pela preferência!_`;

                // URL encode the message and create the WhatsApp link with the number
                const whatsappUrl = `https://wa.me/${whatsappNumber}?text=${encodeURIComponent(message)}`;
                window.open(whatsappUrl, '_blank');
            });

            // Initial rendering and state setup
            tableNumberInput.addEventListener('input', () => {
                const tableNumber = tableNumberInput.value.trim();
                tableNumberSummary.textContent = tableNumber ? `Mesa: ${tableNumber}` : '';
            });

            // Initial render of all categories
            for (const category in menu) {
                renderMenuSection(category);
            }
            
            // Show the first category by default
            document.querySelector('.category-tab').classList.add('active');
            document.getElementById('entradas-section').classList.remove('hidden');
            
            updateOrderSummary();
        });
    </script>

</body>
</html>