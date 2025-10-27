# Calculadora-fun-es-

<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculadora com Arrays e Struct</title>
    <style>
        * { margin: 0; padding: 0; }
        .fundo {
            background-image: linear-gradient(45deg, black, pink);
            height: 100vh;
            color: white;
            font-family: Arial, Helvetica, sans-serif;
            text-align: center;
        }
        .calculadora {
            position: absolute;
            background-color: rgba(0, 0, 0, 0.8);
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            border-radius: 15px;
            padding: 15px;
        }
        .botao {
            width: 50px;
            height: 50px;
            font-size: 25px;
            cursor: pointer;
            margin: 3px;
            background-color: rgb(31, 31, 31);
            border: none;
            color: #fff;
        }
        .botao:hover { background-color: black; }
        #resultado {
            background-color: white;
            width: 207px;
            height: 30px;
            margin: 5px;
            font-size: 26px;
            color: black;
            text-align: right;
            padding: 5px;
        }
        #historico {
            background-color: rgba(255, 255, 255, 0.2);
            margin-top: 10px;
            padding: 10px;
            border-radius: 8px;
            font-size: 14px;
        }
    </style>
</head>
<body>
    <div class="fundo">
        <h1>Calculadora Científica</h1>
        <div class="calculadora">
            <h1>Calculadora</h1>
            <p id="resultado"></p>
            <table>
                <tr>
                    <td><button class="botao" onclick="clean()">C</button></td>
                    <td><button class="botao" onclick="back()"><</button></td>
                    <td><button class="botao" onclick="insert('/')">/</button></td>
                    <td><button class="botao" onclick="insert('*')">×</button></td>
                </tr>
                <tr>
                    <td><button class="botao" onclick="insert('7')">7</button></td>
                    <td><button class="botao" onclick="insert('8')">8</button></td>
                    <td><button class="botao" onclick="insert('9')">9</button></td>
                    <td><button class="botao" onclick="insert('-')">-</button></td>
                </tr>
                <tr>
                    <td><button class="botao" onclick="insert('4')">4</button></td>
                    <td><button class="botao" onclick="insert('5')">5</button></td>
                    <td><button class="botao" onclick="insert('6')">6</button></td>
                    <td><button class="botao" onclick="insert('+')">+</button></td>
                </tr>
                <tr>
                    <td><button class="botao" onclick="insert('1')">1</button></td>
                    <td><button class="botao" onclick="insert('2')">2</button></td>
                    <td><button class="botao" onclick="insert('3')">3</button></td>
                    <td rowspan="2"><button class="botao" style="height: 106px;" onclick="calcular()">=</button></td>
                </tr>
                <tr>
                    <td colspan="2"><button class="botao" style="width: 106px;" onclick="insert('0')">0</button></td>
                    <td><button class="botao" onclick="insert('%')">%</button></td>
                </tr>
                <tr>
                    <td><button class="botao" onclick="funcaoTrig('sin')">sen</button></td>
                    <td><button class="botao" onclick="funcaoTrig('cos')">cos</button></td>
                    <td><button class="botao" onclick="funcaoTrig('tan')">tan</button></td>
                </tr>
                <tr>
                    <td><button class="botao" onclick="potencia()">x²</button></td>
                    <td><button class="botao" onclick="raizQuadrada()">√</button></td>
                    <td><button class="botao" onclick="logaritmo()">log</button></td>
                </tr>
                <tr>
                    <td><button class="botao" onclick="inserirPi()">π</button></td>
                    <td><button class="botao" onclick="fatorial()">n!</button></td>
                    <td><button class="botao" onclick="trocarSinal()">±</button></td>
                </tr>
                <tr>
                    <td><button class="botao" onclick="alternarModo()">RAD/DEG</button></td>
                    <td><button class="botao" onclick="limparHistorico()">CLH</button></td>
                    <td><button class="botao" onclick="salvarHistorico()">💾</button></td>
                </tr>
            </table>

            <div id="historico">
                <h3>Histórico:</h3>
                <ul id="listaHistorico"></ul>
            </div>
        </div>
    </div>

    <script>
        // ---------- STRUCT (Objeto) ----------
        function Operacao(expressao, resultado) {
            this.expressao = expressao;
            this.resultado = resultado;
        }

        let historico = [];
        let modoRadiano = true;

        function insert(num) {
            let numero = document.getElementById('resultado').innerHTML;
            document.getElementById('resultado').innerHTML = numero + num;
        }

        function clean() {
            document.getElementById('resultado').innerHTML = "";
        }

        function back() {
            let resultado = document.getElementById('resultado').innerHTML;
            document.getElementById('resultado').innerHTML = resultado.substring(0, resultado.length - 1);
        }

        function calcular() {
            let expressao = document.getElementById('resultado').innerHTML;
            if (expressao) {
                try {
                    let resultado = eval(expressao);
                    document.getElementById('resultado').innerHTML = resultado;
                    let operacao = new Operacao(expressao, resultado);
                    historico.push(operacao);
                    atualizarHistorico();
                } catch {
                    document.getElementById('resultado').innerHTML = "Erro";
                }
            }
        }

        function funcaoTrig(tipo) {
            let resultado = parseFloat(document.getElementById('resultado').innerHTML);
            if (isNaN(resultado)) return;
            let valor = modoRadiano ? resultado : resultado * Math.PI / 180;
            let res = 0;
            if (tipo === 'sin') res = Math.sin(valor);
            if (tipo === 'cos') res = Math.cos(valor);
            if (tipo === 'tan') res = Math.tan(valor);
            document.getElementById('resultado').innerHTML = res;
            historico.push(new Operacao(`${tipo}(${resultado})`, res));
            atualizarHistorico();
        }

        function atualizarHistorico() {
            let lista = document.getElementById('listaHistorico');
            lista.innerHTML = "";
            historico.forEach(op => {
                let li = document.createElement('li');
                li.textContent = `${op.expressao} = ${op.resultado}`;
                lista.appendChild(li);
            });
        }

        // --------- 14 NOVAS FUNÇÕES ---------
        function potencia() {
            let valor = parseFloat(document.getElementById('resultado').innerHTML);
            if (!isNaN(valor)) {
                let res = Math.pow(valor, 2);
                document.getElementById('resultado').innerHTML = res;
                historico.push(new Operacao(`${valor}²`, res));
                atualizarHistorico();
            }
        }

        function raizQuadrada() {
            let valor = parseFloat(document.getElementById('resultado').innerHTML);
            if (!isNaN(valor)) {
                let res = Math.sqrt(valor);
                document.getElementById('resultado').innerHTML = res;
                historico.push(new Operacao(`√(${valor})`, res));
                atualizarHistorico();
            }
        }

        function logaritmo() {
            let valor = parseFloat(document.getElementById('resultado').innerHTML);
            if (!isNaN(valor)) {
                let res = Math.log10(valor);
                document.getElementById('resultado').innerHTML = res;
                historico.push(new Operacao(`log(${valor})`, res));
                atualizarHistorico();
            }
        }

        function fatorial() {
            let n = parseInt(document.getElementById('resultado').innerHTML);
            if (isNaN(n) || n < 0) return;
            let res = 1;
            for (let i = 1; i <= n; i++) res *= i;
            document.getElementById('resultado').innerHTML = res;
            historico.push(new Operacao(`${n}!`, res));
            atualizarHistorico();
        }

        function inserirPi() {
            document.getElementById('resultado').innerHTML = Math.PI.toFixed(8);
        }

        function trocarSinal() {
            let valor = parseFloat(document.getElementById('resultado').innerHTML);
            if (!isNaN(valor)) document.getElementById('resultado').innerHTML = -valor;
        }

        function alternarModo() {
            modoRadiano = !modoRadiano;
            alert(`Modo alterado para: ${modoRadiano ? "Radianos" : "Graus"}`);
        }

        function limparHistorico() {
            historico = [];
            atualizarHistorico();
        }

        function salvarHistorico() {
            const blob = new Blob([JSON.stringify(historico, null, 2)], { type: "application/json" });
            const link = document.createElement("a");
            link.href = URL.createObjectURL(blob);
            link.download = "historico_calculadora.json";
            link.click();
        }

        // Funções adicionais utilitárias
        function cubo() {
            let valor = parseFloat(document.getElementById('resultado').innerHTML);
            if (!isNaN(valor)) {
                let res = Math.pow(valor, 3);
                document.getElementById('resultado').innerHTML = res;
                historico.push(new Operacao(`${valor}³`, res));
                atualizarHistorico();
            }
        }

        function inverso() {
            let valor = parseFloat(document.getElementById('resultado').innerHTML);
            if (!isNaN(valor) && valor !== 0) {
                let res = 1 / valor;
                document.getElementById('resultado').innerHTML = res;
                historico.push(new Operacao(`1/${valor}`, res));
                atualizarHistorico();
            }
        }

        function elevarPersonalizado() {
            let base = parseFloat(prompt("Digite a base:"));
            let expo = parseFloat(prompt("Digite o expoente:"));
            if (!isNaN(base) && !isNaN(expo)) {
                let res = Math.pow(base, expo);
                document.getElementById('resultado').innerHTML = res;
                historico.push(new Operacao(`${base}^${expo}`, res));
                atualizarHistorico();
            }
        }

        function logNatural() {
            let valor = parseFloat(document.getElementById('resultado').innerHTML);
            if (!isNaN(valor)) {
                let res = Math.log(valor);
                document.getElementById('resultado').innerHTML = res;
                historico.push(new Operacao(`ln(${valor})`, res));
                atualizarHistorico();
            }
        }

        function porcentagem() {
            let valor = parseFloat(document.getElementById('resultado').innerHTML);
            if (!isNaN(valor)) {
                let res = valor / 100;
                document.getElementById('resultado').innerHTML = res;
                historico.push(new Operacao(`${valor}%`, res));
                atualizarHistorico();
            }
        }
    </script>
</body>
</html>