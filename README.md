<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Traductor Lira</title>
    <style>
        body {
            margin: 0;
            font-family: Arial, sans-serif;
            background-color: #ffe792;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: flex-start;
            min-height: 100vh;
        }
        h1 {
            background-color: #f7b8f3;
            padding: 10px 40px;
            margin-top: 30px;
            border-radius: 10px;
            box-shadow: 0 3px 6px rgba(0,0,0,0.2);
        }
        .container {
            display: flex;
            flex-direction: column;
            align-items: center;
            margin-top: 40px;
            gap: 20px;
        }
        .labels {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 50px;
            font-size: 1.4rem;
            font-weight: bold;
        }
        .swap {
            cursor: pointer;
            font-size: 1.5rem;
            transition: transform 0.3s ease;
        }
        .swap:hover {
            transform: scale(1.2) rotate(180deg);
        }
        .areas {
            display: flex;
            gap: 50px;
        }
        textarea {
            width: 250px;
            height: 150px;
            background-color: #f7b8f3;
            border: 2px solid #333;
            border-radius: 10px;
            padding: 10px;
            font-size: 1rem;
            resize: none;
            box-shadow: 0 2px 5px rgba(0,0,0,0.2);
        }
        .actions {
            display: flex;
            align-items: center;
            gap: 20px;
            margin-top: 10px;
        }
        .btn {
            padding: 10px 25px;
            background-color: #f7b8f3;
            border: none;
            border-radius: 30px;
            font-size: 1.2rem;
            cursor: pointer;
            box-shadow: 0 3px 6px rgba(0,0,0,0.2);
            transition: background-color 0.3s ease, transform 0.2s ease;
        }
        .btn:hover {
            background-color: #f49cf0;
            transform: scale(1.05);
        }
        .icon-btn {
            cursor: pointer;
            font-size: 1.5rem;
            border: none;
            background: none;
        }
    </style>
</head>
<body>
    <h1>Traducir</h1>
    <div class="container">
        <div class="labels">
            <span id="label1">Español</span>
            <span class="swap" onclick="swapLanguages()">⇆</span>
            <span id="label2">Lira</span>
        </div>
        <div class="areas">
            <textarea id="inputText" placeholder="Introducir texto..."></textarea>
            <textarea id="outputText" readonly></textarea>
        </div>
        <div class="actions">
            <button class="btn" onclick="translateText()">Traducir</button>
            <button class="icon-btn" onclick="copyOutput()">📄 Copiar</button>
            <button class="icon-btn" onclick="speakOutput()">🔊</button>
        </div>
    </div>

    <script>
        let esToLira = true;

        const diccionario = {
            "hola": "lumi","adios": "seran","gracias": "thir",
            "por": "mel","favor": "lan","por favor": "melan",
            "si": "noa","no": "nei","por qué": "parqa",
            "qué": "kai","quién": "kuen","dónde": "domer",
            "cómo": "kuma","cuándo": "kwen","cuál": "kualo",
            "porque": "parke","yo": "mi","tú": "ti",
            "él": "il","ella": "ila","nosotros": "nosa",
            "ustedes": "usta","ellos": "ilos","me": "me",
            "te": "te","se": "se","llamo": "lamo",
            "nombre": "nomen","es": "es","soy": "soi",
            "eres": "eres","estar": "esta","tener": "hav",
            "hacer": "krea","ir": "van","venir": "vena",
            "comer": "gresta","beber": "drinki","casa": "domi",
            "escuela": "skola","libro": "buka","mesa": "taba",
            "silla": "chera","puerta": "gaten","ventana": "fenera",
            "perro": "kano","gato": "feli","pájaro": "avina",
            "pez": "fisha","caballo": "horsa","vaca": "muka",
            "oveja": "shepa","conejo": "lopa","ratón": "mika",
            "amarillo": "yelo","azul": "blu","rojo": "roja",
            "verde": "virda","negro": "nira","blanco": "blana",
            "gris": "grisa","marrón": "bruna","rosa": "roza",
            "naranja": "oranja","feliz": "hapi","triste": "sori",
            "enojado": "grun","cansado": "tira","hermoso": "belia",
            "feo": "ugli","grande": "granda","pequeño": "smala",
            "rápido": "swift","lento": "slen","día": "dian",
            "noche": "nokti","mañana": "morra","tarde": "vesta",
            "hoy": "hodia","ayer": "yara","mañana(futuro)": "morna",
            "agua": "liva","pan": "breti","carne": "meata",
            "fruta": "fruita","verdura": "verdura","coche": "caria",
            "camino": "roda","ciudad": "civita","pueblo": "vilia",
            "trabajo": "jobi","dinero": "mona","amigo": "fren",
            "amigos": "frens","amor": "lufa","mujer": "fema",
            "hombre": "mano","niño": "kido","niña": "kida",
            "familia": "famila","hermano": "brato","hermana": "sista",
            "madre": "modra","padre": "fodra","abuela": "granma",
            "abuelo": "granpa","encantado": "hapi","encantada": "hapi",
            "conocerte": "konoserte","estás": "estas",
            ".": ".",",": ",","?": "?","¿": "¿","!": "!","¡": "¡"
        };

        const diccionarioInvertido = {};
        for (const [es, lira] of Object.entries(diccionario)) {
            diccionarioInvertido[lira] = es;
        }

        function swapLanguages() {
            esToLira = !esToLira;
            document.getElementById('label1').innerText = esToLira ? "Español" : "Lira";
            document.getElementById('label2').innerText = esToLira ? "Lira" : "Español";
            document.getElementById('inputText').value = "";
            document.getElementById('outputText').value = "";
        }

        function translateText() {
            const text = document.getElementById('inputText').value;
            const words = text.split(/(\s+|(?=[.,?!¡¿])|(?<=[.,?!¡¿]))/);
            let result = "";
            if (esToLira) {
                result = words.map(w => diccionario[w.toLowerCase()] || w).join("");
            } else {
                result = words.map(w => diccionarioInvertido[w.toLowerCase()] || w).join("");
            }
            document.getElementById('outputText').value = result;
        }

        function copyOutput() {
            const output = document.getElementById('outputText').value;
            navigator.clipboard.writeText(output).then(() => alert("Texto copiado"));
        }

        function speakOutput() {
            const output = document.getElementById('outputText').value;
            const utterance = new SpeechSynthesisUtterance(output);
            utterance.lang = esToLira ? "es-ES" : "es-ES";
            utterance.pitch = 1;
            utterance.rate = 1;
            speechSynthesis.speak(utterance);
        }
    </script>
</body>
</html>
