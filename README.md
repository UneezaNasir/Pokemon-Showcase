<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Pokémon Card Showcase - A reminiscene of childhood</title>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap" rel="stylesheet">
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      font-family: 'Poppins', sans-serif;
      background: radial-gradient(circle at top left, #f9fdff, #e2ebf0);
      padding: 3rem 1rem;
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 2rem;
      position: relative;
      overflow-x: hidden;
    }

    .doodle {
      position: absolute;
      opacity: 0.07;
      z-index: 0;
      animation: float 8s ease-in-out infinite;
    }

    .doodle img {
      max-width: 90px;
    }

    .d1 { top: 10%; left: 5%; animation-delay: 0s; }
    .d2 { top: 40%; right: 10%; animation-delay: 2s; }
    .d3 { bottom: 20%; left: 15%; animation-delay: 4s; }
    .d4 { top: 30%; left: 60%; animation-delay: 1s; }
    .d5 { bottom: 5%; right: 5%; animation-delay: 3s; }

    @keyframes float {
      0%, 100% { transform: translateY(0); }
      50% { transform: translateY(-10px); }
    }

    .mascot {
      position: fixed;
      bottom: 10px;
      right: 10px;
      width: 60px;
      height: auto;
      opacity: 0.9;
      animation: bounce 2s infinite ease-in-out;
      z-index: 10;
    }

    @keyframes bounce {
      0%, 100% { transform: translateY(0); }
      50% { transform: translateY(-10px); }
    }

    .card-wrapper {
      width: 260px;
      height: 380px;
      perspective: 1000px;
      z-index: 1;
    }

    .card {
      width: 100%;
      height: 100%;
      position: relative;
      transform-style: preserve-3d;
      transition: transform 0.8s;
    }

    .card-wrapper:hover .card {
      transform: rotateY(180deg);
    }

    .card-face {
      position: absolute;
      width: 100%;
      height: 100%;
      backface-visibility: hidden;
      border-radius: 12px;
      padding: 1rem;
      box-shadow: 0 8px 16px rgba(0, 0, 0, 0.15);
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      text-align: center;
      color: #333;
      border: 3px solid;
    }

    .card-front img {
      width: 100%;
      border-radius: 10px;
      margin-bottom: 1rem;
    }

    .flip-note {
      font-size: 0.8rem;
      opacity: 0.6;
      margin-top: auto;
    }

    .card-back {
      transform: rotateY(180deg);
    }

    .fire { background: #ffe5e0; border-color: #ff6347; }
    .water { background: #e0f0ff; border-color: #47b2ff; }
    .grass { background: #e7fce8; border-color: #7C6F45; }
    .electric { background: #fff9d6; border-color: #ffeb3b; }
    .psychic { background: #f3e8ff; border-color: #a16dfb; }
    .darkness { background: #e8e8e8; border-color: #555; }
    .fairy { background: #ffe7f8; border-color: #f97fcc; }
    .normal { background: #f5f5f5; border-color: #999; }
    .fighting { background: #fcefe0; border-color: #b76b29; }
    .metal { background: #f0f0f0; border-color: #aaa; }
    .dragon { background: #e6f5ff; border-color: #6b92c7; }
    .default { background: #ffffff; border-color: #ccc; }

    footer {
      width: 100%;
      text-align: center;
      margin-top: 3rem;
      font-size: 0.9rem;
      color: #777;
    }
  </style>
</head>
<body>
  <!-- Doodles -->
  <div class="doodle d1"><img src="https://img.icons8.com/doodle/96/pokeball.png" /></div>
  <div class="doodle d2"><img src="https://img.icons8.com/doodle/96/star--v1.png" /></div>
  <div class="doodle d3"><img src="https://img.icons8.com/doodle/96/cloud.png" /></div>
  <div class="doodle d4"><img src="https://img.icons8.com/doodle/96/heart.png" /></div>
  <div class="doodle d5"><img src="https://img.icons8.com/doodle/96/lightning-bolt.png" /></div>

  <!-- Pikachu Mascot -->
  <img class="mascot" src= "c:\Users\DANISH TRADERS\Downloads\pikachu.png"  alt="pikachu"/>

  <script>
    const typeClasses = {
      Fire: "fire",
      Water: "water",
      Grass: "grass",
      Lightning: "electric",
      Psychic: "psychic",
      Darkness: "darkness",
      Fairy: "fairy",
      Normal: "normal",
      Fighting: "fighting",
      Metal: "metal",
      Dragon: "dragon"
    };

    fetch('https://api.pokemontcg.io/v2/cards?pageSize=40')
      .then(res => res.json())
      .then(data => {
        data.data.forEach(card => {
          const type = card.types ? card.types[0] : "default";
          const cardTypeClass = typeClasses[type] || "default";

          const wrapper = document.createElement('div');
          wrapper.className = 'card-wrapper';

          const cardDiv = document.createElement('div');
          cardDiv.className = `card`;

          const abilities = card.abilities?.length
            ? card.abilities.map(ab => `<p><strong>${ab.name}:</strong> ${ab.text}</p>`).join('')
            : '';

          cardDiv.innerHTML = `
            <div class="card-face card-front ${cardTypeClass}">
              <img src="${card.images.large}" alt="${card.name}">
              <p class="flip-note">🔁 Flip Me</p>
            </div>
            <div class="card-face card-back ${cardTypeClass}">
              ${abilities}
              <p><strong>Type:</strong> ${type}</p>
              ${card.set ? `<p><strong>Set:</strong> ${card.set.name}</p>` : ''}
              ${card.hp ? `<p><strong>HP:</strong> ${card.hp}</p>` : ''}
            </div>
          `;

          wrapper.appendChild(cardDiv);
          document.body.appendChild(wrapper);
        });
      });
  </script>

  <footer>
   Retreving The Memories
  </footer>
</body>
</html>

