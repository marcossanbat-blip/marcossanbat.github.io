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
            background-color: #ffffff;
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
        }
        .menu-item-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
        }
        .category-tab.active {
            border-bottom-color: #fece24;
            color: #fece24;
        }
        .cart-item {
            animation: fadeIn 0.5s;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
    </style>
</head>
<body class="bg-gray-50 min-h-screen flex flex-col items-center py-8">

    <div class="w-full max-w-2xl bg-white rounded-xl shadow-lg my-4 p-6 flex flex-col">

        <!-- Header -->
        <div class="text-center mb-6">
            <h1 class="text-3xl font-bold text-gray-800 mb-2">Cardápio Digital</h1>
            <p class="text-sm text-gray-500">Faça seu pedido diretamente pelo cardápio</p>
        </div>

        <!-- Search and Filter -->
        <div class="mb-6 flex flex-col sm:flex-row gap-3">
            <div class="relative flex-grow">
                <input type="text" id="searchInput" placeholder="Buscar item..." class="w-full pl-10 pr-4 py-2 bg-gray-100 border border-gray-300 rounded-lg shadow-sm focus:outline-none focus:ring-2 focus:ring-[#fece24] focus:border-[#fece24]">
                <i class="fas fa-search absolute left-3 top-3 text-gray-400"></i>
            </div>
        </div>

        <!-- Table Number Input -->
        <div class="mb-6 flex items-center bg-gray-100 p-3 rounded-lg">
            <i class="fas fa-utensils text-gray-700 mr-3"></i>
            <div class="flex-grow">
                <label for="tableNumber" class="text-sm font-medium text-gray-700">Número da Mesa:</label>
                <input type="number" id="tableNumber" min="1" placeholder="Ex: 5" class="mt-1 block w-full px-4 py-2 bg-white border border-gray-300 rounded-lg shadow-sm focus:outline-none focus:ring-2 focus:ring-[#fece24] focus:border-[#fece24]">
            </div>
        </div>

        <!-- Categories -->
        <div class="relative mb-6">
            <div id="category-tabs" class="flex overflow-x-auto scroll-container border-b-2 border-gray-200 pb-1">
                <button data-category="entradas" class="category-tab active text-gray-700 font-semibold py-2 px-4 whitespace-nowrap border-b-2 border-[#fece24] transition duration-300">Entradas</button>
                <button data-category="pratos-principais" class="category-tab text-gray-700 font-semibold py-2 px-4 whitespace-nowrap border-b-2 border-transparent transition duration-300">Pratos Principais</button>
                <button data-category="bebidas" class="category-tab text-gray-700 font-semibold py-2 px-4 whitespace-nowrap border-b-2 border-transparent transition duration-300">Bebidas</button>
                <button data-category="sobremesas" class="category-tab text-gray-700 font-semibold py-2 px-4 whitespace-nowrap border-b-2 border-transparent transition duration-300">Sobremesas</button>
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
        <div id="order-summary" class="bg-gray-100 rounded-lg p-4 mt-8 shadow-inner">
            <h3 class="text-xl font-bold text-gray-800 mb-4 flex items-center"><i class="fas fa-shopping-cart text-gray-700 mr-2"></i> Resumo do Pedido</h3>
            <p id="table-number-summary" class="text-gray-600 mb-2"></p>
            <ul id="summary-list" class="space-y-4 text-gray-700"></ul>
            <p id="empty-cart-message" class="text-gray-400 italic text-center py-4">Nenhum item adicionado.</p>
            <div id="order-total" class="font-bold text-lg text-gray-800 mt-4 pt-4 border-t border-gray-300 hidden">
                <span class="text-[#fece24]">Total:</span> <span id="total-price">R$ 0,00</span>
            </div>
        </div>

        <button id="whatsapp-button" class="w-full mt-6 py-4 bg-green-500 text-white font-bold rounded-lg shadow-lg hover:bg-green-600 transition duration-300 transform hover:scale-105 focus:outline-none focus:ring-4 focus:ring-green-300">
            <i class="fab fa-whatsapp mr-2"></i> Enviar Pedido via WhatsApp
        </button>
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
            const orderTotalDiv = document.getElementById('order-total');
            const totalPriceSpan = document.getElementById('total-price');
            const whatsappButton = document.getElementById('whatsapp-button');
            const categoryTabs = document.getElementById('category-tabs');
            const menuSections = document.getElementById('menu-sections');
            const searchInput = document.getElementById('searchInput');

            // Function to render a single menu item
            function renderMenuItem(item) {
                const itemDiv = document.createElement('div');
                itemDiv.className = 'menu-item-card bg-white rounded-lg shadow-md overflow-hidden p-4';
                itemDiv.innerHTML = `
                    <div>
                        <h3 class="font-bold text-gray-800 text-lg mb-1">${item.name}</h3>
                        <p class="text-sm text-gray-500 mb-3">${item.description}</p>
                        <div class="flex items-center justify-between mb-4">
                            <span class="text-[#fece24] font-extrabold text-xl">R$ ${item.price.toFixed(2)}</span>
                            <div class="flex items-center space-x-2 quantity-controls">
                                <button class="quantity-btn decrement bg-gray-200 text-gray-700 w-8 h-8 rounded-full font-bold transition hover:bg-gray-300"><i class="fas fa-minus"></i></button>
                                <span class="quantity text-lg font-medium">0</span>
                                <button class="quantity-btn increment bg-gray-200 text-gray-700 w-8 h-8 rounded-full font-bold transition hover:bg-gray-300"><i class="fas fa-plus"></i></button>
                            </div>
                        </div>
                    </div>
                `;

                const quantitySpan = itemDiv.querySelector('.quantity');
                const decrementBtn = itemDiv.querySelector('.decrement');
                const incrementBtn = itemDiv.querySelector('.increment');

                // Update item quantity in order and on screen
                const updateItemQuantity = (change) => {
                    let quantity = parseInt(quantitySpan.textContent);
                    quantity += change;

                    if (quantity < 0) quantity = 0;
                    
                    quantitySpan.textContent = quantity;

                    if (quantity > 0) {
                        order[item.name] = { ...item, quantity };
                    } else {
                        delete order[item.name];
                    }
                    updateOrderSummary();
                };

                // Handle quantity changes
                incrementBtn.addEventListener('click', () => updateItemQuantity(1));
                decrementBtn.addEventListener('click', () => updateItemQuantity(-1));

                return itemDiv;
            }

            // Function to render all items for a given category with search filter
            function renderMenuSection(category) {
                const section = document.getElementById(`${category}-section`);
                section.innerHTML = '';
                const searchTerm = searchInput.value.toLowerCase();

                const filteredItems = menu[category].filter(item => {
                    const matchesSearch = item.name.toLowerCase().includes(searchTerm) || item.description.toLowerCase().includes(searchTerm);
                    return matchesSearch;
                });

                if (filteredItems.length === 0) {
                    section.innerHTML = `<p class="text-center text-gray-500 italic py-8">Nenhum item encontrado nesta categoria com os filtros aplicados.</p>`;
                } else {
                    filteredItems.forEach(item => {
                        section.appendChild(renderMenuItem(item));
                    });
                }
            }
            
            // Function to update order summary on screen
            function updateOrderSummary() {
                summaryList.innerHTML = '';
                let hasItems = false;
                let total = 0;

                for (const itemName in order) {
                    if (order[itemName].quantity > 0) {
                        hasItems = true;
                        const item = order[itemName];
                        const subtotal = item.quantity * item.price;
                        total += subtotal;

                        const li = document.createElement('li');
                        li.className = 'cart-item flex flex-col sm:flex-row items-start sm:items-center justify-between p-3 bg-white rounded-lg shadow-sm';
                        li.innerHTML = `
                            <div class="flex items-center">
                                <span class="text-[#fece24] font-bold mr-2">${item.quantity}x</span>
                                <div>
                                    <span>${item.name}</span>
                                    <span class="text-xs text-gray-500 block sm:inline-block"> (R$ ${item.price.toFixed(2)} cada)</span>
                                </div>
                            </div>
                            <div class="flex items-center mt-2 sm:mt-0">
                                <span class="font-semibold text-gray-700">R$ ${subtotal.toFixed(2)}</span>
                                <button class="remove-item-btn text-red-500 hover:text-red-700 transition ml-4" data-item-name="${itemName}">
                                    <i class="fas fa-trash-alt"></i>
                                </button>
                            </div>
                        `;
                        summaryList.appendChild(li);
                    }
                }
                emptyCartMessage.classList.toggle('hidden', hasItems);
                orderTotalDiv.classList.toggle('hidden', !hasItems);
                totalPriceSpan.textContent = `R$ ${total.toFixed(2)}`;
            }

            // Event listener to remove item from summary
            document.getElementById('order-summary').addEventListener('click', (event) => {
                const removeItemBtn = event.target.closest('.remove-item-btn');
                if (removeItemBtn) {
                    const itemName = removeItemBtn.getAttribute('data-item-name');
                    delete order[itemName];
                    updateOrderSummary();
                }
            });

            // Handle category tab clicks
            categoryTabs.addEventListener('click', (event) => {
                const tab = event.target.closest('.category-tab');
                if (!tab) return;
                
                document.querySelectorAll('.category-tab').forEach(t => t.classList.remove('active', 'border-[#fece24]'));
                document.querySelectorAll('.menu-section').forEach(section => section.classList.add('hidden'));

                const selectedCategory = tab.getAttribute('data-category');
                tab.classList.add('active', 'border-[#fece24]');
                document.getElementById(`${selectedCategory}-section`).classList.remove('hidden');

                renderMenuSection(selectedCategory);
            });
            
            // Handle search changes
            let currentCategory = 'entradas';
            searchInput.addEventListener('input', () => {
                const activeTab = document.querySelector('.category-tab.active');
                currentCategory = activeTab ? activeTab.getAttribute('data-category') : 'entradas';
                renderMenuSection(currentCategory);
            });

            // Handle WhatsApp button click
            whatsappButton.addEventListener('click', () => {
                const tableNumber = tableNumberInput.value.trim();
                if (!tableNumber) {
                    alert('Por favor, preencha o número da mesa.');
                    return;
                }

                if (Object.keys(order).length === 0) {
                    alert('Seu pedido está vazio.');
                    return;
                }

                let message = `*Novo Pedido*\n\n*Mesa:* ${tableNumber}\n\n*Itens:*\n`;
                let total = 0;
                for (const itemName in order) {
                    if (order[itemName].quantity > 0) {
                        const item = order[itemName];
                        const subtotal = item.quantity * item.price;
                        total += subtotal;
                        message += `- ${item.quantity}x ${item.name} (R$ ${subtotal.toFixed(2)})\n`;
                    }
                }
                message += `\n*Total:* R$ ${total.toFixed(2)}`;
                message += `\n\nObrigado!`;

                // URL encode the message and create the WhatsApp link with the number
                const whatsappUrl = `https://wa.me/${whatsappNumber}?text=${encodeURIComponent(message)}`;
                window.open(whatsappUrl, '_blank');
            });

            // Initial rendering and state setup
            tableNumberInput.addEventListener('input', () => {
                const tableNumber = tableNumberInput.value.trim();
                tableNumberSummary.textContent = tableNumber ? `Mesa: ${tableNumber}` : '';
            });

            // Initial render of the first category
            renderMenuSection('entradas');
            document.querySelector('.category-tab[data-category="entradas"]').classList.add('active');
            updateOrderSummary();
        });
    </script>
</body>
</html>

