<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>NRJ Tours - Écoute & plus</title>
  <style>
    body {
      margin: 0;
      font-family: 'Segoe UI', sans-serif;
      background: #f5f5f5;
      color: #333;
      text-align: center;
    }
    header {
      background: #ff003c;
      color: white;
      padding: 20px;
    }
    header img {
      width: 100px;
    }
    header h1 {
      margin: 10px 0 5px;
    }
    #date, #meteo, #actualites {
      font-size: 1.1rem;
      margin: 5px;
    }
    main {
      padding: 20px;
    }
    audio {
      width: 80%;
      max-width: 400px;
      margin: 20px 0;
    }
    .positives {
      background: #fff;
      border-radius: 8px;
      padding: 15px;
      margin: 20px auto;
      box-shadow: 0 2px 6px rgba(0,0,0,0.1);
      max-width: 600px;
      text-align: left;
    }
    #livreOr {
      background: #fff;
      border-radius: 8px;
      padding: 15px;
      margin: 20px auto;
      box-shadow: 0 2px 6px rgba(0,0,0,0.1);
      max-width: 600px;
      text-align: left;
    }
    #livreOr textarea {
      width: 100%;
      height: 80px;
      margin-bottom: 10px;
    }
    #livreOr button {
      background: #ff003c;
      color: white;
      border: none;
      padding: 10px 20px;
      border-radius: 4px;
      cursor: pointer;
    }
    #livreOr ul {
      list-style: none;
      padding: 0;
      max-height: 200px;
      overflow-y: auto;
    }
    footer {
      margin: 20px;
      font-size: 0.9rem;
      color: #777;
    }
  </style>
</head>
<body>
  <header>
    <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/1e/NRJ_Logo_2014.svg/512px-NRJ_Logo_2014.svg.png" alt="Logo NRJ">
    <h1>NRJ Tours</h1>
    <div id="date">📅 …</div>
    <div id="meteo">🌤️ …</div>
    <div id="actualites">✨ …</div>
  </header>

  <main>
    <audio controls autoplay>
      <source src="http://cdn.nrjaudio.fm/audio1/fr/40092/aac_64.mp3" type="audio/mpeg">
      Votre navigateur ne supporte pas l'audio HTML5.
    </audio>

    <section class="positives">
      <h2>🌟 Infos & bonnes nouvelles à Tours</h2>
      <ul id="positivesList">
        <li>Chargement...</li>
      </ul>
    </section>

    <section id="livreOr">
      <h2>📝 Livre d’or</h2>
      <textarea id="msg" placeholder="Laisse ton message ici…"></textarea><br>
      <button onclick="ajouterMessage()">Envoyer</button>
      <ul id="messagesList"></ul>
    </section>
  </main>

  <footer>© NRJ Tours — Page créée avec ❤️</footer>

  <script>
    // Date du jour
    function updateDate() {
      const d = new Date();
      document.getElementById('date').textContent =
        '📅 ' + d.toLocaleDateString('fr-FR', { weekday: 'long', day: 'numeric', month: 'long', year: 'numeric' });
    }
    updateDate();

    // Météo en temps réel
    function updateMeteo() {
      fetch('https://wttr.in/Tours?format=3')
        .then(res => res.text())
        .then(txt => document.getElementById('meteo').textContent = '🌤️ Météo : ' + txt)
        .catch(() => document.getElementById('meteo').textContent = '🌤️ Météo non disponible');
    }
    updateMeteo();

    // Infos positives dynamiques (exemple aléatoire)
    const exemples = [
      "Promenade au bord de la Loire recommandée ce jour pour sa douceur estivale !",
      "Visite du marché des Halles à privilégier pour ses produits locaux frais.",
      "Festival Jazz de Touraine : ambiance garantie cet été !",
      "Parc de la Gloriette : lieu paisible avec de magnifiques roses en fleurs."
    ];
    function updatePositives() {
      const ul = document.getElementById('positivesList');
      ul.innerHTML = '';
      exemples.sort(() => 0.5 - Math.random()).slice(0,3)
        .forEach(txt => {
          const li = document.createElement('li');
          li.textContent = txt;
          ul.appendChild(li);
        });
    }
    updatePositives();
    setInterval(updatePositives, 600000); // rafraîchit toutes les 10 min

    // Livre d’or (localStorage)
    const msgs = JSON.parse(localStorage.getItem('livreMessages') || '[]');
    const ulMsgs = document.getElementById('messagesList');
    function refreshMsgs() {
      ulMsgs.innerHTML = '';
      msgs.forEach(m => {
        const li = document.createElement('li');
        li.textContent = m;
        ulMsgs.appendChild(li);
      });
    }
    window.ajouterMessage = function() {
      const t = document.getElementById('msg');
      const v = t.value.trim();
      if(!v) return alert('Écris quelque chose :)');
      msgs.push(v);
      localStorage.setItem('livreMessages', JSON.stringify(msgs));
      t.value = '';
      refreshMsgs();
    }
    refreshMsgs();
  </script>
</body>
</html>
