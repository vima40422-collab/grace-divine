# Resto grace-divine

<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="Site de commande en ligne pour le restaurant Chez Aya, avec menu, panier, paiement et dashboard admin.">
  <title>Restaurant Chez Aya 🍽️</title>

  <style>
    :root {
      --primary: #ff6600;
      --text: #333;
      --bg: #f5f5f5;
      --card: #ffffff;
    }

    * { box-sizing: border-box; }
    body { margin: 0; font-family: 'Segoe UI', 'Roboto', sans-serif; background: var(--bg); color: var(--text); }

    header { position: sticky; top: 0; background: white; border-bottom: 3px solid var(--primary); z-index: 10; }
    .topbar { display: flex; justify-content: space-between; align-items: center; max-width: 1200px; margin: 0 auto; padding: 1rem; }
    .brand { font-size: 1.5rem; font-weight: 700; }
    .brand span { color: var(--primary); }
    .hero-links { display: flex; gap: 0.8rem; }
    .hero-links a { font-size: 0.9rem; color: white; background: var(--primary); padding: 0.65rem 0.9rem; border-radius: 8px; text-decoration: none; }

    .container { max-width: 1200px; margin: 20px auto; padding: 0 15px; display: grid; grid-template-columns: 1fr 320px; gap: 20px; }
    .container-main { background: white; border-radius: 12px; padding: 20px; box-shadow: 0 4px 18px rgba(0,0,0,0.08); }

    .submenu { margin-bottom: 20px; display: flex; flex-wrap: wrap; gap: 10px; }
    .submenu input[type="search"] { flex: 1; min-width: 240px; padding: 10px; border: 1px solid #ccc; border-radius: 8px; }

    .menu-grid { display: grid; grid-template-columns: repeat(auto-fill,minmax(240px,1fr)); gap: 18px; }
    .card { background: var(--card); border-radius: 12px; overflow: hidden; box-shadow: 0 3px 12px rgba(0,0,0,0.08); transition: transform 0.2s ease, box-shadow 0.2s ease; }
    .card:hover { transform: translateY(-4px); box-shadow: 0 8px 18px rgba(0,0,0,0.14); }
    .card img { width: 100%; height: 160px; object-fit: cover; display: block; }
    .card-content { padding: 14px 14px 16px; }
    .card-content h3 { margin: 0 0 8px; font-size: 1.1rem; }
    .price { color: var(--primary); font-weight: 700; margin-bottom: 10px; }
    .card-content button { width: 100%; border: none; background: var(--primary); color: white; padding: 10px; border-radius: 8px; font-size: 0.95rem; cursor: pointer; transition: background 0.2s; }
    .card-content button:hover { background: #e55a00; }

    .cart { position: sticky; top: 20px; height: fit-content; background: white; border-radius: 12px; box-shadow: 0 4px 18px rgba(0,0,0,0.08); padding: 16px; }
    .cart h2 { margin-top: 0; }
    .cart-items { min-height: 120px; margin-bottom: 10px; }
    .cart-item { border-bottom: 1px solid #eee; padding: 8px 0; display: grid; grid-template-columns: 1fr auto; align-items: center; gap: 10px; }
    .cart-item:last-child { border-bottom: none; }
    .cart-actions { display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 8px; }
    .cart-actions button, .cart-actions a { border: none; background: var(--primary); color: white; padding: 9px 12px; border-radius: 8px; text-decoration: none; cursor: pointer; }
    .cart-actions button:hover, .cart-actions a:hover { background: #e55a00; }

    .payment-option { display: flex; align-items: center; gap: 10px; border: 1px solid #ddd; border-radius: 8px; padding: 8px 10px; cursor: pointer; margin-bottom: 8px; }
    .payment-option.selected { border-color: var(--primary); background: #fff3eb; }

    .footer { text-align: center; margin: 30px 0 20px; color: #555; font-size: 0.9rem; }
    .footer a { color: var(--primary); text-decoration: none; }

    @media (max-width: 960px) { .container { grid-template-columns: 1fr; } .cart { position: relative; top: 0; } }
  </style>
</head>

<body>
  <header>
    <div class="topbar">
      <div class="brand">Chez <span>Aya</span></div>
      <nav class="hero-links" aria-label="Liens rapides">
        <a href="https://www.opentable.fr/r/chez-aya" target="_blank" rel="noopener">Réservation</a>
        <a href="https://www.google.com/maps/search/Restaurant+Chez+Aya+Abidjan" target="_blank" rel="noopener">Itinéraire</a>
        <a href="https://www.facebook.com/" target="_blank" rel="noopener">Facebook</a>
      </nav>
    </div>
  </header>

  <main class="container" aria-live="polite">
    <section class="container-main" aria-label="Carte des plats">
      <div class="submenu">
        <input id="searchMenu" type="search" placeholder="Rechercher un plat..." aria-label="Rechercher un plat" />
      </div>

      <div class="menu-grid" id="menuGrid" aria-label="Liste des plats disponibles">
        <article class="card" data-name="Attiéké Poisson" data-price="3000">
          <img src="https://images.unsplash.com/photo-1598514983447-0160c95412c8?auto=format&fit=crop&w=800&q=80" alt="Attiéké Poisson" loading="lazy" />
          <div class="card-content">
            <h3>Attiéké Poisson</h3>
            <p class="price">3 000 FCFA</p>
            <button type="button" onclick="addToCart('Attiéké Poisson', 3000)">➕ Commander</button>
          </div>
        </article>

        <article class="card" data-name="Poulet braisé" data-price="4000">
          <img src="https://images.unsplash.com/photo-1551963831-b3b1ca40c98e?auto=format&fit=crop&w=800&q=80" alt="Poulet braisé" loading="lazy" />
          <div class="card-content">
            <h3>Poulet braisé</h3>
            <p class="price">4 000 FCFA</p>
            <button type="button" onclick="addToCart('Poulet braisé', 4000)">➕ Commander</button>
          </div>
        </article>

        <article class="card" data-name="Alloco" data-price="2000">
          <img src="https://images.unsplash.com/photo-1598514983750-d898174e1c5f?auto=format&fit=crop&w=800&q=80" alt="Alloco" loading="lazy" />
          <div class="card-content">
            <h3>Alloco</h3>
            <p class="price">2 000 FCFA</p>
            <button type="button" onclick="addToCart('Alloco', 2000)">➕ Commander</button>
          </div>
        </article>

        <article class="card" data-name="Riz gras" data-price="3500">
          <img src="https://images.unsplash.com/photo-1565299624946-b28f40a0ae38?auto=format&fit=crop&w=800&q=80" alt="Riz gras" loading="lazy" />
          <div class="card-content">
            <h3>Riz gras</h3>
            <p class="price">3 500 FCFA</p>
            <button type="button" onclick="addToCart('Riz gras', 3500)">➕ Commander</button>
          </div>
        </article>

        <article class="card" data-name="Brochettes" data-price="2500">
          <img src="https://images.unsplash.com/photo-1525351484163-7529414344d8?auto=format&fit=crop&w=800&q=80" alt="Brochettes" loading="lazy" />
          <div class="card-content">
            <h3>Brochettes</h3>
            <p class="price">2 500 FCFA</p>
            <button type="button" onclick="addToCart('Brochettes', 2500)">➕ Commander</button>
          </div>
        </article>

        <article class="card" data-name="Jus de bissap" data-price="1000">
          <img src="https://images.unsplash.com/photo-1571091718767-f634f05c2a49?auto=format&fit=crop&w=800&q=80" alt="Jus de bissap" loading="lazy" />
          <div class="card-content">
            <h3>Jus de bissap</h3>
            <p class="price">1 000 FCFA</p>
            <button type="button" onclick="addToCart('Jus de bissap', 1000)">➕ Commander</button>
          </div>
        </article>
      </div>
    </section>

    <aside class="cart" aria-label="Panier de commande">
      <h2>🛒 Panier</h2>
      <div id="cartItems" class="cart-items" aria-live="polite">Votre panier est vide.</div>
      <div class="total"><strong>Total :</strong> <span id="cartTotal">0</span> FCFA</div>

      <h3>Choisir paiement</h3>
      <div id="paymentMethods">
        <label class="payment-option" for="orangeMoney">
          <input id="orangeMoney" type="radio" name="payment" value="Orange Money" onchange="selectPayment(event)" /> Orange Money
        </label>
        <label class="payment-option" for="mtnMoney">
          <input id="mtnMoney" type="radio" name="payment" value="MTN Mobile Money" onchange="selectPayment(event)" /> MTN Mobile Money
        </label>
        <label class="payment-option" for="wavePay">
          <input id="wavePay" type="radio" name="payment" value="Wave" onchange="selectPayment(event)" /> Wave
        </label>
        <label class="payment-option" for="cardPay">
          <input id="cardPay" type="radio" name="payment" value="Carte Bancaire" onchange="selectPayment(event)" /> Carte Bancaire
        </label>
      </div>
      <input type="tel" id="phoneNumber" placeholder="Numéro de téléphone (ex: 0700000000)" aria-label="Numéro de téléphone" />

      <div class="cart-actions">
        <button id="clearCart" type="button" onclick="clearCart()">🗑️ Vider le panier</button>
        <a id="whatsappLink" class="whatsapp" href="https://wa.me/2250700000000" target="_blank" rel="noopener" onclick="return submitOrder();">📩 Commander via WhatsApp</a>
      </div>

      <div role="region" aria-live="assertive" id="message" style="margin-top:10px;color:#444;font-weight:600;"></div>
    </aside>
  </main>

  <footer class="footer">
    <p>📍 10 Rue des Héros, Abidjan, Côte d'Ivoire · ☎️ +225 07 0000 0000 · <a href="https://www.opentable.fr/" target="_blank" rel="noopener">Réserver</a></p>
  </footer>

  <script>
    let cart = [];
    let selectedPayment = null;

    function formatFCFA(amount) { return amount.toLocaleString('fr-FR'); }

    function renderCart() {
      const cartItems = document.getElementById('cartItems');
      const cartTotal = document.getElementById('cartTotal');
      const messageBox = document.getElementById('message');

      if (!cart.length) {
        cartItems.innerHTML = '<p>Votre panier est vide.</p>';
        cartTotal.textContent = '0';
        return;
      }

      cartItems.innerHTML = cart.map((item, i) => 
        `<div class="cart-item">
          <span>${item.name} (x${item.qty})</span>
          <span>${formatFCFA(item.price * item.qty)} FCFA</span>
          <button type="button" onclick="removeFromCart(${i})">Supprimer</button>
        </div>`
      ).join('');

      const total = cart.reduce((acc, item) => acc + item.price * item.qty, 0);
      cartTotal.textContent = formatFCFA(total);
      messageBox.textContent = `${cart.length} article(s) dans le panier.`;

      const phone = document.getElementById('phoneNumber').value.trim();
      const whatsapp = document.getElementById('whatsappLink');
      let text = `Bonjour, je voudrais commander :%0A`;
      cart.forEach(item => {
        text += `• ${item.name} x${item.qty} = ${formatFCFA(item.price * item.qty)} FCFA%0A`;
      });
      text += `%0ATotal: ${formatFCFA(total)} FCFA`;
      if (selectedPayment) text += `%0APaiement: ${selectedPayment}`;
      if (phone) text += `%0ATéléphone: ${phone}`;
      whatsapp.href = `https://wa.me/2250700000000?text=${encodeURIComponent(text)}`;
    }

    function addToCart(name, price) {
      const article = cart.find(item => item.name === name);
      if (article) { article.qty += 1; }
      else { cart.push({ name, price, qty: 1 }); }
      renderCart();
    }

    function removeFromCart(index) {
      cart.splice(index, 1);
      renderCart();
    }

    function clearCart() {
      cart = [];
      selectedPayment = null;
      document.querySelectorAll('input[name="payment"]').forEach(i => i.checked = false);
      document.getElementById('message').textContent = 'Panier vidé.';
      renderCart();
    }

    function selectPayment(event) {
      selectedPayment = event.target.value;
      document.querySelectorAll('.payment-option').forEach(el => el.classList.remove('selected'));
      event.target.closest('.payment-option').classList.add('selected');
      renderCart();
    }

    function submitOrder() {
      if (!cart.length) { alert('Votre panier est vide.'); return false; }
      if (!selectedPayment) { alert('Choisissez un moyen de paiement.'); return false; }
      const phone = document.getElementById('phoneNumber').value.trim();
      if (!phone) { alert('Veuillez renseigner votre numéro de téléphone.'); return false; }
      alert('Votre commande est prête et le chat WhatsApp s\'ouvrira pour confirmation.');
      clearCart();
      return true;
    }

    document.getElementById('searchMenu').addEventListener('input', function(e) {
      const term = e.target.value.toLowerCase();
      document.querySelectorAll('.menu-grid .card').forEach(card => {
        const name = card.dataset.name.toLowerCase();
        card.style.display = name.includes(term) ? '' : 'none';
      });
    });

    document.getElementById('phoneNumber').addEventListener('input', renderCart);

    renderCart();
  </script>
</body>
</html>
