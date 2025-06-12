<!DOCTYPE html>
<html lang="tg">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Маълумоти мол</title>
  <style>
    body { font-family: sans-serif; margin: 0; background: #f9f9f9; padding: 20px; }
    .container { max-width: 500px; margin: auto; background: white; border-radius: 8px; box-shadow: 0 0 10px #aaa; padding: 20px; text-align: center; }
    img { width: 100%; height: auto; border-radius: 6px; }
    h2 { margin-top: 15px; }
    p { color: #444; }
    .btns { margin-top: 20px; display: flex; justify-content: space-around; }
    .btns a {
      padding: 10px 20px;
      text-decoration: none;
      color: white;
      background: #25D366;
      border-radius: 6px;
      font-weight: bold;
    }
    .btns a.instagram {
      background: #E1306C;
    }
  </style>
</head>
<body>
  <div class="container">
    <img id="product-img" src="" alt="">
    <h2 id="product-name"></h2>
    <p id="product-price"></p>
    <p id="product-desc"></p>
    <div class="btns">
      <a id="whatsapp-link" target="_blank">WhatsApp</a>
      <a id="instagram-link" class="instagram" target="_blank">Instagram</a>
    </div>
  </div>

  <script>
    const products = JSON.parse(localStorage.getItem("products") || "[]");
    const selectedId = localStorage.getItem("selectedProductId");
    const product = products.find(p => p.id === selectedId);

    if (product) {
      document.getElementById("product-img").src = product.image;
      document.getElementById("product-name").textContent = product.name;
      document.getElementById("product-price").textContent = "Нарх: " + product.price;
      document.getElementById("product-desc").textContent = product.desc || "Тавсиф нест.";
      document.getElementById("whatsapp-link").href = product.whatsapp || "#";
      document.getElementById("instagram-link").href = product.instagram || "#";
    } else {
      document.querySelector(".container").innerHTML = "<p>Мол ёфт нашуд!</p>";
    }
  </script>
</body>
</html>
