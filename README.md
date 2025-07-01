<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Enviar Mensaje WhatsApp</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f1f1f1;
      padding: 30px;
    }

    .container {
      max-width: 600px;
      background: white;
      padding: 25px;
      border-radius: 10px;
      margin: auto;
      box-shadow: 0 4px 10px rgba(0,0,0,0.1);
    }

    h2 {
      text-align: center;
      color: #25D366;
    }

    label {
      font-weight: bold;
      display: block;
      margin-top: 10px;
    }

    input, textarea, select, button {
      width: 100%;
      padding: 12px;
      margin-top: 5px;
      margin-bottom: 15px;
      border-radius: 6px;
      border: 1px solid #ccc;
      font-size: 14px;
    }

    button {
      background-color: #25D366;
      color: white;
      font-weight: bold;
      border: none;
      cursor: pointer;
    }

    button:hover {
      background-color: #1ebe5b;
    }

    .resultado {
      text-align: center;
      font-size: 14px;
    }
  </style>
</head>
<body>

<div class="container">
  <h2>📤 Enviar Mensaje por WhatsApp</h2>

  <label for="codigo">Código de país:</label>
  <select id="codigo">
    <option value="57">🇨🇴 +57 (Colombia)</option>
    <option value="1">🇺🇸 +1 (EEUU)</option>
    <option value="34">🇪🇸 +34 (España)</option>
    <option value="52">🇲🇽 +52 (México)</option>
    <!-- Puedes agregar más -->
  </select>

  <label for="numero">Número de teléfono (sin código):</label>
  <input type="text" id="numero" placeholder="Ej: 3225135816">

  <label for="tipo">Tipo de mensaje:</label>
  <select id="tipo">
    <option value="texto">📨 Texto personalizado</option>
    <option value="plantilla">📋 Plantilla WhatsApp</option>
  </select>

  <div id="mensajeBox">
    <label for="mensaje">Mensaje personalizado:</label>
    <textarea id="mensaje" rows="4" placeholder="Escribe el mensaje..."></textarea>
  </div>

  <div id="plantillaBox" style="display: none;">
    <label for="idioma">Idioma de plantilla:</label>
    <select id="idioma">
      <option value="es_ES">🇪🇸 Español</option>
      <option value="en_US">🇺🇸 Inglés</option>
    </select>

    <label for="plantilla">Nombre de la plantilla:</label>
    <select id="plantilla">
      <option value="hello_world">hello_world</option>
      <option value="envio">envio</option>
      <option value="nuevo_usuario_">nuevo_usuario_</option>
      <option value="codigo_de_verificacion">codigo_de_verificacion</option>
      <option value="pedido">pedido</option>
    </select>
  </div>

  <button onclick="enviar()">📤 Enviar Mensaje</button>
  <div class="resultado" id="resultado"></div>
</div>

<script>
  const accessToken = "EAAYUXix8hp8BOy8hrwrAUvH6wtbEO0McN4jluxJd66CP0wQxUdi6sO2JKkCHQD7UAFW7JsVgbUumRiKVqa3ZB4y8eY5Ft7aAgUh7Y9HjafEdNP3OXFxtEspQfZB3ri0kJiDq2qAs77Lps4YWqKQArQmuI95x6Vw2VpbINlolWWfVrtWOJVt11UXHqrbViOPgZDZD";
  const phoneNumberId = "138339792706281";

  document.getElementById("tipo").addEventListener("change", function () {
    const tipo = this.value;
    document.getElementById("mensajeBox").style.display = tipo === "texto" ? "block" : "none";
    document.getElementById("plantillaBox").style.display = tipo === "plantilla" ? "block" : "none";
  });

  async function enviar() {
    const codigo = document.getElementById("codigo").value;
    const numero = document.getElementById("numero").value.trim();
    const tipo = document.getElementById("tipo").value;
    const resultado = document.getElementById("resultado");

    resultado.innerText = "";

    if (!numero.match(/^\d+$/)) {
      resultado.innerText = "⚠️ El número debe contener solo dígitos.";
      resultado.style.color = "red";
      return;
    }

    const fullNumber = codigo + numero;

    let payload = {
      messaging_product: "whatsapp",
      to: fullNumber
    };

    if (tipo === "texto") {
      const mensaje = document.getElementById("mensaje").value.trim();
      if (!mensaje) {
        resultado.innerText = "⚠️ Escribe un mensaje.";
        resultado.style.color = "red";
        return;
      }
      payload.type = "text";
      payload.text = {
        body: mensaje,
        preview_url: false
      };
    } else {
      const idioma = document.getElementById("idioma").value;
      const plantilla = document.getElementById("plantilla").value;
      payload.type = "template";
      payload.template = {
        name: plantilla,
        language: { code: idioma }
      };
    }

    try {
      const res = await fetch(`https://graph.facebook.com/v17.0/${phoneNumberId}/messages`, {
        method: "POST",
        headers: {
          "Authorization": `Bearer ${accessToken}`,
          "Content-Type": "application/json"
        },
        body: JSON.stringify(payload)
      });

      const data = await res.json();
      if (res.ok) {
        resultado.innerText = "✅ Mensaje enviado correctamente.";
        resultado.style.color = "green";
      } else {
        resultado.innerText = "❌ Error al enviar: " + (data?.error?.message || "Error desconocido");
        resultado.style.color = "red";
        console.warn(data);
      }
    } catch (e) {
      resultado.innerText = "❌ Error de red o API.";
      resultado.style.color = "red";
      console.error(e);
    }
  }
</script>

</body>
</html>