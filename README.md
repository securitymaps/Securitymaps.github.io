# Securitymaps.github.io
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Simple Browser - Security Maps</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      display: flex;
      flex-direction: column;
      height: 100vh;
    }
    #nav-bar {
      background: #4285F4;
      padding: 10px;
      display: flex;
      align-items: center;
      gap: 8px;
    }
    #nav-bar input[type="text"] {
      flex-grow: 1;
      padding: 8px;
      border: none;
      border-radius: 4px;
      font-size: 16px;
    }
    #nav-bar button {
      background: white;
      border: none;
      border-radius: 4px;
      padding: 8px 12px;
      cursor: pointer;
      font-weight: bold;
      color: #4285F4;
      transition: background 0.3s;
    }
    #nav-bar button:hover {
      background: #e0e0e0;
    }
    #browser-frame {
      flex-grow: 1;
      border: none;
      width: 100%;
    }
  </style>
</head>
<body>
  <div id="nav-bar">
    <button id="back-btn" title="Atrás" disabled>&larr;</button>
    <button id="forward-btn" title="Adelante" disabled>&rarr;</button>
    <button id="reload-btn" title="Recargar">&#x21bb;</button>
    <input type="text" id="url-input" placeholder="Ingresa una URL, por ejemplo: https://www.google.com" />
    <button id="go-btn" title="Ir">Ir</button>
  </div>
  <iframe id="browser-frame" src="https://www.google.com" sandbox="allow-scripts allow-same-origin allow-forms"></iframe>

  <script>
    const iframe = document.getElementById('browser-frame');
    const urlInput = document.getElementById('url-input');
    const backBtn = document.getElementById('back-btn');
    const forwardBtn = document.getElementById('forward-btn');
    const reloadBtn = document.getElementById('reload-btn');
    const goBtn = document.getElementById('go-btn');

    // Historial simple para navegación atrás/adelante
    let historyStack = ['https://www.google.com'];
    let historyIndex = 0;

    function updateButtons() {
      backBtn.disabled = historyIndex <= 0;
      forwardBtn.disabled = historyIndex >= historyStack.length - 1;
    }

    function navigate(url) {
      if (!url.startsWith('http://') && !url.startsWith('https://')) {
        url = 'https://' + url;
      }
      iframe.src = url;
      // Si navegamos a una nueva URL, cortamos el historial adelante
      historyStack = historyStack.slice(0, historyIndex + 1);
      historyStack.push(url);
      historyIndex++;
      urlInput.value = url;
      updateButtons();
    }

    goBtn.addEventListener('click', () => {
      const url = urlInput.value.trim();
      if (url) navigate(url);
    });

    urlInput.addEventListener('keydown', (e) => {
      if (e.key === 'Enter') {
        goBtn.click();
      }
    });

    backBtn.addEventListener('click', () => {
      if (historyIndex > 0) {
        historyIndex--;
        iframe.src = historyStack[historyIndex];
        urlInput.value = historyStack[historyIndex];
        updateButtons();
      }
    });

    forwardBtn.addEventListener('click', () => {
      if (historyIndex < historyStack.length - 1) {
        historyIndex++;
        iframe.src = historyStack[historyIndex];
        urlInput.value = historyStack[historyIndex];
        updateButtons();
      }
    });

    reloadBtn.addEventListener('click', () => {
      iframe.src = historyStack[historyIndex];
    });

    // Actualizar la barra de direcciones si el iframe cambia (limitado por CORS)
    iframe.addEventListener('load', () => {
      try {
        const currentUrl = iframe.contentWindow.location.href;
        urlInput.value = currentUrl;
      } catch (e) {
        // No se puede acceder por CORS, dejamos la URL actual
      }
    });

    // Inicializar
    updateButtons();
    urlInput.value = historyStack[historyIndex];
  </script>
</body>
</html>
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Simple Browser - Security Maps</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      display: flex;
      flex-direction: column;
      height: 100vh;
    }
    #nav-bar {
      background: #4285F4;
      padding: 10px;
      display: flex;
      align-items: center;
      gap: 8px;
    }
    #nav-bar input[type="text"] {
      flex-grow: 1;
      padding: 8px;
      border: none;
      border-radius: 4px;
      font-size: 16px;
    }
    #nav-bar button {
      background: white;
      border: none;
      border-radius: 4px;
      padding: 8px 12px;
      cursor: pointer;
      font-weight: bold;
      color: #4285F4;
      transition: background 0.3s;
    }
    #nav-bar button:hover {
      background: #e0e0e0;
    }
    #browser-frame {
      flex-grow: 1;
      border: none;
      width: 100%;
    }
  </style>
</head>
<body>
  <div id="nav-bar">
    <button id="back-btn" title="Atrás" disabled>&larr;</button>
    <button id="forward-btn" title="Adelante" disabled>&rarr;</button>
    <button id="reload-btn" title="Recargar">&#x21bb;</button>
    <input type="text" id="url-input" placeholder="Ingresa una URL, por ejemplo: https://www.google.com" />
    <button id="go-btn" title="Ir">Ir</button>
  </div>
  <iframe id="browser-frame" src="https://www.google.com" sandbox="allow-scripts allow-same-origin allow-forms"></iframe>

  <script>
    const iframe = document.getElementById('browser-frame');
    const urlInput = document.getElementById('url-input');
    const backBtn = document.getElementById('back-btn');
    const forwardBtn = document.getElementById('forward-btn');
    const reloadBtn = document.getElementById('reload-btn');
    const goBtn = document.getElementById('go-btn');

    // Historial simple para navegación atrás/adelante
    let historyStack = ['https://www.google.com'];
    let historyIndex = 0;

    function updateButtons() {
      backBtn.disabled = historyIndex <= 0;
      forwardBtn.disabled = historyIndex >= historyStack.length - 1;
    }

    function navigate(url) {
      if (!url.startsWith('http://') && !url.startsWith('https://')) {
        url = 'https://' + url;
      }
      iframe.src = url;
      // Si navegamos a una nueva URL, cortamos el historial adelante
      historyStack = historyStack.slice(0, historyIndex + 1);
      historyStack.push(url);
      historyIndex++;
      urlInput.value = url;
      updateButtons();
    }

    goBtn.addEventListener('click', () => {
      const url = urlInput.value.trim();
      if (url) navigate(url);
    });

    urlInput.addEventListener('keydown', (e) => {
      if (e.key === 'Enter') {
        goBtn.click();
      }
    });

    backBtn.addEventListener('click', () => {
      if (historyIndex > 0) {
        historyIndex--;
        iframe.src = historyStack[historyIndex];
        urlInput.value = historyStack[historyIndex];
        updateButtons();
      }
    });

    forwardBtn.addEventListener('click', () => {
      if (historyIndex < historyStack.length - 1) {
        historyIndex++;
        iframe.src = historyStack[historyIndex];
        urlInput.value = historyStack[historyIndex];
        updateButtons();
      }
    });

    reloadBtn.addEventListener('click', () => {
      iframe.src = historyStack[historyIndex];
    });

    // Actualizar la barra de direcciones si el iframe cambia (limitado por CORS)
    iframe.addEventListener('load', () => {
      try {
        const currentUrl = iframe.contentWindow.location.href;
        urlInput.value = currentUrl;
      } catch (e) {
        // No se puede acceder por CORS, dejamos la URL actual
      }
    });

    // Inicializar
    updateButtons();
    urlInput.value = historyStack[historyIndex];
  </script>
</body>
</html>

