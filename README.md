<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    
</head>
<body>
    <h1 align="center">Vulnerabilidade de Injeção SQL</h1>

   <section>
        <p>Bem-vindo à documentação sobre a vulnerabilidade de Injeção SQL. Este documento explora uma função de login vulnerável e fornece orientações sobre como proteger aplicações contra esse tipo de ataque.</p>
    </section>

   <h2 align="center">Demonstração da Vulnerabilidade</h2>
    <p align="center">
        <img src="https://example.com/path-to-image.png" alt="Demonstração da Vulnerabilidade" />
    </p>

  <section>
        <h2>Descrição do Problema</h2>
        <p>O código abaixo apresenta uma função chamada <code>login_vulnerable</code>, usada para autenticar usuários com base em nome de usuário e senha. Esta função é propositalmente vulnerável a ataques de Injeção SQL devido à construção insegura da consulta SQL.</p>
    </section>

  <section>
        <h2>Explicação da Vulnerabilidade</h2>

   <h3>Construção da Consulta SQL</h3>
        <p>A função <code>login_vulnerable</code> utiliza interpolação de strings para construir a consulta SQL:</p>
        <pre><code>query = f"SELECT * FROM users WHERE username = '{username}' AND password = '{password}'"</code></pre>
        <p>Este método não realiza a sanitização ou validação das entradas fornecidas pelo usuário, tornando o código vulnerável a ataques de Injeção SQL.</p>

  <h3>Possível Exploração</h3>
        <p>Um usuário mal-intencionado pode inserir dados manipulativos para alterar a lógica da consulta SQL. Por exemplo, ao inserir o seguinte como nome de usuário:</p>
        <pre><code>' OR '1'='1</code></pre>
        <p>A consulta SQL gerada será:</p>
        <pre><code>SELECT * FROM users WHERE username = '' OR '1'='1' AND password = ''</code></pre>
        <p>A condição <code>'1'='1'</code> sempre é verdadeira, permitindo ao invasor contornar a autenticação e acessar todos os registros do banco de dados.</p>

   <h3>Implicações de Segurança</h3>
        <p>Explorar essa vulnerabilidade pode permitir a um invasor:</p>
        <ul>
            <li><b>Acesso Não Autorizado:</b> Contornar o sistema de autenticação.</li>
            <li><b>Recuperação de Dados Sensíveis:</b> Obter informações confidenciais do banco de dados.</li>
            <li><b>Manipulação ou Destruição de Dados:</b> Alterar ou excluir informações no banco de dados.</li>
         <li><b>Execução de Operações Administrativas:</b> Realizar operações com permissões administrativas, dependendo do banco de dados.</li>
  </ul>
  </section>

  <section>
        <h2>Prevenção de Injeção SQL</h2>
        <p>Para evitar vulnerabilidades de Injeção SQL, as entradas do usuário devem ser validadas e consultas parametrizadas devem ser usadas. No SQLite (e na maioria dos bancos de dados SQL), isso pode ser feito usando placeholders (<code>?</code>) e vinculando as entradas corretamente:</p>
        <pre><code>cursor.execute("SELECT * FROM users WHERE username = ? AND password = ?", (username, password))</code></pre>

  <h3>Exemplo de Consulta Segura</h3>
        <p>Aqui está um exemplo de uma função segura para autenticação:</p>
        <pre><code>def login_safe(cursor, username, password):
    query = "SELECT * FROM users WHERE username = ? AND password = ?"
    cursor.execute(query, (username, password))
    return cursor.fetchall()</code></pre>
        <p>Consultas parametrizadas garantem que as entradas do usuário sejam tratadas como dados e não como parte da consulta SQL, prevenindo ataques de Injeção SQL.</p>
  </section>

  <section>
        <h2>Interface de Usuário</h2>
        <p>A interface de login usa campos de entrada de texto e um botão para simular o processo de login. Ao clicar no botão, a consulta SQL vulnerável é executada no backend, o que pode levar a uma brecha de segurança.</p>
  </section>

  <section>
        <h2>Como Executar o Projeto</h2>
        <ol>
            <li>Clone o repositório: <pre><code>git clone https://github.com/seu-usuario/vulnerabilidade-sql.git</code></pre></li>
            <li>Abra o projeto no seu editor de código.</li>
            <li>Revise o código para entender as vulnerabilidades e as práticas de prevenção.</li>
        </ol>
  </section>

   <section>
        <h2>Contribuições</h2>
        <p>Quer contribuir para o projeto? Sinta-se à vontade para fazer um fork, criar uma nova branch, e submeter um pull request. Suas contribuições para melhorar a segurança são bem-vindas!</p>
    </section>

  <footer>
        <h1 align="center">Contato</h1>
        <p align="left">Para mais informações, entre em contato:</p>
        <ul>
            <li>Douglas Souza Silva - <a href="mailto:ddouglss1999@gmail.com">ddouglss1999@gmail.com</a></li>
            <li>Guilherme Guimarães - <a href="mailto:guiguimaraes906@gmail.com">guiguimaraes906@gmail.com</a></li>
          
        </ul>
        <p>Muito obrigado!</p>
    </footer>
</body>
</html>
