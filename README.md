<html lang="tg">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>HBKORVON - Сомона</title>
<style>
  body { font-family: Arial, sans-serif; margin: 0; padding: 0; background: #f0f0f0; }
  header { background: #008080; color: white; font-size: 24px; padding: 15px; text-align: center; font-weight: bold; }

  #login-section, #admin-section {
    max-width: 400px; margin: 20px auto; background: white;
    padding: 20px; border-radius: 8px; box-shadow: 0 0 10px #aaa;
  }
  #login-section input[type="password"] {
    width: 100%; padding: 10px; font-size: 16px; margin-bottom: 10px;
    border-radius: 6px; border: 1px solid #ccc;
  }
  #login-section button, #admin-section button {
    width: 100%; padding: 10px; font-size: 18px;
    background: #008080; color: white;
    border: none; border-radius: 6px; cursor: pointer;
  }
  #login-error { color: red; margin-bottom: 10px; }
  #admin-section input, #admin-section textarea {
    width: 100%; padding: 8px; margin-bottom: 10px;
    font-size: 15px; border-radius: 6px; border: 1px solid #ccc;
  }
  #admin-section textarea { resize: vertical; }

  #search-container {
    max-width: 700px;
    margin: 20px auto 0 auto;
  }
  #search {
    width: 100%; padding: 10px; font-size: 16px;
    border-radius: 6px; border: 1px solid #ccc;
  }

  /* CSS молҳо дар 2 сутун ва чокунча */
  #product-list {
    max-width: 700px;
    margin: 20px auto 60px auto;
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    grid-auto-rows: 140px;
    gap: 15px 20px;
  }
  .product {
    background: white;
    border-radius: 8px;
    box-shadow: 0 0 6px #aaa;
    padding: 8px;
    cursor: pointer;
    display: flex;
    align-items: center;
    gap: 12px;
    position: relative;
    overflow: hidden;
  }
  .product img {
    width: 100px;
    height: 100px;
    object-fit: cover;
    border-radius: 6px;
    flex-shrink: 0;
  }
  .product-info {
    flex-grow: 1;
    overflow: hidden;
  }
  .product-info h3 {
    margin: 0 0 6px 0;
    font-size: 16px;
    color: #333;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }
  .product-info p {
    margin: 2px 0;
    font-size: 14px;
    color: #555;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  .delete-btn {
    margin-top: 10px; padding: 5px 10px;
    background: #cc0000; color: white;
    border: none; border-radius: 4px; cursor: pointer;
    font-size: 14px;
  }

  /* Стили саҳифаи муфассал */
  #product-detail {
    position: fixed;
    top: 0; left: 0; right: 0; bottom: 0;
    background: rgba(0,0,0,0.6);
    display: none;
    align-items: center;
    justify-content: center;
    z-index: 1000;
  }
  #product-detail-content {
    background: white;
    max-width: 400px;
    width: 90%;
    border-radius: 10px;
    padding: 20px;
    position: relative;
  }
  #product-detail-content img {
    max-width: 100%;
    border-radius: 8px;
    margin-bottom: 15px;
  }
  #product-detail-content h2 {
    margin: 0 0 10px 0;
    font-size: 22px;
    color: #333;
  }
  #product-detail-content p {
    color: #555;
    margin: 5px 0;
  }
  #product-detail-content .btns {
    margin-top: 15px;
    display: flex;
    justify-content: space-between;
  }
  #product-detail-content .btns a {
    flex: 1;
    margin: 0 5px;
    text-align: center;
    padding: 10px 0;
    border-radius: 6px;
    color: white;
    text-decoration: none;
    font-weight: bold;
  }
  #whatsapp-btn {
    background: #25d366;
  }
  #instagram-btn {
    background: #e1306c;
  }
  #close-detail {
    position: absolute;
    top: 10px; right: 15px;
    font-size: 24px;
    font-weight: bold;
    cursor: pointer;
    color: #888;
  }
  #close-detail:hover {
    color: #333;
  }
</style>
</head>
<body>

<header>HBKORVON</header>

<div id="login-section">
  <div id="login-error"></div>
  <input type="password" id="admin-password" placeholder="Пароли админро ворид кунед" />
  <button id="login-btn">Ворид шудан</button>
</div>

<div id="admin-section" style="display:none;">
  <h2>Иловаи мол</h2>
  <input type="text" id="product-name" placeholder="Номи мол" />
  <input type="text" id="product-price" placeholder="Нарх" />
  <input type="text" id="product-image" placeholder="URL акси мол" />
  <textarea id="product-desc" placeholder="Тавсифи мол"></textarea>
  <input type="text" id="product-whatsapp" placeholder="Пайванд ба WhatsApp" />
  <input type="text" id="product-instagram" placeholder="Пайванд ба Instagram" />
  <button id="add-product-btn">Илова кардан</button>
  <button id="logout-btn" style="margin-top: 10px; background: #cc0000;">Баромадан</button>
</div>

<div id="search-container" style="display:none;">
  <input type="text" id="search" placeholder="Номи молро нависед..." oninput="renderProducts()" />
</div>

<div id="product-list"></div>

<!-- Саҳифаи муфассали мол -->
<div id="product-detail">
  <div id="product-detail-content">
    <span id="close-detail">&times;</span>
    <img id="detail-image" src="" alt="Product Image" />
    <h2 id="detail-name"></h2>
    <p id="detail-price"></p>
    <p id="detail-desc"></p>
    <div class="btns">
      <a href="#" target="_blank" id="whatsapp-btn">WhatsApp</a>
      <a href="#" target="_blank" id="instagram-btn">Instagram</a>
    </div>
  </div>
</div>

<script>
  const adminPassword = "MANHABIB*^*2007";
  let isAdmin = false;

  const productList = document.getElementById("product-list");
  const productNameInput = document.getElementById("product-name");
  const productPriceInput = document.getElementById("product-price");
  const productImageInput = document.getElementById("product-image");
  const productDescInput = document.getElementById("product-desc");
  const productWhatsappInput = document.getElementById("product-whatsapp");
  const productInstagramInput = document.getElementById("product-instagram");

  let products = JSON.parse(localStorage.getItem("products") || "[]");

  function saveProducts() {
    localStorage.setItem("products", JSON.stringify(products));
  }

  function renderProducts() {
    const searchTerm = document.getElementById("search").value.trim().toLowerCase();
    productList.innerHTML = "";
    const filteredProducts = products.filter(p => p.name.toLowerCase().includes(searchTerm));

    filteredProducts.forEach(product => {
      const div = document.createElement("div");
      div.className = "product";
      div.dataset.id = product.id;

      div.innerHTML = `
        <img src="${product.image}" alt="${product.name}" />
        <div class="product-info">
          <h3 title="${product.name}">${product.name}</h3>
          <p>${product.price} сомонӣ</p>
        </div>
        ${isAdmin ? `<button class="delete-btn">Ҳазф</button>` : ""}
      `;

      productList.appendChild(div);

      // Ҳазфи мол барои админ
      if (isAdmin) {
        const deleteBtn = div.querySelector(".delete-btn");
        deleteBtn.addEventListener("click", (e) => {
          e.stopPropagation();
          products = products.filter(p => p.id !== product.id);
          saveProducts();
          renderProducts();
        });
      }

      // Клик барои дидани муфассал
      div.addEventListener("click", () => {
        openProductDetail(product);
      });
    });

    if (filteredProducts.length === 0) {
      productList.innerHTML = "<p style='text-align:center; color:#666;'>Маҳсулот вуҷуд надорад.</p>";
    }
  }

  function openProductDetail(product) {
    document.getElementById("detail-image").src = product.image;
    document.getElementById("detail-name").textContent = product.name;
    document.getElementById("detail-price").textContent = product.price + " сомонӣ";
    document.getElementById("detail-desc").textContent = product.desc || "";
    document.getElementById("whatsapp-btn").href = product.whatsapp || "#";
    document.getElementById("instagram-btn").href = product.instagram || "#";
    document.getElementById("product-detail").style.display = "flex";
  }
  document.getElementById("close-detail").addEventListener("click", () => {
    document.getElementById("product-detail").style.display = "none";
  });

  document.getElementById("login-btn").addEventListener("click", () => {
    const pass = document.getElementById("admin-password").value;
    const errorDiv = document.getElementById("login-error");
    if (pass === adminPassword) {
      isAdmin = true;
      errorDiv.textContent = "";
      document.getElementById("login-section").style.display = "none";
      document.getElementById("admin-section").style.display = "block";
      document.getElementById("search-container").style.display = "block";
      renderProducts();
    } else {
      errorDiv.textContent = "Пароли нодуруст!";
    }
  });

  document.getElementById("logout-btn").addEventListener("click", () => {
    isAdmin = false;
    document.getElementById("admin-section").style.display = "none";
    document.getElementById("search-container").style.display = "none";
    document.getElementById("login-section").style.display = "block";
    renderProducts();
  });

  document.getElementById("add-product-btn").addEventListener("click", () => {
    const name = productNameInput.value.trim();
    const price = productPriceInput.value.trim();
    const image = productImageInput.value.trim();
    if (!name || !price || !image) {
      alert("Номи мол, нарх ва аксро пур кунед.");
      return;
    }
    const newProduct = {
      id: Date.now(),
      name,
      price,
      image,
      desc: productDescInput.value.trim(),
      whatsapp: productWhatsappInput.value.trim(),
      instagram: productInstagramInput.value.trim()
    };
    products.push(newProduct);
    saveProducts();
    renderProducts();
    productNameInput.value = "";
    productPriceInput.value = "";
    productImageInput.value = "";
    productDescInput.value = "";
    productWhatsappInput.value = "";
    productInstagramInput.value = "";
  });

  renderProducts();
</script>

</body>
</html>
