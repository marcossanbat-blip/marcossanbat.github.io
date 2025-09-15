<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cardápio Interativo</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f7f3f0;
        }
        .main-container {
            max-width: 600px;
            margin: auto;
            min-height: 100vh;
        }
        .modal {
            display: flex;
            position: fixed;
            z-index: 100;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0,0,0,0.5);
            align-items: center;
            justify-content: center;
        }
        .scroll-container {
            -webkit-overflow-scrolling: touch;
        }
        .horizontal-scroll {
            overflow-x: auto;
            white-space: nowrap;
        }
        .horizontal-scroll::-webkit-scrollbar {
            display: none;
        }
        .horizontal-scroll {
            -ms-overflow-style: none;
            scrollbar-width: none;
        }
        .header-sticky {
            position: sticky;
            top: 0;
            z-index: 10;
        }
    </style>
</head>
<body class="bg-gray-100">

    <!-- Modal para inserir o número da mesa -->
    <div id="tableModal" class="modal">
        <div class="bg-white p-8 rounded-xl shadow-2xl w-11/12 max-w-sm text-center">
            <h2 class="text-2xl font-bold mb-4 text-gray-800">Bem-vindo(a)!</h2>
            <p class="text-gray-600 mb-6">Por favor, informe o número da sua mesa para começar.</p>
            <input type="number" id="tableNumberInput" class="w-full p-3 mb-4 text-center text-gray-700 bg-gray-100 rounded-lg focus:outline-none focus:ring-2 focus:ring-amber-500" placeholder="Número da Mesa" min="1">
            <button id="startOrderButton" class="w-full py-3 px-6 bg-amber-500 text-white font-bold rounded-lg shadow-lg hover:bg-amber-600 transition-colors duration-300">
                Acessar Cardápio
            </button>
            <div id="error-message" class="text-red-500 mt-2 hidden">Por favor, insira um número de mesa válido.</div>
        </div>
    </div>

    <!-- Contêiner principal do cardápio -->
    <div id="menuContainer" class="main-container hidden bg-white rounded-t-3xl shadow-2xl flex flex-col pt-4 w-full min-h-screen">
        <header class="p-4 bg-white shadow-md header-sticky">
            <h1 class="text-3xl font-bold text-center text-gray-800 mb-4">Seu Cardápio</h1>
            <nav class="horizontal-scroll pb-2">
                <ul id="category-menu" class="flex gap-2">
                </ul>
            </nav>
        </header>

        <main id="menuItems" class="flex-1 overflow-y-auto scroll-container p-4">
        </main>

        <!-- Carrinho e Total -->
        <footer class="p-4 bg-white sticky bottom-0 z-20 shadow-[0_-5px_15px_rgba(0,0,0,0.05)]">
            <div id="cartSummary" class="flex justify-between items-center text-lg font-semibold mb-2 text-gray-700">
                <span>Total do Pedido:</span>
                <span id="totalPrice">R$ 0,00</span>
            </div>
            <div id="order-error" class="text-red-500 text-center mb-2 hidden"></div>
            <button id="sendOrderButton" class="w-full py-3 px-6 bg-green-500 text-white font-bold rounded-lg shadow-lg hover:bg-green-600 transition-colors duration-300">
                <div class="flex items-center justify-center">
                    <svg class="w-5 h-5 mr-2" fill="currentColor" viewBox="0 0 20 20">
                        <path fill-rule="evenodd" d="M18 5.75c0-1.57-.42-2.333-1.428-2.333H3.428C2.422 3.417 2 4.18 2 5.75V14.5c0 1.57.422 2.333 1.428 2.333h13.144c1.006 0 1.428-.763 1.428-2.333V5.75zM12 10a2 2 0 10-4 0 2 2 0 004 0zM17.5 7.75a.5.5 0 010-1H17v1h.5zM17.5 9.75a.5.5 0 010-1H17v1h.5zM17.5 11.75a.5.5 0 010-1H17v1h.5z" clip-rule="evenodd" />
                    </svg>
                    Enviar Pedido por WhatsApp
                </div>
            </button>
        </footer>
    </div>

    <script>
        // --- CONFIGURAÇÕES E DADOS DO CARDÁPIO ---
        const RESTAURANT_WHATSAPP_NUMBER = '5531999998888';

        const menuData = [
            {
                category: "Entradas",
                items: [
                    { name: "Batata Frita", price: 30.90, description: "Tradicional, crocante e saborosa." },
                    { name: "Batata Frita c/ Cheddar e Bacon", price: 40.90, description: "Batata Frita c/ Cheddar e Bacon." },
                    { name: "Calabresa Acebolada", price: 45.90, description: "Tradicional corte de calabresa acebolada." },
                    { name: "Gurjão de Frango", price: 45.90, description: "Tiras de Frango empanado frito com molho rosé." },
                    { name: "Gurjão de Peixe", price: 49.90, description: "Tiras de Tilápia empanada frita com molho Tátaro." }
                ]
            },
            {
                category: "Caldos e Sopas",
                items: [
                    { name: "Angu à Baiana", price: 20.00, description: "Tradicional Angu à Baiana." },
                    { name: "Caldo Taberna", price: 20.00, description: "Frango desfiado, bacon, batata." },
                ]
            },
            {
                category: "Bebidas",
                items: [
                    { name: "Água Mineral", price: 6.00, description: "Sem gás, 500ml." },
                    { name: "Refrigerante Lata", price: 8.00, description: "Coca-Cola, Guaraná, Soda." },
                    { name: "Suco Natural", price: 12.00, description: "Laranja, Abacaxi, Morango." },
                    { name: "Cerveja Artesanal", price: 22.00, description: "IPA local, 600ml." }
                ]
            },
            {
                category: "Sobremesas",
                items: [
                    { name: "Pudim de Leite Condensado", price: 15.00, description: "Clássico pudim de leite, cremoso e com calda de caramelo." },
                    { name: "Mousse de Chocolate", price: 18.00, description: "Feito com chocolate belga, leve e aerado." }
                ]
            },
        ];

        // --- VARIÁVEIS GLOBAIS ---
        let tableNumber = null;
        const cart = {};

        // --- ELEMENTOS DO DOM ---
        const tableModal = document.getElementById('tableModal');
        const tableNumberInput = document.getElementById('tableNumberInput');
        const startOrderButton = document.getElementById('startOrderButton');
        const errorMessage = document.getElementById('error-message');
        const menuContainer = document.getElementById('menuContainer');
        const menuItemsEl = document.getElementById('menuItems');
        const totalPriceEl = document.getElementById('totalPrice');
        const sendOrderButton = document.getElementById('sendOrderButton');
        const categoryMenu = document.getElementById('category-menu');

        // --- FUNÇÕES PRINCIPAIS ---

        // Renderiza o menu de categorias
        function renderCategoryMenu() {
            categoryMenu.innerHTML = '';
            menuData.forEach(category => {
                const li = document.createElement('li');
                const button = document.createElement('button');
                button.textContent = category.category;
                button.classList.add('px-4', 'py-2', 'rounded-full', 'bg-amber-100', 'text-amber-700', 'font-semibold', 'transition-colors', 'duration-200', 'hover:bg-amber-200', 'whitespace-nowrap');
                button.addEventListener('click', () => {
                    document.getElementById(`category-${category.category.replace(/\s+/g, '-')}`).scrollIntoView({ behavior: 'smooth', block: 'start' });
                });
                li.appendChild(button);
                categoryMenu.appendChild(li);
            });
        }

        // Renderiza o cardápio a partir dos dados do menuData
        function renderMenu() {
            menuItemsEl.innerHTML = '';
            menuData.forEach(category => {
                const categorySection = document.createElement('div');
                categorySection.id = `category-${category.category.replace(/\s+/g, '-')}`;
                categorySection.classList.add('mb-8');
                categorySection.innerHTML = `<h2 class="text-2xl font-bold text-gray-800 mb-4 p-2 bg-gray-200 rounded-lg header-sticky">${category.category}</h2>`;
                const itemsContainer = document.createElement('div');
                category.items.forEach(item => {
                    const itemEl = document.createElement('div');
                    itemEl.classList.add('flex', 'items-center', 'justify-between', 'p-4', 'mb-4', 'bg-white', 'rounded-xl', 'shadow-md', 'hover:shadow-lg', 'transition-shadow', 'duration-300');
                    const itemDetails = `
                        <div>
                            <h3 class="text-lg font-semibold text-gray-800">${item.name}</h3>
                            <p class="text-sm text-gray-500">${item.description}</p>
                            <span class="text-lg font-bold text-amber-600 mt-1 block">R$ ${item.price.toFixed(2).replace('.', ',')}</span>
                        </div>`;
                    const quantityControl = `
                        <div class="flex items-center space-x-2 min-w-[100px] justify-end">
                            <button data-name="${item.name}" class="remove-item p-2 text-gray-500 hover:text-red-500 transition-colors duration-200">
                                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M18 12H6" /></svg>
                            </button>
                            <span id="quantity-${item.name.replace(/\s+/g, '-')}" class="w-6 text-center text-lg font-bold">0</span>
                            <button data-name="${item.name}" class="add-item p-2 text-gray-500 hover:text-green-500 transition-colors duration-200">
                                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6v6m0 0v6m0-6h6m-6 0H6" /></svg>
                            </button>
                        </div>`;
                    itemEl.innerHTML = itemDetails + quantityControl;
                    itemsContainer.appendChild(itemEl);
                });
                categorySection.appendChild(itemsContainer);
                menuItemsEl.appendChild(categorySection);
            });
            setupEventListeners();
        }

        // Adiciona os eventos de clique aos botões de adicionar/remover
        function setupEventListeners() {
            document.querySelectorAll('.add-item').forEach(button => {
                button.addEventListener('click', (event) => {
                    const itemName = event.currentTarget.getAttribute('data-name');
                    addItemToCart(itemName);
                });
            });
            document.querySelectorAll('.remove-item').forEach(button => {
                button.addEventListener('click', (event) => {
                    const itemName = event.currentTarget.getAttribute('data-name');
                    removeItemFromCart(itemName);
                });
            });
        }

        function addItemToCart(itemName) {
            cart[itemName] = (cart[itemName] || 0) + 1;
            updateCartDisplay();
        }

        function removeItemFromCart(itemName) {
            if (cart[itemName] && cart[itemName] > 0) {
                cart[itemName] -= 1;
                if (cart[itemName] === 0) {
                    delete cart[itemName];
                }
            }
            updateCartDisplay();
        }

        function updateCartDisplay() {
            let total = 0;
            menuData.forEach(category => {
                category.items.forEach(item => {
                    const quantity = cart[item.name] || 0;
                    const quantityEl = document.getElementById(`quantity-${item.name.replace(/\s+/g, '-')}`);
                    if (quantityEl) {
                        quantityEl.textContent = quantity;
                    }
                    total += quantity * item.price;
                });
            });
            totalPriceEl.textContent = `R$ ${total.toFixed(2).replace('.', ',')}`;
            if (total > 0) {
                sendOrderButton.classList.remove('bg-green-500');
                sendOrderButton.classList.add('bg-green-600', 'shadow-xl');
            } else {
                sendOrderButton.classList.remove('bg-green-600', 'shadow-xl');
                sendOrderButton.classList.add('bg-green-500');
            }
        }

        // --- EVENTOS DO APLICATIVO ---

        // Início da interação: botão do modal
        startOrderButton.addEventListener('click', () => {
            const inputTableNumber = tableNumberInput.value.trim();
            if (inputTableNumber === '' || isNaN(parseInt(inputTableNumber)) || parseInt(inputTableNumber) <= 0) {
                errorMessage.classList.remove('hidden');
                return;
            }
            tableNumber = inputTableNumber;
            
            // --- CORREÇÃO APLICADA ---
            // Esconde o modal e mostra o container principal usando classes do Tailwind.
            // Isso garante que a propriedade 'display: flex' do container seja preservada.
            tableModal.classList.add('hidden');
            menuContainer.classList.remove('hidden');

            renderCategoryMenu();
            renderMenu();
        });

        // Envio do pedido
        sendOrderButton.addEventListener('click', () => {
            const orderedItems = Object.keys(cart).filter(item => cart[item] > 0);
            const orderErrorEl = document.getElementById('order-error');
            orderErrorEl.classList.add('hidden');
            
            if (orderedItems.length === 0) {
                // --- CORREÇÃO: Substituído alert() por uma mensagem na UI ---
                orderErrorEl.textContent = "Seu carrinho está vazio! Adicione alguns itens.";
                orderErrorEl.classList.remove('hidden');
                setTimeout(() => { orderErrorEl.classList.add('hidden'); }, 3000);
                return;
            }

            let message = `*NOVO PEDIDO DE MESA ${tableNumber}*\n\n`;
            let total = 0;

            orderedItems.forEach(itemName => {
                const quantity = cart[itemName];
                const item = menuData.flatMap(cat => cat.items).find(i => i.name === itemName);
                if (item) {
                    message += `* ${quantity}x ${itemName} - R$ ${(quantity * item.price).toFixed(2).replace('.', ',')}\n`;
                    total += quantity * item.price;
                }
            });

            message += `\n*Total do Pedido:* R$ ${total.toFixed(2).replace('.', ',')}`;
            
            const whatsappUrl = `https://wa.me/${RESTAURANT_WHATSAPP_NUMBER}?text=${encodeURIComponent(message)}`;
            window.open(whatsappUrl, '_blank');
        });
    </script>
</body>
</html>

