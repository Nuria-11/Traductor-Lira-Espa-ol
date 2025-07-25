<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Traductor Lira</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background: #f7f4f9;
            color: #333;
            margin: 0;
            padding: 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: flex-start;
            min-height: 100vh;
        }
        header {
            background: #c9e4de;
            width: 100%;
            padding: 1rem;
            text-align: center;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }
        main {
            margin-top: 2rem;
            background: #fff;
            padding: 2rem;
            border-radius: 12px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
            width: 90%;
            max-width: 700px;
        }
        select, textarea, button {
            width: 100%;
            margin-top: 1rem;
            padding: 0.8rem;
            border: 1px solid #ccc;
            border-radius: 8px;
            font-size: 1rem;
        }
        button {
            background: #f9c6c9;
            border: none;
            cursor: pointer;
            transition: background 0.3s;
        }
        button:hover {
            background: #f7a8ad;
        }
        #output {
            background: #f3eac2;
            min-height: 100px;
            margin-top: 1rem;
            padding: 1rem;
            border-radius: 8px;
            white-space: pre-wrap;
        }
    </style>
</head>
<body>
    <header>
        <h1>Traductor Español ↔ Lira</h1>
    </header>
    <main>
        <select id="direction">
            <option value="es-lira">Español → Lira</option>
            <option value="lira-es">Lira → Español</option>
        </select>
        <textarea id="input" rows="5" placeholder="Escribe el texto aquí..."></textarea>
        <button onclick="translateText()">Traducir</button>
        <div id="output" placeholder="Resultado de la traducción..."></div>
    </main>
    <script>
        const diccionario = {
            "hola": "lumi", "adios": "seran", "gracias": "thir",
            "por": "mel", "favor": "lan", "por favor": "melan",
            "si": "noa", "no": "nei", "por qué": "parqa",
            "qué": "kai", "quién": "kuen", "dónde": "domer",
            "cómo": "kuma", "cuándo": "kwen", "cuál": "kualo",
            "porque": "parke", "yo": "mi", "tú": "ti",
            "él": "il", "ella": "ila", "nosotros": "nosa",
            "ustedes": "usta", "ellos": "ilos", "me": "me",
            "te": "te", "se": "se", "llamo": "lamo",
            "nombre": "nomen", "es": "es", "soy": "soi",
            "estar": "esta", "tener": "hav", "hacer": "krea",
            "ir": "van", "venir": "vena", "comer": "gresta",
            "beber": "drinki", "casa": "domi", "escuela": "skola",
            "libro": "buka", "mesa": "taba", "silla": "chera",
            "puerta": "gaten", "ventana": "fenera", "perro": "kano",
            "gato": "feli", "pájaro": "avina", "pez": "fisha",
            "caballo": "horsa", "vaca": "muka", "oveja": "shepa",
            "conejo": "lopa", "ratón": "mika", "amarillo": "yelo",
            "azul": "blu", "rojo": "roja", "verde": "virda",
            "negro": "nira", "blanco": "blana", "gris": "grisa",
            "marrón": "bruna", "rosa": "roza", "naranja": "oranja",
            "feliz": "hapi", "triste": "sori", "enojado": "grun",
            "cansado": "tira", "hermoso": "belia", "feo": "ugli",
            "grande": "granda", "pequeño": "smala", "rápido": "swift",
            "lento": "slen", "día": "dian", "noche": "nokti",
            "mañana": "morra", "tarde": "vesta", "hoy": "hodia",
            "ayer": "yara", "mañana(futuro)": "morna", "agua": "liva",
            "pan": "breti", "carne": "meata", "fruta": "fruita",
            "verdura": "verdura", "coche": "caria", "camino": "roda",
            "ciudad": "civita", "pueblo": "vilia", "trabajo": "jobi",
            "dinero": "mona", "amigo": "fren", "amigos": "frens",
            "amor": "lufa", "mujer": "fema", "hombre": "mano",
            "niño": "kido", "niña": "kida", "familia": "famila",
            "hermano": "brato", "hermana": "sista", "madre": "modra",
            "padre": "fodra", "abuela": "granma", "abuelo": "granpa",
            ".": ".", ",": ",", "?": "?", "¿": "¿",
            "!": "!", "¡": "¡"
        };

        const diccionarioInvertido = {};
        for (const [es, lira] of Object.entries(diccionario)) {
            diccionarioInvertido[lira] = es;
        }

        function translateText() {
            const direction = document.getElementById('direction').value;
            const inputText = document.getElementById('input').value;
            const words = inputText.split(/(\s+|(?=[.,?!¡¿])|(?<=[.,?!¡¿]))/);
            let result = "";

            if (direction === "es-lira") {
                result = words.map(w => diccionario[w.toLowerCase()] || w).join("");
            } else {
                result = words.map(w => diccionarioInvertido[w.toLowerCase()] || w).join("");
            }

            document.getElementById('output').innerText = result;
        }
    </script>
</body>
</html>
