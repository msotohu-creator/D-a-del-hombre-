# D-a-del-hombre-
Día del hombre
<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Tarjeta interactiva - Feliz Día del Hombre</title>
  <style>
    :root{--bg:#0b1020;--card:#fff;--accent:#9adf6b}
    html,body{height:100%;margin:0;font-family:Inter, system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial}
    body{display:grid;place-items:center;background:linear-gradient(180deg,#071023 0%, #0b1630 100%);color:var(--card)}
    .container{max-width:980px;width:92%;display:grid;grid-template-columns:repeat(auto-fit,minmax(260px,1fr));gap:24px}

    /* Tarjeta (preview) */
    .card{background:linear-gradient(180deg, rgba(255,255,255,0.06), rgba(255,255,255,0.03));border-radius:18px;padding:20px;box-shadow:0 8px 30px rgba(2,6,23,0.6);cursor:pointer;transition:transform .22s;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:12px;text-align:center}
    .card:hover{transform:translateY(-6px) scale(1.02)}
    .card .titulo{font-size:20px;font-weight:700}
    .card .sub{opacity:.9;font-size:14px}

    /* Flores - SVG container */
    .flowers{width:120px;height:120px}

    /* Modal (carta digital) */
    .overlay{position:fixed;inset:0;background:linear-gradient(0deg, rgba(2,6,23,0.86), rgba(2,6,23,0.5));display:none;align-items:center;justify-content:center;padding:24px;z-index:60}
    .overlay.show{display:flex}
    .modal{background:linear-gradient(180deg,#fff 0%, #f5fff9 100%);color:#061017;border-radius:14px;max-width:880px;width:100%;box-shadow:0 20px 60px rgba(2,6,23,0.6);padding:26px;position:relative}
    .modal .close{position:absolute;right:12px;top:12px;background:transparent;border:0;font-size:18px;cursor:pointer}

    .letter{display:grid;grid-template-columns:1fr 320px;gap:20px}
    @media(max-width:880px){.letter{grid-template-columns:1fr;}}

    .letter .texto{padding:12px 18px;border-radius:10px;background:linear-gradient(180deg, rgba(10,20,12,0.02), rgba(10,20,12,0.01));}
    .letter h1{margin:6px 0 12px;font-size:30px;color:var(--accent)}
    .letter p{line-height:1.6;color:#163235}

    .side{display:flex;flex-direction:column;align-items:center;gap:12px}
    .side .flower-wrap{background:linear-gradient(180deg,#e9fff0,#f9fff9);padding:14px;border-radius:12px}
    .side .message{font-weight:700}

    /* Play button */
    .play-btn{display:inline-flex;align-items:center;gap:10px;padding:10px 14px;border-radius:999px;border:0;background:linear-gradient(90deg,var(--accent),#6ec84b);color:#052010;font-weight:700;cursor:pointer}
    .muted{opacity:.6;font-size:13px}

    footer.small{font-size:12px;color:#6b6f72;margin-top:18px;text-align:center}

    /* subtle animation for flowers */
    .flower-cluster{transform-origin:center center;animation:float 6s ease-in-out infinite}
    @keyframes float{0%{transform:translateY(0)}50%{transform:translateY(-6px)}100%{transform:translateY(0)}}
  </style>
</head>
<body>
  <div class="container">
    <!-- Tarjeta que actúa como link (user gesture) -->
    <div class="card" id="openCard" aria-role="button" tabindex="0">
      <!-- SVG de hortensias verdes estilizadas -->
      <svg class="flowers" viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg" aria-hidden>
        <defs>
          <radialGradient id="g1" cx="30%" cy="30%">
            <stop offset="0%" stop-color="#e6f9de"/>
            <stop offset="100%" stop-color="#66b84a"/>
          </radialGradient>
        </defs>
        <g class="flower-cluster" transform="translate(20,20)">
          <!-- cluster 1 -->
          <circle cx="30" cy="30" r="18" fill="url(#g1)" opacity="0.95" />
          <circle cx="52" cy="26" r="14" fill="#8bd56a" opacity="0.95" />
          <circle cx="18" cy="52" r="12" fill="#65a94a" opacity="0.95" />
          <circle cx="46" cy="52" r="10" fill="#8fde7b" opacity="0.95" />
        </g>
        <g class="flower-cluster" transform="translate(90,40)">
          <circle cx="30" cy="30" r="18" fill="#7fcf68"/>
          <circle cx="52" cy="26" r="14" fill="#64b24b"/>
          <circle cx="18" cy="52" r="12" fill="#9feaa0"/>
        </g>
        <g class="flower-cluster" transform="translate(45,90)">
          <circle cx="30" cy="30" r="20" fill="#6fbf58"/>
          <circle cx="60" cy="30" r="13" fill="#4e9d34"/>
          <circle cx="18" cy="52" r="11" fill="#a8f2a6"/>
        </g>
      </svg>

      <div class="titulo">Feliz Día del Hombre</div>
      <div class="sub">Haz click para abrir tu carta digital</div>
    </div>
  </div>

  <!-- Modal: carta digital -->
  <div class="overlay" id="overlay">
    <div class="modal" role="dialog" aria-modal="true" aria-labelledby="modalTitle">
      <button class="close" id="closeBtn" aria-label="Cerrar">✕</button>
      <div class="letter">
        <div class="texto">
          <h1 id="modalTitle">Feliz Día del Hombre</h1>
          <p>Querido (nombre),</p>
          <p>Hoy quiero regalarte este pequeño gesto: unas flores y unas palabras para decirte lo especial que eres. Que este día te recuerde lo valiente, tierno y fuerte que puedes ser. Gracias por ser luz y compañía.</p>
          <p class="muted">(Haz click en reproducir para escuchar la canción mientras lees.)</p>

          <div style="margin-top:14px;display:flex;gap:10px;flex-wrap:wrap;align-items:center">
            <button class="play-btn" id="play">▶ Reproducir canción</button>
            <button class="play-btn" id="pause" style="display:none">⏸ Pausar</button>
          </div>

          <footer class="small">Puedes descargar o compartir esta tarjeta copiando la URL de tu navegador.</footer>
        </div>

        <aside class="side">
          <div class="flower-wrap">
            <!-- Repetición decorativa de la flor -->
            <svg width="220" height="220" viewBox="0 0 120 120" xmlns="http://www.w3.org/2000/svg" aria-hidden>
              <g transform="translate(10,10)">
                <circle cx="20" cy="20" r="12" fill="#7fcf68" />
                <circle cx="40" cy="16" r="10" fill="#66b84a" />
                <circle cx="16" cy="44" r="9" fill="#9feaa0" />
                <circle cx="44" cy="44" r="11" fill="#4e9d34" />
              </g>
            </svg>
          </div>
          <div class="message">Feliz día</div>
          <div class="muted">Comparte esta carta con quien quieras</div>
        </aside>
      </div>

      <!-- Hidden: iframe para reproducir la canción. Se carga con autoplay SOLO cuando el usuario hace click -->
      <div id="playerHolder" style="display:none;margin-top:12px">
        <!-- Sustituye YOUTUBE_ID por el id del video de YouTube de "Ojos Color Sol - Calle 13" -->
        <iframe id="ytplayer" width="0" height="0" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
      </div>
    </div>
  </div>

  <script>
    // --- Config: reemplaza con el video de YouTube deseado ---
    // Busca el video en YouTube, copia el ID (la parte después de "v=") y pégalo aquí.
    const YOUTUBE_ID = 'REEMPLAZA_POR_ID_DEL_VIDEO';
    // Ejemplo de URL final que se cargará al hacer click:
    // https://www.youtube.com/embed/{YOUTUBE_ID}?autoplay=1&rel=0&modestbranding=1

    const openCard = document.getElementById('openCard');
    const overlay = document.getElementById('overlay');
    const closeBtn = document.getElementById('closeBtn');
    const play = document.getElementById('play');
    const pause = document.getElementById('pause');
    const playerHolder = document.getElementById('playerHolder');
    const ytplayer = document.getElementById('ytplayer');

    function openLetter(){
      overlay.classList.add('show');
      // focus for accessibility
      closeBtn.focus();
    }
    function closeLetter(){
      overlay.classList.remove('show');
      // stop audio by removing src
      ytplayer.src = '';
      play.style.display = '';
      pause.style.display = 'none';
    }

    openCard.addEventListener('click', (e)=>{
      openLetter();
    });
    openCard.addEventListener('keydown', (e)=>{ if(e.key==='Enter' || e.key===' ') openLetter(); });
    closeBtn.addEventListener('click', closeLetter);

    // Play button: set iframe src on user gesture so autoplay is allowed
    play.addEventListener('click', ()=>{
      if(!YOUTUBE_ID || YOUTUBE_ID.includes('REEMPLAZA')){
        alert('Por favor reemplaza la constante YOUTUBE_ID en el código por el ID del video de YouTube de "Ojos Color Sol - Calle 13".\n\nInstrucciones:\n1) Ve a YouTube y busca "Ojos Color Sol Calle 13".\n2) Abre el video oficial y copia la parte del enlace después de "v=".\n3) Pega ese ID en el archivo HTML donde aparece REEMPLAZA_POR_ID_DEL_VIDEO.');
        return;
      }
      // build embed url with autoplay
      const url = `https://www.youtube.com/embed/${YOUTUBE_ID}?autoplay=1&rel=0&modestbranding=1`;
      ytplayer.src = url;
      playerHolder.style.display = 'block';
      play.style.display = 'none';
      pause.style.display = '';
    });

    pause.addEventListener('click', ()=>{
      // stopping by removing src
      ytplayer.src = '';
      play.style.display = '';
      pause.style.display = 'none';
    });

    // allow closing the modal with Escape
    document.addEventListener('keydown', (e)=>{ if(e.key==='Escape'){ closeLetter(); } });

    // --- End of script ---
  </script>
</body>
</html>
