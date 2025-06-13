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

    input, textarea, button {
      width: 100%; padding: 10px; margin-bottom: 10px;
      font-size: 16px; border-radius: 6px; border: 1px solid #ccc;
    }

    button {
      background: #008080; color: white; border: none; cursor: pointer;
    }

    #product-list {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(160px, 1fr));
      gap: 15px;
      max-width: 800px;
      margin: 20px auto;
    }

    .product {
      background: white;
      border-radius: 8px;
      box-shadow: 0 0 5px #aaa;
      padding: 10px;
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    .product img {
      width: 100%;
      height: 120px;
      object-fit: cover;
      border-radius: 6px;
    }

    .product-info {
      text-align: center;
    }

    .product-info h3 {
      margin: 8px 0 4px 0;
      font-size: 18px;
      color: #333;
    }

    .product-info p {
      margin: 0;
      color: #555;
    }

    .delete-btn {
      background: #cc0000;
      color: white;
      padding: 5px 10px;
      border: none;
      border-radius: 4px;
      font-size: 14px;
      margin-top: 8px;
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
  <h3>Иловаи мол</h3>
  <input type="text" id="product-name" placeholder="Номи мол" />
  <input type="text" id="product-price" placeholder="Нарх" />
  <input type="file" id="product-image" accept="image/*" />
  <textarea id="product-desc" placeholder="Тавсифи мол"></textarea>
  <input type="text" id="product-whatsapp" placeholder="Пайванд ба WhatsApp" />
  <input type="text" id="product-instagram" placeholder="Пайванд ба Instagram" />
  <button id="add-product-btn">Илова кардан</button>
  <button id="logout-btn" style="background:#cc0000;">Баромадан</button>
</div>

<div id="product-list"></div>

<script>
  const adminPassword = "MANHABIB*^*2007";
  let isAdmin = false;

  let products = JSON.parse(localStorage.getItem("products") || "[]");

  function saveProducts() {
    localStorage.setItem("products", JSON.stringify(products));
  }

  function renderProducts() {
    const container = document.getElementById("product-list");
    container.innerHTML = "";

    products.forEach(product => {
      const div = document.createElement("div");
      div.className = "product";

      const img = document.createElement("img");
      img.src = product.image;
      div.appendChild(img);

      const info = document.createElement("div");
      info.className = "product-info";
      info.innerHTML = `
        <h3>${product.name}</h3>
        <p><strong>Нарх:</strong> ${product.price}</p>
      `;
      div.appendChild(info);

      if (isAdmin) {
        const delBtn = document.createElement("button");
        delBtn.textContent = "Удалит";
        delBtn.className = "delete-btn";
        delBtn.onclick = () => {
          if (confirm("Мехоҳед ин молро нест кунед?")) {
            products = products.filter(p => p.id !== product.id);
            saveProducts();
            renderProducts();
          }
        };
        div.appendChild(delBtn);
      }

      container.appendChild(div);
    });

    if (products.length === 0) {
      container.innerHTML = "<p style='text-align:center;color:#777;'>Ҳоло ҳеҷ моле нест.</p>";
    }
  }

  document.getElementById("login-btn").onclick = () => {
    const pass = document.getElementById("admin-password").value;
    if (pass === adminPassword) {
      isAdmin = true;
      document.getElementById("login-section").style.display = "none";
      document.getElementById("admin-section").style.display = "block";
      renderProducts();
    } else {
      alert("Парол хато аст!");
    }
  };

  document.getElementById("logout-btn").onclick = () => {
    isAdmin = false;
    document.getElementById("admin-section").style.display = "none";
    document.getElementById("login-section").style.display = "block";
    renderProducts();
  };

  document.getElementById("add-product-btn").onclick = () => {
    const name = document.getElementById("product-name").value.trim();
    const price = document.getElementById("product-price").value.trim();
    const imageFile = document.getElementById("product-image").files[0];
    const desc = document.getElementById("product-desc").value.trim();
    const whatsapp = document.getElementById("product-whatsapp").value.trim();
    const instagram = document.getElementById("product-instagram").value.trim();

    if (!name || !price || !imageFile) {
      alert("Ном, нарх ва акси молро пур кунед.");
      return;
    }

    const reader = new FileReader();
    reader.onload = function(e) {
      const product = {
        id: Date.now(),
        name,
        price,
        image: e.target.result, // base64 image
        desc,
        whatsapp,
        instagram
      };
      products.push(product);
      saveProducts();
      renderProducts();

      document.getElementById("product-name").value = "";
      document.getElementById("product-price").value = "";
      document.getElementById("product-image").value = "";
      document.getElementById("product-desc").value = "";
      document.getElementById("product-whatsapp").value = "";
      document.getElementById("product-instagram").value = "";
    };
    reader.readAsDataURL(imageFile);
  };

  renderProducts();
</script>

</body>
</html>
