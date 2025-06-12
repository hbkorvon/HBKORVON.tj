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

  #product-list {
    max-width: 600px; margin: 20px auto 60px auto;
    display: grid; grid-template-columns: repeat(auto-fill,minmax(180px,1fr)); gap: 15px;
  }
  .product {
    background: white; border-radius: 8px; box-shadow: 0 0 5px #aaa;
    padding: 10px; cursor: pointer; position: relative; user-select: none;
    display: flex; flex-direction: column; align-items: center;
  }
  .product img {
    max-width: 100%; border-radius: 6px;
    height: 120px; object-fit: cover;
  }
  .product-info { text-align: center; }
  .product-info h3 { margin: 8px 0 4px 0; font-size: 18px; color: #333; }
  .product-info p { margin: 0; color: #555; }

  .like-btn {
    position: absolute; top: 8px; right: 10px;
    font-size: 22px; user-select: none; color: #555;
    transition: color 0.3s ease;
  }
  .like-btn.liked { color: red; }

  #search-container {
    max-width: 600px; margin: 20px auto 0 auto;
  }
  #search {
    width: 100%; padding: 10px; font-size: 16px;
    border-radius: 6px; border: 1px solid #ccc;
  }

  .delete-btn {
    margin-top: 10px; padding: 5px 10px;
    background: #cc0000; color: white;
    border: none; border-radius: 4px; cursor: pointer;
    font-size: 14px;
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
  let likedProducts = JSON.parse(localStorage.getItem("likedProducts") || "[]");

  function saveProducts() {
    localStorage.setItem("products", JSON.stringify(products));
  }

  function saveLikes() {
    localStorage.setItem("likedProducts", JSON.stringify(likedProducts));
  }

  function renderProducts() {
    const searchTerm = document.getElementById("search").value.trim().toLowerCase();
    productList.innerHTML = "";
    products
      .filter(product => product.name.toLowerCase().includes(searchTerm))
      .forEach(product => {
        const div = document.createElement("div");
        div.className = "product";

        div.innerHTML = `
          <img src="${product.image}" alt="${product.name}" />
          <div class="like-btn" data-id="${product.id}">${likedProducts.includes(product.id) ? "❤️" : "🤍"}</div>
          <div class="product-info">
            <h3>${product.name}</h3>
            <p><strong>Нарх:</strong> ${product.price}</p>
          </div>
        `;

        const likeBtn = div.querySelector(".like-btn");
        likeBtn.addEventListener("click", e => {
          e.stopPropagation();
          const pid = product.id;
          if (likedProducts.includes(pid)) {
            likedProducts = likedProducts.filter(id => id !== pid);
            likeBtn.textContent = "🤍";
            likeBtn.classList.remove("liked");
          } else {
            likedProducts.push(pid);
            likeBtn.textContent = "❤️";
            likeBtn.classList.add("liked");
          }
          saveLikes();
        });

        if (likedProducts.includes(product.id)) {
          likeBtn.classList.add("liked");
        }

        if (isAdmin) {
          const delBtn = document.createElement("button");
          delBtn.className = "delete-btn";
          delBtn.textContent = "Удалит";
          delBtn.onclick = (e) => {
            e.stopPropagation();
            if (confirm("Шумо мутмаин ҳастед ки мехоҳед моли мазкурро нест кунед?")) {
              products = products.filter(p => p.id !== product.id);
              saveProducts();
              renderProducts();
            }
          };
          div.appendChild(delBtn);
        }

        productList.appendChild(div);
      });

    if (products.length === 0) {
      productList.innerHTML = "<p style='text-align:center;color:#777;'>Ҳоло ҳеҷ моле илова нашудааст.</p>";
    }
  }

  document.getElementById("login-btn").onclick = () => {
    const entered = document.getElementById("admin-password").value;
    if (entered === adminPassword) {
      isAdmin = true;
      document.getElementById("login-section").style.display = "none";
      document.getElementById("admin-section").style.display = "block";
      document.getElementById("search-container").style.display = "block";
      document.getElementById("login-error").textContent = "";
      renderProducts();
    } else {
      document.getElementById("login-error").textContent = "Парол хато аст!";
    }
  };

  document.getElementById("logout-btn").onclick = () => {
    isAdmin = false;
    document.getElementById("admin-section").style.display = "none";
    document.getElementById("login-section").style.display = "block";
    document.getElementById("search-container").style.display = "none";
    productList.innerHTML = "";
    document.getElementById("admin-password").value = "";
  };

  document.getElementById("add-product-btn").onclick = () => {
    const name = productNameInput.value.trim();
    const price = productPriceInput.value.trim();
    const image = productImageInput.value.trim();
    const desc = productDescInput.value.trim();
    const whatsapp = productWhatsappInput.value.trim();
    const instagram = productInstagramInput.value.trim();
    const id = Date.now().toString();

    if (!name || !price || !image) {
      alert("Лутфан номи мол, нарх ва URL акси молро пур кунед.");
      return;
    }

    products.push({ id, name, price, image, desc, whatsapp, instagram });
    saveProducts();
    renderProducts();

    productNameInput.value = "";
    productPriceInput.value = "";
    productImageInput.value = "";
    productDescInput.value = "";
    productWhatsappInput.value = "";
    productInstagramInput.value = "";
  };

  window.onload = () => {
    renderProducts();
  };
</script>

</body>
</html>
