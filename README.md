# marcossanbat.github.io
Menu Digital Taberna
[menu taberna.txt](https://github.com/user-attachments/files/22350973/menu.taberna.txt)
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Card�pio Interativo</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet"/>
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
      inset: 0;
      z-index: 100;
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
      scrollbar-width: none;
    }
    .horizontal-scroll::-webkit-scrollbar { display: none; }
    .header-sticky {
      position: sticky;
      top: 0;
      z-index: 10;
    }
  </style>
</head>
<body class="bg-gray-100 flex items-center justify-center min-h-screen">

  <!-- Modal inicial -->
  <div id="tableModal" class="modal">
    <div class="bg-white p-8 rounded-xl shadow-2xl w-11/12 max-w-sm text-center">
      <h2 class="text-2xl font-bold mb-4 text-gray-800">Bem-vindo(a)!</h2>
      <p class="text-gray-600 mb-6">Informe o n�mero da sua mesa para come�ar.</p>
      <input id="tableNumberInput" type="number" min="1"
        class="w-full p-3 mb-4 text-center text-gray-700 bg-gray-100 rounded-lg focus:ring-2 focus:ring-amber-500" 
        placeholder="N�mero da Mesa"/>
      <button id="startOrderButton" class="w-full py-3 px-6 bg-amber-500 text-white font-bold rounded-lg shadow-lg hover:bg-amber-600 transition">
        Acessar Card�pio
      </button>
      <div id="error-message" class="text-red-500 mt-2 hidden">Insira um n�mero de mesa v�lido.</div>
    </div>
  </div>

  <!-- Container principal -->
  <div id="menuContainer" class="main-container hidden bg-white rounded-t-3xl shadow-2xl flex flex-col pt-4 min-h-screen">
    <header class="p-4 bg-white shadow-md header-sticky">
      <h1 class="text-3xl font-bold text-center text-gray-800 mb-4">Seu Card�pio</h1>
      <nav class="horizontal-scroll pb-2">
        <ul id="category-menu" class="flex gap-2"></ul>
      </nav>
    </header>

    <main id="menuItems" class="flex-1 overflow-y-auto scroll-container p-4"></main>

    <footer class="p-4 bg-white sticky bottom-0 z-20 shadow-[0_-5px_15px_rgba(0,0,0,0.05)]">
      <div class="flex justify-between items-center text-lg font-semibold mb-2 text-gray-700">
        <span>Total do Pedido:</span>
        <span id="totalPrice">R$ 0,00</span>
      </div>
      <button id="sendOrderButton" class="w-full py-3 px-6 bg-green-500 text-white font-bold rounded-lg shadow-lg hover:bg-green-600 transition flex items-center justify-center">
        <svg class="w-5 h-5 mr-2" fill="currentColor" viewBox="0 0 20 20">
          <path fill-rule="evenodd" d="M18 5.75c0-1.57-.42-2.333-1.428-2.333H3.428C2.422 3.417 2 4.18 2 5.75V14.5c0 1.57.422 2.333 1.428 2.333h13.144c1.006 0 1.428-.763 1.428-2.333V5.75zM12 10a2 2 0 10-4 0 2 2 0 004 0zM17.5 7.75a.5.5 0 010-1H17v1h.5zM17.5 9.75a.5.5 0 010-1H17v1h.5zM17.5 11.75a.5.5 0 010-1H17v1h.5z" clip-rule="evenodd"/>
        </svg>
        Enviar Pedido por WhatsApp
      </button>
    </footer>
  </div>

  <script>
    // Configura��o
    const WHATSAPP = '5531999998888'; // Substitua pelo n�mero do restaurante

    const menuData = [
      { category: "Petiscos", items: [
        { name: "Batata Frita", price: 30,9 description: "Tradicioanl, crocante e saborosa." },
        { name: "Batata Frita c/ Cheddar e Bacon", price: 40,9 description: "Batata Frita c/ Cheddar e Bacon." },
        { name: "Calabresa Acebolada", price: 45,9 description: "Tradicional corte de calabresa acebolada." },
	{ name: "Gurj�o de Frango", price: 45,9 description: "Tiras de Frango empadado frito com molho ros�." },
	{ name: "Gurj�o de Peixe", price: 49,9 description: "Tiras de Til�pia empadada frita com molho T�taro." }
      ]},
      { category: "Caldos e Sopas", items: [
        { name: "Angu � Baiana", price: 20, description: "Tradicional Angu � Baiana." },
        { name: "Caldo Taberna", price: 20, description: "Frango desfiado, bacon, batata." },
        { name: "Lasanha de Carne", price: 48, description: "Lasanha artesanal gratinada com queijo." }
      ]},
      { category: "Bebidas", items: [
        { name: "�gua Mineral", price: 6, description: "Sem g�s, 500ml." },
        { name: "Refrigerante Lata", price: 8, description: "Coca-Cola, Guaran�, Soda." },
        { name: "Suco Natural", price: 12, description: "Laranja, Abacaxi, Morango." }
      ]},
      { category: "Sobremesas", items: [
        { name: "Pudim de Leite", price: 15, description: "Cl�ssico pudim de leite com calda de caramelo." },
        { name: "Mousse de Chocolate", price: 18, description: "Feito com chocolate belga, leve e aerado." }
      ]}
    ];

    // Estado
    let tableNumber = null;
    const cart = {};

    // Seletores
    const els = {
      modal: document.getElementById('tableModal'),
      input: document.getElementById('tableNumberInput'),
      startBtn: document.getElementById('startOrderButton'),
      error: document.getElementById('error-message'),
      container: document.getElementById('menuContainer'),
      menu: document.getElementById('menuItems'),
      total: document.getElementById('totalPrice'),
      sendBtn: document.getElementById('sendOrderButton'),
      categories: document.getElementById('category-menu'),
    };

    // Renderiza��o
    function renderCategories() {
      els.categories.innerHTML = menuData.map(c =>
        `<li><button class="px-4 py-2 rounded-full bg-amber-100 text-amber-700 font-semibold hover:bg-amber-200 whitespace-nowrap"
          data-scroll="#category-${c.category.replace(/\s+/g,'-')}">${c.category}</button></li>`).join('');
    }

    function renderMenu() {
      els.menu.innerHTML = menuData.map(c => `
        <div id="category-${c.category.replace(/\s+/g,'-')}" class="mb-8">
          <h2 class="text-2xl font-bold text-gray-800 mb-4 p-2 bg-gray-200 rounded-lg header-sticky">${c.category}</h2>
          ${c.items.map(item => `
            <div class="flex items-center justify-between p-4 mb-4 bg-white rounded-xl shadow-md hover:shadow-lg transition">
              <div>
                <h3 class="text-lg font-semibold text-gray-800">${item.name}</h3>
                <p class="text-sm text-gray-500">${item.description}</p>
                <span class="text-lg font-bold text-amber-600 mt-1 block">R$ ${item.price.toFixed(2).replace('.', ',')}</span>
              </div>
              <div class="flex items-center space-x-2 min-w-[100px] justify-end">
                <button data-action="remove" data-name="${item.name}" class="p-2 text-gray-500 hover:text-red-500">
                  <svg class="h-6 w-6" fill="none" stroke="currentColor"><path stroke-linecap="round" stroke-width="2" d="M18 12H6"/></svg>
                </button>
                <span id="q-${item.name.replace(/\s+/g,'-')}" class="w-6 text-center text-lg font-bold">0</span>
                <button data-action="add" data-name="${item.name}" class="p-2 text-gray-500 hover:text-green-500">
                  <svg class="h-6 w-6" fill="none" stroke="currentColor"><path stroke-linecap="round" stroke-width="2" d="M12 6v12M6 12h12"/></svg>
                </button>
              </div>
            </div>`).join('')}
        </div>`).join('');
    }

    function updateCart() {
      let total = 0;
      menuData.flatMap(c => c.items).forEach(item => {
        const q = cart[item.name] || 0;
        document.getElementById(`q-${item.name.replace(/\s+/g,'-')}`).textContent = q;
        total += q * item.price;
      });
      els.total.textContent = `R$ ${total.toFixed(2).replace('.', ',')}`;
      els.sendBtn.classList.toggle('bg-green-600', total > 0);
    }

    // Eventos
    els.startBtn.addEventListener('click', () => {
      const num = els.input.value.trim();
      if (!num) return els.error.classList.remove('hidden');
      tableNumber = num;
      els.modal.classList.add('hidden');
      els.container.classList.remove('hidden');
      renderCategories();
      renderMenu();
    });

    els.categories.addEventListener('click', e => {
      if (e.target.dataset.scroll) {
        document.querySelector(e.target.dataset.scroll).scrollIntoView({ behavior: 'smooth' });
      }
    });

    els.menu.addEventListener('click', e => {
      const btn = e.target.closest('button[data-action]');
      if (!btn) return;
      const { action, name } = btn.dataset;
      cart[name] = (cart[name] || 0) + (action === 'add' ? 1 : -1);
      if (cart[name] <= 0) delete cart[name];
      updateCart();
    });

    els.sendBtn.addEventListener('click', () => {
      const items = Object.entries(cart);
      if (!items.length) return alert("Carrinho vazio!");
      let msg = `*PEDIDO - Mesa ${tableNumber}*\n\n`;
      let total = 0;
      items.forEach(([name, q]) => {
        const item = menuData.flatMap(c => c.items).find(i => i.name === name);
        msg += `${q}x ${name} - R$ ${(q * item.price).toFixed(2).replace('.', ',')}\n`;
        total += q * item.price;
      });
      msg += `\n*Total:* R$ ${total.toFixed(2).replace('.', ',')}`;
      window.open(`https://wa.me/${WHATSAPP}?text=${encodeURIComponent(msg)}`, '_blank');
    });
  </script>
</body>
</html>
