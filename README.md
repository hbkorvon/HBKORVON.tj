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

    input[type="file"], input, textarea {
      width: 100%; padding: 10px; margin-bottom: 10px;
      border-radius: 6px; border: 1px solid #ccc;
      font-size: 15px;
    }

    button {
      width: 100%; padding: 10px; font-size: 18px;
      background: #008080; color: white;
      border: none; border-radius: 6px; cursor: pointer;
    }

    #search-container {
      max-width: 600px; margin: 20px auto 0 auto;
    }

    #search {
      width: 100%; padding: 10px; font-size: 16px;
      border-radius: 6px; border: 1px solid #ccc;
    }

    #product-list {
      max-width: 900px; margin: 20px auto; padding-bottom: 100px;
      display: grid; grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
      gap: 20px;
    }

    .product {
      background: white; border-radius: 8px; box-shadow: 0 0 5px #aaa;
      padding: 10px; cursor: pointer;
      display: flex; flex-direction: column; align-items: center;
    }

    .product img {
      width: 100%; height: 150px; object-fit: cover; border-radius: 6px;
    }

    .product-info h3 { margin: 8px 0 4px 0; font-size: 18px; color: #333; text-align: center; }

    .delete-btn {
      margin-top: 10px; padding: 5px 10px;
      background: #cc0000; color: white;
      border: none; border-radius: 4px; cursor: pointer;
      font-size: 14px;
    }

    #product-detail {
      display: none; flex-direction: column;
      position: fixed; top: 0; left: 0; width: 100%; height: 100%;
      background: rgba(0,0,0,0.6); justify-content: center; align-items: center;
    }

    #product-detail .detail-box {
      background: white; padding: 20px; border-radius: 10px;
      max-width: 400px; width: 90%;
    }

    #product-detail img {
      width: 100%; height: 200px; object-fit: cover; border-radius: 6px;
    }

    .detail-btns a {
      display: inline-block; margin-top: 10px; text-align: center;
      padding: 10px 15px; border-radius: 6px;
      text-decoration: none; color: white; font-weight: bold;
    }

    .whatsapp-btn { background: #25D366; margin-right: 10px; }
    .instagram-btn { background: #C13584; }

    #close-detail {
      margin-top: 10px; background: #555; color: white;
      border: none; padding: 8px 16px; border-radius: 6px;
      cursor: pointer;
    }
  </style>
</head>
<body>

<header>HBKORVON</header>

<div id="login-section">
  <input type="password" id="admin-password" placeholder="Пароли админро ворид кунед" />
  <button id="login-btn">Ворид шудан</button>
</div>

<div id="admin-section" style="display:none;">
  <h2>Иловаи мол</h2>
  <input type="text" id="product-name" placeholder="Номи мол" />
  <input type="text" id="product-price" placeholder="Нарх" />
  <input type="file" id="product-image" accept="image/*" />
  <textarea id="product-desc" placeholder="Тавсифи мол"></textarea>
  <input type="text" id="product-whatsapp" placeholder="Номери WhatsApp (бе +)" />
  <input type="text" id="product-instagram" placeholder="Силкаи Instagram" />
  <button id="add-product-btn">Илова кардан</button>
  <button id="logout-btn" style="background:#cc0000;margin-top:10px;">Баромадан</button>
</div>

<div id="search-container" style="display:none;">
  <input type="text" id="search" placeholder="Ҷустуҷӯи мол..." oninput="renderProducts()" />
</div>

<div id="product-list"></div>

<div id="product-detail">
  <div class="detail-box">
    <img id="detail-image" src="" alt="">
    <h3 id="detail-name"></h3>
    <p id="detail-price"></p>
    <p id="detail-desc"></p>
    <div class="detail-btns">
      <a href="#" target="_blank" id="whatsapp-link" class="whatsapp-btn">WhatsApp</a>
      <a href="#" target="_blank" id="instagram-link" class="instagram-btn">Instagram</a>
    </div>
    <button id="close-detail">Бастан</button>
  </div>
</div>

<script>
const adminPassword = "MANHABIB*^*2007";
let isAdmin = false;
let products = JSON.parse(localStorage.getItem("products") || "[]");

function saveProducts() {
  localStorage.setItem("products", JSON.stringify(products));
}

function renderProducts() {
  const search = document.getElementById("search").value.toLowerCase();
  const list = document.getElementById("product-list");
  list.innerHTML = "";

  const filtered = products.filter(p => p.name.toLowerCase().includes(search));
  if (filtered.length === 0) {
    list.innerHTML = "<p style='text-align:center;color:#777;'>Ҳоло ҳеҷ моле нест.</p>";
    return;
  }

  filtered.forEach(p => {
    const div = document.createElement("div");
    div.className = "product";
    div.innerHTML = `
      <img src="${p.image}" />
      <div class="product-info">
        <h3>${p.name}</h3>
        <p><strong>Нарх:</strong> ${p.price}</p>
      </div>
    `;

    div.onclick = () => openProductDetail(p);

    if (isAdmin) {
      const delBtn = document.createElement("button");
      delBtn.textContent = "Удалит";
      delBtn.className = "delete-btn";
      delBtn.onclick = (e) => {
        e.stopPropagation();
        if (confirm("Молро нест кунам?")) {
          products = products.filter(x => x.id !== p.id);
          saveProducts();
          renderProducts();
        }
      };
      div.appendChild(delBtn);
    }

    list.appendChild(div);
  });
}

function openProductDetail(product) {
  document.getElementById("detail-image").src = product.image;
  document.getElementById("detail-name").textContent = product.name;
  document.getElementById("detail-price").innerHTML = "<strong>Нарх:</strong> " + product.price;
  document.getElementById("detail-desc").textContent = product.desc || "Тавсифи мол нест.";

  const whatsappBtn = document.getElementById("whatsapp-link");
  const instaBtn = document.getElementById("instagram-link");

  if (product.whatsapp) {
    const phone = product.whatsapp.replace(/\D/g, '');
    whatsappBtn.href = `https://wa.me/${phone}`;
  } else {
    whatsappBtn.href = "#";
  }

  if (product.instagram) {
    instaBtn.href = product.instagram;
  } else {
    instaBtn.href = "#";
  }

  document.getElementById("product-detail").style.display = "flex";
}

document.getElementById("close-detail").onclick = () => {
  document.getElementById("product-detail").style.display = "none";
};

document.getElementById("login-btn").onclick = () => {
  if (document.getElementById("admin-password").value === adminPassword) {
    isAdmin = true;
    document.getElementById("login-section").style.display = "none";
    document.getElementById("admin-section").style.display = "block";
    document.getElementById("search-container").style.display = "block";
    renderProducts();
  } else {
    alert("Парол нодуруст аст!");
  }
};

document.getElementById("logout-btn").onclick = () => {
  isAdmin = false;
  document.getElementById("admin-section").style.display = "none";
  document.getElementById("login-section").style.display = "block";
  document.getElementById("search-container").style.display = "none";
  renderProducts();
};

document.getElementById("add-product-btn").onclick = () => {
  const name = document.getElementById("product-name").value.trim();
  const price = document.getElementById("product-price").value.trim();
  const imageInput = document.getElementById("product-image");
  const desc = document.getElementById("product-desc").value.trim();
  const whatsapp = document.getElementById("product-whatsapp").value.trim();
  const instagram = document.getElementById("product-instagram").value.trim();

  if (!name || !price || !imageInput.files[0]) {
    alert("Лутфан ҳама маълумотро пур кунед.");
    return;
  }

  const reader = new FileReader();
  reader.onload = () => {
    const imageData = reader.result;
    const id = Date.now().toString();
    products.push({ id, name, price, image: imageData, desc, whatsapp, instagram });
    saveProducts();
    renderProducts();

    // Clear inputs
    document.getElementById("product-name").value = "";
    document.getElementById("product-price").value = "";
    imageInput.value = "";
    document.getElementById("product-desc").value = "";
    document.getElementById("product-whatsapp").value = "";
    document.getElementById("product-instagram").value = "";
  };
  reader.readAsDataURL(imageInput.files[0]);
};

window.onload = renderProducts;
</script>

</body>
</html>
