# Vulnerabilidade de Injeção SQL
O código acima contém uma função chamada login_vulnerable, que é usada para autenticar usuários com base em seu nome de usuário e senha. No entanto, ele é propositalmente vulnerável a ataques de Injeção SQL devido à maneira como a consulta SQL é construída.

Explicação:
Construção da Consulta SQL: A função login_vulnerable usa a interpolação de strings para inserir os dados de entrada do usuário (nome de usuário e senha) diretamente na consulta SQL:

query = f"SELECT * FROM users WHERE username = '{username}' AND password = '{password}'"

Esse método é perigoso porque não sanitiza nem valida as entradas fornecidas pelo usuário. Um invasor pode explorar essa vulnerabilidade para injetar código SQL que altera a lógica da consulta.

Possível Exploração: Um usuário mal-intencionado pode inserir dados maliciosos para manipular a consulta SQL. Por exemplo, ao inserir o seguinte como nome de usuário:

' OR '1'='1

A consulta SQL gerada seria:
SELECT * FROM users WHERE username = '' OR '1'='1' AND password = '';

A condição '1'='1' é sempre verdadeira, permitindo que o invasor contorne a autenticação e recupere todos os registros de usuários do banco de dados.

Implicações de Segurança: Ao explorar essa vulnerabilidade, um invasor pode:

Obter acesso não autorizado ao sistema.
Recuperar dados sensíveis do banco de dados (como credenciais de usuários).
Manipular ou destruir dados dentro do banco de dados.
Executar operações administrativas, dependendo das permissões do banco de dados.
Como Prevenir Injeção SQL: Para evitar vulnerabilidades de Injeção SQL, as entradas do usuário devem ser sempre validadas, e consultas parametrizadas devem ser usadas. No SQLite (e na maioria dos bancos de dados SQL), isso pode ser feito usando placeholders (?) e vinculando as entradas corretamente:

cursor.execute("SELECT * FROM users WHERE username = ? AND password = ?", (username, password))

Widgets e Interface: A interface de login usa ipywidgets para criar campos de entrada de texto e um botão para simular o processo de login. As credenciais do usuário são inseridas nos campos de texto e, ao clicar no botão de login, a consulta SQL vulnerável é executada no backend, o que pode levar a uma brecha de segurança.

Exemplo de Consulta Segura
def login_safe(cursor, username, password):
query = "SELECT * FROM users WHERE username = ? AND password = ?"
cursor.execute(query, (username, password))
return cursor.fetchall()

Usando consultas parametrizadas, o banco de dados trata as entradas do usuário como dados, e não como parte da consulta SQL em si, prevenindo assim os ataques de Injeção SQL.
