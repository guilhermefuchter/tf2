# tf2

<!DOCTYPE html>
<html>
<head>
    <title>Consulta de CEP</title>
    <style>
        body {
            font-family: Arial, sans-serif;
        }
        form {
            margin-bottom: 20px;
        }
        input[type="text"] {
            padding: 5px;
            width: 200px;
        }
        button {
            padding: 5px 10px;
        }
        #result {
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <h1>Consulta de CEP</h1>
    <form>
        <label for="cep">CEP:</label>
        <input type="text" id="cep" maxlength="8" placeholder="Digite o CEP" required>
        <button type="submit">Consultar</button>
    </form>
    <div id="result"></div>

    <script>
        const form = document.querySelector('form');
        const inputCep = document.querySelector('#cep');
        const resultDiv = document.querySelector('#result');

        form.addEventListener('submit', function (e) {
            e.preventDefault();
            const cep = inputCep.value;
            consultarCEP(cep);
        });

        function consultarCEP(cep) {
            const url = `https://viacep.com.br/ws/${cep}/json/`;

            fetch(url)
                .then(response => response.json())
                .then(data => {
                    if (data.erro) {
                        mostrarErro('CEP não encontrado');
                    } else {
                        mostrarResultado(data);
                    }
                })
                .catch(error => {
                    mostrarErro('Erro na consulta');
                });
        }

        function mostrarResultado(data) {
            const resultHTML = `
                <p><strong>CEP:</strong> ${data.cep}</p>
                <p><strong>Logradouro:</strong> ${data.logradouro}</p>
                <p><strong>Bairro:</strong> ${data.bairro}</p>
                <p><strong>Cidade:</strong> ${data.localidade}</p>
                <p><strong>Estado:</strong> ${data.uf}</p>
            `;
            resultDiv.innerHTML = resultHTML;
        }

        function mostrarErro(message) {
            resultDiv.innerHTML = `<p class="error">${message}</p>`;
        }
    </script>
</body>
</html>
