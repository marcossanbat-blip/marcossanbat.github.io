<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cardápio Digital</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');
        
        body {
            font-family: 'Inter', sans-serif;
            background-color: #F8F8F8;
            color: #333333;
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
            border-left: 4px solid #FF7F32;
            background-color: #FFFFFF;
        }
        
        .menu-item-card:hover {
            transform: translateY(-3px);
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
        }
        
        .category-tab {
            transition: color 0.3s, background-color 0.3s;
        }
        
        .category-tab.active {
            background-color: #FF7F32;
            color: #FFFFFF;
            border-bottom-color: #FF7F32;
        }
        
        .cart-item {
            animation: fadeIn 0.5s;
            border-left: 3px solid #FF7F32;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .quantity-btn {
            transition: all 0.2s;
            background-color: #E5E7EB;
            color: #4B5563;
        }
        
        .quantity-btn:hover {
            background-color: #FF7F32 !important;
            color: #FFFFFF !important;
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

        <!-- Cabeçalho -->
        <div class="text-center mb-6">
            <h1 class="text-3xl font-bold text-gray-800 mb-2">Cardápio Digital</h1>
            <p class="text-sm text-gray-600">Faça seu pedido diretamente pelo cardápio</p>
        </div>
        
        <!-- Campo de Pesquisa -->
        <div class="mb-6">
            <div class="relative flex-grow">
                <input type="text" id="searchInput" placeholder="Buscar item..." class="w-full pl-10 pr-4 py-2 bg-gray-100 text-gray-800 border border-gray-300 rounded-lg shadow-sm focus:outline-none focus:ring-2 focus:ring-[#FF7F32] focus:border-[#FF7F32]">
                <i class="fas fa-search absolute left-3 top-3 text-gray-400"></i>
            </div>
        </div>

        <!-- Número da Mesa -->
        <div class="mb-6 flex items-center bg-gray-100 p-3 rounded-lg">
            <i class="fas fa-utensils text-[#FF7F32] mr-3"></i>
            <div class="flex-grow">
                <label for="tableNumber" class="text-sm font-medium text-gray-700">Número da Mesa:</label>
                <input type="number" id="tableNumber" min="1" placeholder="Ex: 5" class="mt-1 block w-full px-4 py-2 bg-white text-gray-800 border border-gray-300 rounded-lg shadow-sm focus:outline-none focus:ring-2 focus:ring-[#FF7F32] focus:border-[#FF7F32]">
            </div>
        </div>

        <!-- Categorias -->
        <div class="relative mb-6">
            <div id="category-tabs" class="flex overflow-x-auto scroll-container border-b-2 border-gray-200 pb-1">
                <button data-category="entradas" class="category-tab active text-white font-semibold py-2 px-4 whitespace-nowrap border-b-2 border-transparent transition duration-300 rounded-t-lg">Entradas</button>
                <button data-category="pratos-principais" class="category-tab text-gray-800 font-semibold py-2 px-4 whitespace-nowrap border-b-2 border-transparent transition duration-300 rounded-t-lg">Pratos Principais</button>
                <button data-category="bebidas" class="category-tab text-gray-800 font-semibold py-2 px-4 whitespace-nowrap border-b-2 border-transparent transition duration-300 rounded-t-lg">Bebidas</button>
                <button data-category="sobremesas" class="category-tab text-gray-800 font-semibold py-2 px-4 whitespace-nowrap border-b-2 border-transparent transition duration-300 rounded-t-lg">Sobremesas</button>
            </div>
        </div>

        <!-- Seções do Menu -->
        <div id="menu-sections" class="flex-grow">
            <div id="entradas-section" data-category="entradas" class="menu-section grid grid-cols-1 gap-4"></div>
            <div id="pratos-principais-section" data-category="pratos-principais" class="menu-section hidden grid grid-cols-1 gap-4"></div>
            <div id="bebidas-section" data-category="bebidas" class="menu-section hidden grid grid-cols-1 gap-4"></div>
            <div id="sobremesas-section" data-category="sobremesas" class="menu-section hidden grid grid-cols-1 gap-4"></div>
        </div>

        <!-- Resumo do Pedido e Botão WhatsApp -->
        <div class="mt-8 pt-4 pb-2">
            <div id="order-summary" class="bg-gray-100 rounded-lg p-4 mb-4 shadow-inner">
                <h3 class="text-xl font-semibold text-gray-800 mb-3 flex items-center">
                    <i class="fas fa-receipt mr-2 text-[#FF7F32]"></i> Resumo do Pedido
                </h3>
                <p id="table-number-summary" class="text-gray-600 mb-3 bg-gray-200 py-1 px-3 rounded-md inline-block"></p>
                
                <div class="max-h-40 overflow-y-auto pr-2">
                    <ul id="summary-list" class="space-y-3 text-gray-700"></ul>
                    <p id="empty-cart-message" class="text-gray-500 italic text-center py-4">Nenhum item adicionado.</p>
                </div>

                <div id="total-container" class="border-t border-gray-300 pt-3 mt-3 hidden">
                    <div class="flex justify-between font-bold text-lg text-gray-800">
                        <span>Total:</span>
                        <span id="total-price" class="text-[#FF7F32]">R$ 0,00</span>
                    </div>
                </div>
            </div>
            
            <button id="whatsapp-button" class="w-full py-3 text-white font-bold rounded-lg shadow-md transition duration-300 transform hover:scale-[1.02] focus:outline-none focus:ring-2 focus:ring-green-500 focus:ring-opacity-50 flex items-center justify-center">
                <i class="fab fa-whatsapp mr-2 text-xl"></i> Enviar Pedido via WhatsApp
            </button>
        </div>

        <!-- Caixa de Mensagem -->
        <div id="message-box" class="fixed inset-0 bg-gray-800 bg-opacity-75 flex items-center justify-center p-4 z-50 hidden">
            <div class="bg-white rounded-lg p-6 shadow-xl max-w-xs w-full text-center border-t-4 border-[#FF7F32]">
                <div class="w-12 h-12 bg-gray-100 rounded-full flex items-center justify-center mx-auto mb-4">
                    <i class="fas fa-info-circle text-[#FF7F32] text-xl"></i>
                </div>
                <p id="message-text" class="text-lg font-semibold text-gray-800 mb-4"></p>
                <button id="close-message-box" class="px-6 py-2 bg-gray-800 text-white rounded-lg hover:bg-gray-700 transition">OK</button>
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
            const searchInput = document.getElementById('searchInput');
            const categoryTabs = document.getElementById('category-tabs');

            // Exibe uma caixa de mensagem personalizada
            function showMessage(text) {
                messageText.textContent = text;
                messageBox.classList.remove('hidden');
            }

            // Fecha a caixa de mensagem
            closeMessageBox.addEventListener('click', () => {
                messageBox.classList.add('hidden');
            });

            // Formata o preço para o padrão brasileiro
            function formatPrice(price) {
                return new Intl.NumberFormat('pt-BR', { style: 'currency', currency: 'BRL' }).format(price);
            }

            // Renderiza um único item do menu
            function renderMenuItem(item) {
                const itemDiv = document.createElement('div');
                itemDiv.className = 'menu-item-card rounded-lg shadow overflow-hidden p-4';
                itemDiv.innerHTML = `
                    <div class="flex items-start justify-between mb-2">
                        <div class="flex-grow">
                            <h3 class="font-semibold text-gray-800 text-lg">${item.name}</h3>
                            <p class="text-sm text-gray-600 mt-1">${item.description}</p>
                        </div>
                        <span class="text-[#FF7F32] font-bold ml-4">${formatPrice(item.price)}</span>
                    </div>
                    <div class="flex items-center justify-between mt-4">
                        <div class="flex items-center space-x-2">
                            <button class="quantity-btn decrement w-8 h-8 rounded-full font-bold transition">-</button>
                            <span class="quantity text-xl font-medium w-10 text-center text-gray-800">0</span>
                            <button class="quantity-btn increment w-8 h-8 rounded-full font-bold transition">+</button>
                        </div>
                    </div>
                `;

                const quantitySpan = itemDiv.querySelector('.quantity');
                const decrementBtn = itemDiv.querySelector('.decrement');
                const incrementBtn = itemDiv.querySelector('.increment');

                // Atualiza a quantidade do item no pedido e na tela
                const updateItemQuantity = (change) => {
                    let quantity = parseInt(quantitySpan.textContent);
                    const newQuantity = quantity + change;

                    if (newQuantity < 0) return;
                    
                    quantitySpan.textContent = newQuantity;

                    if (newQuantity > 0) {
                        order[item.name] = { ...item, quantity: newQuantity };
                    } else {
                        delete order[item.name];
                    }
                    updateOrderSummary();
                };

                incrementBtn.addEventListener('click', () => updateItemQuantity(1));
                decrementBtn.addEventListener('click', () => updateItemQuantity(-1));

                return itemDiv;
            }

            // Renderiza todos os itens de uma categoria com filtro de busca
            function renderMenuSection(category) {
                const section = document.getElementById(`${category}-section`);
                section.innerHTML = '';
                const searchTerm = searchInput.value.toLowerCase();

                const filteredItems = menu[category].filter(item => {
                    return item.name.toLowerCase().includes(searchTerm) || item.description.toLowerCase().includes(searchTerm);
                });

                if (filteredItems.length === 0) {
                    section.innerHTML = `<p class="text-center text-gray-500 italic py-8">Nenhum item encontrado nesta categoria com os filtros aplicados.</p>`;
                } else {
                    filteredItems.forEach(item => {
                        section.appendChild(renderMenuItem(item));
                    });
                }
            }

            // Atualiza o resumo do pedido na tela
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
                                <div class="font-medium text-gray-800">${item.quantity}x ${item.name}</div>
                                <div class="text-[#FF7F32] font-medium mt-1">${formatPrice(itemTotal)}</div>
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

            // Evento para remover item do resumo
            document.getElementById('order-summary').addEventListener('click', (event) => {
                const removeItemBtn = event.target.closest('.remove-item-btn');
                if (removeItemBtn) {
                    const itemName = removeItemBtn.getAttribute('data-item-name');
                    delete order[itemName];
                    updateOrderSummary();
                    
                    document.querySelectorAll('.menu-item-card').forEach(card => {
                        const itemTitle = card.querySelector('h3').textContent;
                        if (itemTitle === itemName) {
                            card.querySelector('.quantity').textContent = '0';
                        }
                    });
                    
                    showMessage('Item removido do pedido.');
                }
            });

            // Eventos para as abas de categoria
            categoryTabs.addEventListener('click', (event) => {
                const tab = event.target.closest('.category-tab');
                if (!tab) return;
                
                document.querySelectorAll('.category-tab').forEach(t => {
                    t.classList.remove('active');
                    t.classList.remove('text-white');
                    t.classList.add('text-gray-800');
                });
                document.querySelectorAll('.menu-section').forEach(section => section.classList.add('hidden'));

                const selectedCategory = tab.getAttribute('data-category');
                tab.classList.add('active');
                tab.classList.remove('text-gray-800');
                tab.classList.add('text-white');
                document.getElementById(`${selectedCategory}-section`).classList.remove('hidden');

                renderMenuSection(selectedCategory);
            });
            
            // Evento para o campo de busca
            searchInput.addEventListener('input', () => {
                const activeTab = document.querySelector('.category-tab.active');
                const currentCategory = activeTab ? activeTab.getAttribute('data-category') : 'entradas';
                renderMenuSection(currentCategory);
            });

            // Evento para o botão do WhatsApp
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

                const whatsappUrl = `https://wa.me/${whatsappNumber}?text=${encodeURIComponent(message)}`;
                window.open(whatsappUrl, '_blank');
            });

            // Renderização inicial
            tableNumberInput.addEventListener('input', () => {
                const tabl