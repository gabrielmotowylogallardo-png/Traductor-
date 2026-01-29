<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cifrador QWERTY Shift a Números</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background-color: #0a192f; /* Azul marino oscuro */
            color: #e6f1ff;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 20px;
        }
        
        .container {
            max-width: 800px;
            width: 100%;
            background-color: #112240;
            border-radius: 15px;
            padding: 30px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
            margin-top: 20px;
        }
        
        h1 {
            text-align: center;
            color: #64ffda;
            margin-bottom: 10px;
            font-size: 2.5rem;
            text-shadow: 0 2px 4px rgba(0, 0, 0, 0.5);
        }
        
        .subtitle {
            text-align: center;
            color: #8892b0;
            margin-bottom: 30px;
            font-size: 1.1rem;
        }
        
        .input-section, .output-section {
            margin-bottom: 30px;
        }
        
        label {
            display: block;
            margin-bottom: 10px;
            color: #64ffda;
            font-weight: 600;
            font-size: 1.2rem;
        }
        
        textarea {
            width: 100%;
            min-height: 120px;
            padding: 15px;
            border-radius: 8px;
            border: 2px solid #233554;
            background-color: #0a192f;
            color: #e6f1ff;
            font-size: 1rem;
            resize: vertical;
            transition: border-color 0.3s;
        }
        
        textarea:focus {
            outline: none;
            border-color: #64ffda;
        }
        
        .button-container {
            display: flex;
            justify-content: center;
            margin: 25px 0;
        }
        
        .cipher-btn {
            background-color: #0a192f;
            color: #64ffda;
            border: 2px solid #64ffda;
            padding: 15px 40px;
            font-size: 1.2rem;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-weight: 600;
            letter-spacing: 1px;
        }
        
        .cipher-btn:hover {
            background-color: #64ffda;
            color: #0a192f;
            transform: translateY(-3px);
            box-shadow: 0 5px 15px rgba(100, 255, 218, 0.4);
        }
        
        .cipher-btn:active {
            transform: translateY(0);
        }
        
        .output-container {
            background-color: #0a192f;
            border-radius: 8px;
            padding: 20px;
            border: 2px solid #233554;
            min-height: 120px;
            word-wrap: break-word;
        }
        
        .output-label {
            color: #64ffda;
            font-weight: 600;
            margin-bottom: 10px;
            font-size: 1.2rem;
        }
        
        .output-text {
            color: #e6f1ff;
            font-size: 1.1rem;
            line-height: 1.5;
        }
        
        .steps {
            margin-top: 40px;
            background-color: #112240;
            border-radius: 10px;
            padding: 20px;
            border-left: 4px solid #64ffda;
        }
        
        .steps h3 {
            color: #64ffda;
            margin-bottom: 15px;
        }
        
        .steps ol {
            padding-left: 20px;
            color: #8892b0;
        }
        
        .steps li {
            margin-bottom: 10px;
            line-height: 1.5;
        }
        
        .keyboard-info {
            margin-top: 20px;
            color: #8892b0;
            font-size: 0.9rem;
            text-align: center;
        }
        
        .char-count {
            text-align: right;
            margin-top: 5px;
            color: #8892b0;
            font-size: 0.9rem;
        }
        
        @media (max-width: 600px) {
            .container {
                padding: 20px;
            }
            
            h1 {
                font-size: 2rem;
            }
            
            textarea {
                min-height: 100px;
            }
            
            .cipher-btn {
                padding: 12px 30px;
                font-size: 1.1rem;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🔐 Cifrador QWERTY Shift</h1>
        <p class="subtitle">Convierte texto a símbolos (tecla + Shift) y luego a números ASCII</p>
        
        <div class="input-section">
            <label for="inputText">Texto a cifrar:</label>
            <textarea id="inputText" placeholder="Escribe tu texto aquí... Ejemplo: hola mundo"></textarea>
            <div class="char-count" id="charCount">0 caracteres</div>
        </div>
        
        <div class="button-container">
            <button class="cipher-btn" id="cipherBtn">Cifrar Texto</button>
        </div>
        
        <div class="output-section">
            <div class="output-label">Resultado del cifrado:</div>
            <div class="output-container">
                <div class="output-text" id="outputText">El resultado aparecerá aquí...</div>
            </div>
        </div>
        
        <div class="steps">
            <h3>¿Cómo funciona este cifrado?</h3>
            <ol>
                <li>El texto se convierte a minúsculas (excepto si usas símbolos directamente).</li>
                <li>Cada carácter se reemplaza por el símbolo que aparece en la misma tecla al presionar Shift (teclado QWERTY Latinoamericano).</li>
                <li>Los símbolos resultantes se convierten a sus valores ASCII (números decimales).</li>
                <li>Ejemplo: "k" → "(" → "40" (valor ASCII de '(' es 40).</li>
            </ol>
        </div>
        
        <div class="keyboard-info">
            <p>Usa el teclado QWERTY Latinoamericano para mejores resultados. Letras como k, l, ñ se convierten en (, ), ? respectivamente.</p>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const inputText = document.getElementById('inputText');
            const cipherBtn = document.getElementById('cipherBtn');
            const outputText = document.getElementById('outputText');
            const charCount = document.getElementById('charCount');
            
            // Mapeo para teclado QWERTY Latinoamericano (minúsculas a símbolos con Shift)
            const shiftMap = {
                // Letras (minúsculas a símbolos con Shift)
                'a': 'A', 'b': 'B', 'c': 'C', 'd': 'D', 'e': 'E', 'f': 'F',
                'g': 'G', 'h': 'H', 'i': 'I', 'j': 'J', 'k': '(', 'l': ')',
                'm': 'M', 'n': 'N', 'o': 'O', 'p': 'P', 'q': 'Q', 'r': 'R',
                's': 'S', 't': 'T', 'u': 'U', 'v': 'V', 'w': 'W', 'x': 'X',
                'y': 'Y', 'z': 'Z', 'ñ': '?',
                
                // Números a símbolos con Shift
                '0': '=', '1': '!', '2': '"', '3': '#', '4': '$', '5': '%',
                '6': '&', '7': '/', '8': '(', '9': ')',
                
                // Símbolos especiales
                '`': '|', "'": '?', '´': '¨', '+': '*', ',': ';', '.': ':',
                '-': '_', '[': '{', ']': '}',
                
                // Espacio y otros se mantienen igual
                ' ': ' ', '\n': '\n', '\t': '\t'
            };
            
            // Actualizar contador de caracteres
            inputText.addEventListener('input', function() {
                charCount.textContent = `${inputText.value.length} caracteres`;
            });
            
            // Función principal de cifrado
            function cipherText() {
                const text = inputText.value;
                
                if (!text.trim()) {
                    outputText.innerHTML = '<span style="color:#ff6b6b;">Por favor, escribe algún texto para cifrar.</span>';
                    return;
                }
                
                // Paso 1: Convertir a símbolos con Shift
                let shiftedText = '';
                for (let char of text) {
                    const lowerChar = char.toLowerCase();
                    if (shiftMap.hasOwnProperty(lowerChar)) {
                        // Mantener mayúsculas si la entrada original era mayúscula
                        if (char === char.toUpperCase() && char !== char.toLowerCase()) {
                            // Es una letra mayúscula
                            shiftedText += shiftMap[lowerChar];
                        } else {
                            shiftedText += shiftMap[lowerChar];
                        }
                    } else {
                        // Si no está en el mapa, mantener el carácter original
                        shiftedText += char;
                    }
                }
                
                // Paso 2: Convertir a valores ASCII
                let asciiNumbers = '';
                for (let char of shiftedText) {
                    asciiNumbers += char.charCodeAt(0) + ' ';
                }
                
                // Mostrar resultados
                outputText.innerHTML = `
                    <div style="margin-bottom: 15px;">
                        <strong style="color:#64ffda;">Texto con Shift:</strong><br>
                        <span style="color:#e6f1ff; font-family: monospace; background: #1a2c4d; padding: 5px; border-radius: 4px; display: inline-block; margin-top: 5px;">${shiftedText}</span>
                    </div>
                    <div>
                        <strong style="color:#64ffda;">Valores ASCII:</strong><br>
                        <span style="color:#e6f1ff; font-family: monospace; background: #1a2c4d; padding: 5px; border-radius: 4px; display: inline-block; margin-top: 5px; word-break: break-all;">${asciiNumbers.trim()}</span>
                    </div>
                `;
            }
            
            // Asignar evento al botón
            cipherBtn.addEventListener('click', cipherText);
            
            // También permitir cifrado con Ctrl+Enter en el textarea
            inputText.addEventListener('keydown', function(e) {
                if (e.ctrlKey && e.key === 'Enter') {
                    cipherText();
                }
            });
            
            // Ejemplo inicial
            inputText.value = "hola mundo";
            charCount.textContent = `${inputText.value.length} caracteres`;
            cipherText();
        });
    </script>
</body>
</html>
