# Jul
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Insta Jul x Kimberley</title>
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
            background: linear-gradient(45deg, #f09433 0%, #e6683c 25%, #dc2743 50%, #cc2366 75%, #bc1888 100%);
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            overflow: hidden;
        }

        #phone {
            width: 360px;
            height: 640px;
            background: #fff;
            border-radius: 40px;
            box-shadow: 0 25px 50px rgba(0,0,0,0.4);
            display: flex;
            flex-direction: column;
            overflow: hidden;
            position: relative;
            border: 8px solid #222;
        }

        /* HEADER */
        header {
            padding: 20px 15px 10px;
            display: flex;
            align-items: center;
            border-bottom: 1px solid #efefef;
            background: #fff;
            z-index: 10;
        }

        .story-ring {
            width: 56px;
            height: 56px;
            background: linear-gradient(45deg, #f09433, #bc1888);
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            margin-right: 12px;
            cursor: pointer;
        }

        .profile-pic {
            width: 50px;
            height: 50px;
            border-radius: 50%;
            background: white;
            padding: 2px;
            object-fit: cover;
        }

        .user-meta h3 { margin: 0; font-size: 15px; font-weight: 600; }
        .user-meta p { margin: 0; font-size: 12px; color: #8e8e8e; }

        /* CHAT AREA */
        #chat-area {
            flex: 1;
            padding: 15px;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            gap: 10px;
            background: #fff;
        }

        .bubble {
            max-width: 85%;
            padding: 12px 16px;
            border-radius: 20px;
            font-size: 14px;
            background: #efefef;
            color: #262626;
            animation: fadeIn 0.4s ease forwards;
            line-height: 1.5;
        }

        .mention { color: #0095f6; font-weight: 600; }

        /* OVERLAY STORIES */
        #story-overlay {
            position: absolute;
            top: 0; left: 0; width: 100%; height: 100%;
            background: #000;
            z-index: 100;
            display: none;
            flex-direction: column;
        }

        .story-img { width: 100%; height: 100%; object-fit: cover; }

        .story-header {
            position: absolute;
            top: 15px; width: 100%; display: flex;
            padding: 0 10px; box-sizing: border-box; gap: 4px; z-index: 110;
        }

        .story-bar { flex: 1; height: 2px; background: rgba(255,255,255,0.3); }
        .story-bar.active { background: #fff; }

        .story-footer {
            position: absolute;
            bottom: 60px; width: 100%; text-align: center;
            color: white; padding: 0 20px; box-sizing: border-box;
            font-weight: bold; text-shadow: 0 2px 8px rgba(0,0,0,0.9);
            z-index: 110; font-size: 18px;
        }

        .close-story { position: absolute; top: 30px; right: 20px; color: white; font-size: 35px; cursor: pointer; z-index: 120; }

        .nav-zone { position: absolute; top: 0; width: 50%; height: 100%; z-index: 105; }
        #next-zone { right: 0; }
        #prev-zone { left: 0; }

        /* FOOTER */
        .footer { padding: 15px; border-top: 1px solid #efefef; }
        #btn-main {
            background: linear-gradient(to right, #f09433, #bc1888);
            color: white; border: none; padding: 14px;
            border-radius: 12px; width: 100%; font-weight: bold; cursor: pointer;
        }

        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
    </style>
</head>
<body>

<div id="phone">
    <div id="story-overlay">
        <div class="story-header" id="bars"></div>
        <span class="close-story" onclick="closeStory()">&times;</span>
        <div class="nav-zone" id="prev-zone" onclick="changeStory(-1)"></div>
        <div class="nav-zone" id="next-zone" onclick="changeStory(1)"></div>
        <img id="story-viewer" class="story-img" src="">
        <div class="story-footer" id="story-text"></div>
    </div>

    <header>
        <div class="story-ring" onclick="openStory()">
            <img src="Jul.png" class="profile-pic" onerror="this.src='https://via.placeholder.com/50?text=Jul'">
        </div>
        <div class="user-meta">
            <h3>Jul ✅</h3>
            <p id="writing-status">D'or et de Platine</p>
        </div>
    </header>

    <div id="chat-area">
        <div style="text-align:center; font-size:11px; color:#999; margin:10px 0;">AUJOURD'HUI</div>
    </div>

    <div class="footer">
        <button id="btn-main" onclick="startChat()">Ouvrir le message pour Kimberley 🤍</button>
    </div>
</div>

<script>
    const chatArea = document.getElementById('chat-area');
    const storyTextElement = document.getElementById('story-text');
    const statusText = document.getElementById('writing-status');
    
    const stories = [
        { img: "Jul1.png", txt: "Bon anniversaire à la zone ! <br><span style='color:#00d2ff'>@Kimberley Dehaies</span> 🎂" },
        { img: "Jul2.png", txt: "Force à toi <span style='color:#00d2ff'>@Kimberley Dehaies</span>, reste vraie ! ✨" },
        { img: "Jul3.png", txt: "C'est ton jour Kimberley ! <br>Et mercé pour le soutien 🤘⚡" }
    ];

    let currentStory = 0;

    // MESSAGE LONG ET TOUCHANT
    const messages = [
        "Wesh <span class='mention'>@Kimberley Dehaies</span>, c'est le J ! 👽",
        "J'voulais prendre une minute pour t'écrire un vrai truc, pas juste deux mots comme ça. J'espère que tu vas bien et que tu profites de ta journée à fond.",
        "Aujourd'hui c'est ton anniversaire, et j'voulais te dire que la Team est fière d'avoir des personnes comme toi. T'es une fille en or, Kimberley, avec un grand coeur, et dans ce monde c'est rare de rester aussi vraie. ✨",
        "Lâche rien dans tes projets, reste forte même quand c'est dur. T'as une bonne étoile au-dessus de toi et elle te lâchera pas. Oublie pas que c'est pas des LOL, le travail et la bonté ça finit toujours par payer.",
        "Profite bien de tes proches aujourd'hui, crée des bons souvenirs. On vit pour ces moments-là. 🎂",
        "Encore un joyeux anniversaire <span class='mention'>@Kimberley Dehaies</span>. Merci pour tout le soutien que tu donnes, ça me touche vraiment la vérité. On est ensemble, ça bouge pas. ❤️",
        "Gros bisous de Marseille, prends soin de toi. D'or et de Platine bébé ! 💿💿💿"
    ];

    function startChat() {
        let i = 0;
        document.getElementById('btn-main').disabled = true;
        document.getElementById('btn-main').innerText = "Envoi en cours...";
        
        const interval = setInterval(() => {
            if (i < messages.length) {
                statusText.innerText = "En train d'écrire...";
                const div = document.createElement('div');
                div.className = 'bubble';
                div.innerHTML = messages[i];
                chatArea.appendChild(div);
                chatArea.scrollTop = chatArea.scrollHeight;
                i++;
            } else { 
                clearInterval(interval); 
                statusText.innerText = "D'or et de Platine";
                document.getElementById('btn-main').innerText = "Message lu ✅";
            }
        }, 2500); // Délai plus long pour simuler l'écriture d'un long texte
    }

    function openStory() {
        document.getElementById('story-overlay').style.display = 'flex';
        currentStory = 0;
        showStory();
    }

    function closeStory() { document.getElementById('story-overlay').style.display = 'none'; }

    function showStory() {
        const img = document.getElementById('story-viewer');
        img.src = stories[currentStory].img;
        storyTextElement.innerHTML = stories[currentStory].txt;
        renderBars();
    }

    function changeStory(dir) {
        currentStory += dir;
        if (currentStory < 0) currentStory = 0;
        if (currentStory >= stories.length) { closeStory(); return; }
        showStory();
    }

    function renderBars() {
        const barsCont = document.getElementById('bars');
        barsCont.innerHTML = '';
        stories.forEach((_, idx) => {
            const bar = document.createElement('div');
            bar.className = 'story-bar' + (idx <= currentStory ? ' active' : '');
            barsCont.appendChild(bar);
        });
    }
</script>

</body>
</html>
