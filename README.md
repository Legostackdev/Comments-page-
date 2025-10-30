# Comments-page-
<!DOCTYPE html><html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Comments Page</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 20px;
      background: #c8e6c9; /* tablero escolar verde claro */ /* azul claro escolar */ /* fondo suave basado en colores del logo */
    }
    .container {
      max-width: 600px;
      margin: auto;
      background: #fff;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }
    h1 {
      text-align: center;
    }
    textarea {
      width: 100%;
      height: 100px;
      padding: 10px;
      margin-bottom: 10px;
      border-radius: 5px;
      border: 1px solid #ccc;
      resize: none;
    }
    button {
      width: 100%;
      padding: 10px;
      background: #007bff;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    button:hover {
      background: #0056b3;
    }
    .comment {
      background: #fffbe0;
      padding: 10px;
      margin-top: 10px;
      border-radius: 5px;
      border: 1px dashed #ffcc66; /* estilo cuaderno */
      font-family: "Comic Sans MS", cursive;
    }
    body.chalk-mode { background:#2e5b2e; color:#f4f4f4; font-family:"Courier New", monospace; }
    .comment.chalk { background:#3f704d; border:1px solid #9ccc65; color:white; }
    .mode-toggle { background:#388e3c; color:white; margin-bottom:10px; border-radius:6px; padding:8px; }
  </style>
</head>
<body>
  <button class="mode-toggle" onclick="toggleTheme()">🌙 Modo oscuro / tablero</button>
  <div class="container">
    <h1>Página de Comentarios - Estudiantes</h1>
    <input id="nameInput" type="text" placeholder="Tu nombre" style="width:100%;padding:10px;margin-bottom:10px;border-radius:5px;border:1px solid #ccc;" />
    <textarea id="commentInput" placeholder="Escribe tu comentario..."></textarea>
    <button onclick="addComment()">Enviar</button>
    <div id="commentsSection"></div>
    <button onclick="exportComments()" style="margin-top:10px;background:#28a745;">Guardar comentarios en archivo</button>
  </div>  <script>
    const badWords = ["tonto","feo","idiota","estupido","bobo","malo","wtf","caca","mierda","pendejo","stupid","dumb","💩","🤬"]; // lista básica

    function cleanText(text) {
      let cleaned = text;
      badWords.forEach(word => {
        const regex = new RegExp(word, "gi");
        cleaned = cleaned.replace(regex, "***");
      });
      return cleaned;
    }

    function saveComments() {
      localStorage.setItem("comments", document.getElementById("commentsSection").innerHTML);
    }

    function exportComments() {
      const data = document.getElementById("commentsSection").innerHTML;
      const blob = new Blob([data], {type: "text/html"});
      const url = URL.createObjectURL(blob);
      const a = document.createElement("a");
      a.href = url;
      a.download = "comentarios.html";
      a.click();
      URL.revokeObjectURL(url);
    }

    function loadComments() {
      const saved = localStorage.getItem("comments");
      if (saved) document.getElementById("commentsSection").innerHTML = saved;
    }
    loadComments();

    const avatars = ["👩🏻‍🎓","👨🏽‍🎓","👧🏼","👦🏾","🧒🏻","👩🏾‍💻","👨🏼‍💻"]; // avatares escolares
    function getAvatar(){ return avatars[Math.floor(Math.random()*avatars.length)]; }

    function toggleTheme(){ document.body.classList.toggle("chalk-mode"); document.querySelectorAll('.comment').forEach(c=>c.classList.toggle('chalk')); }

    function addComment() {
      const name = document.getElementById("nameInput").value.trim();
      const input = document.getElementById("commentInput");
      const text = input.value.trim();
      if (text === "") return;

      const safeText = cleanText(text);
      const displayName = name !== "" ? name : "Estudiante";
      const time = new Date().toLocaleString();

      const commentDiv = document.createElement("div");
      commentDiv.className = "comment";
      commentDiv.innerHTML = `${getAvatar()} <strong>${displayName}</strong> <small>(${time})</small><br>${safeText}<br>
      <button onclick="this.innerHTML='👍'">👍</button> <button onclick="this.innerHTML='👏'">👏</button> <button onclick="alert('✅ Gracias, reportado al profesor.')">🚩 Reportar</button>`;

      commentDiv.style.opacity = 0;
      commentDiv.style.transform = "translateY(10px)";
      document.getElementById("commentsSection").appendChild(commentDiv);

      setTimeout(() => {
        commentDiv.style.transition = "all 0.4s";
        commentDiv.style.opacity = 1;
        commentDiv.style.transform = "translateY(0)";
      }, 10);

      input.value = "";
      document.getElementById("nameInput").value = "";
      saveComments();
    }
</script></body>
</html>

