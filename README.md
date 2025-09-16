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
            background-color: #f8fafc;
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
            border-bottom-color: #3b82f6;
            color: #3b82f6;
        }
        .cart-item {
            animation: fadeIn 0.5s;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .food-image {
            height: 180px;
            object-fit: cover;
            border-top-left-radius: 0.5rem;
            border-top-right-radius: 0.5rem;
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
                <input type="text" id="searchInput" placeholder="Buscar item..." class="w-full pl-10 pr-4 py-2 bg-gray-50 border border-gray-300 rounded-lg shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500">
                <i class="fas fa-search absolute left-3 top-3 text-gray-400"></i>
            </div>
            <select id="priceFilter" class="px-4 py-2 bg-gray-50 border border-gray-300 rounded-lg shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500">
                <option value="all">Todos os preços</option>
                <option value="0-20">Até R$ 20</option>
                <option value="20-40">R$ 20 - R$ 40</option>
                <option value="40+">Acima de R$ 40</option>
            </select>
        </div>

        <!-- Table Number Input -->
        <div class="mb-6 flex items-center bg-blue-50 p-3 rounded-lg">
            <i class="fas fa-utensils text-blue-500 mr-3"></i>
            <div class="flex-grow">
                <label for="tableNumber" class="text-sm font-medium text-gray-700">Número da Mesa:</label>
                <input type="number" id="tableNumber" min="1" placeholder="Ex: 5" class="mt-1 block w-full px-4 py-2 bg-white border border-gray-300 rounded-lg shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500">
            </div>
        </div>

        <!-- Categories -->
        <div class="relative mb-6">
            <div id="category-tabs" class="flex overflow-x-auto scroll-container border-b-2 border-gray-200 pb-1">
                <button data-category="entradas" class="category-tab active text-gray-700 font-semibold py-2 px-4 whitespace-nowrap border-b-2 border-blue-500 transition duration-300">Entradas</button>
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