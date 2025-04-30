# Desafio-Login-e-Senha---EmComp
Página de Login e Senha, atentando aos critérios do desafio.
Login e Senha pré estabelecidos.
Código programado na plataforma Notepadd++ v8.8
O host utilizado foi o WAMP Server.
O BDD foi desenvolvido pela ferramenta do PHP MyAdmin do WAMP.

O código é constituído em 5 arquivos .php:

- index.php
- conexao.php
- painel.php
- protect.php
- logout.php

Base de Estudos:

- Técnico Integrado em Informática - IFRP - 2022-2024.
- Auto-Didata.
- Conhecimento Empírico.

Meu nome é Plinio Zanelli, sou aluno do 1º Período de CC e sou muito grato pela oportunidade!




//index.php
<?php
include("conexao.php");

if(isset($_POST['cpf']) || isset($_POST['senha'])){
	if(strlen($_POST['cpf']) == 0 ){
		echo "Preencha o campo do CPF.";
	}else if(strlen($_POST['senha']) == 0 ){
		echo "Preencha o campo da Senha.";
	}else{
		$cpf = $mysqli->real_escape_string($_POST['cpf']);
		$senha = $mysqli->real_escape_string($_POST['senha']);
		
		$sql_code = "SELECT * FROM usuarios WHERE cpf='$cpf' AND senha='$senha'";
		$sql_query = $mysqli->query($sql_code) or die("Falha na execução do código SQL: " . $mysqli->error);
		
		$quantidade = $sql_query->num_rows;
		
		if($quantidade == 1){
			$usuario =$sql_query->fetch_assoc();
			
			if(!isset($_SESSION)){
				session_start();
			}
			$_SESSION['id'] = $usuario['id'];
			$_SESSION['cpf'] = $usuario['cpf'];
			$_SESSION['senha'] = $usuario['senha'];
			
			header("Location: painel.php");

			
		} else {
			echo "Falha ao logar! CPF ou senha incorretos";
		}
	}
	
}
		
?>

<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>LOGIN</title>
</head>
<body>
<center>
<h1>LOGIN E SENHA</h1>
<h3>(Para o devido funcionamento, considere 1º Usuário CPF: 12345678901 e Senha: 12345678)</h3>
<h3>(Para o devido funcionamento, considere 2º Usuário CPF: 10987654321 e Senha: 87654321)</h3>

	<form action="" method="POST">
	<p>
		<label>CPF</label>
		<input type="text" name="cpf">
	</p>
	<p>
		<label>Senha</label>
		<input type="password" name="senha">
	</p>
	<p> 
	<button type="submit">Entrar</button>
	</p>
	</form>
</center>
</body>
</html>




//conexao.php

<?php

$hostname = "localhost";
$bancodedados = "login";
$usuario = "root";
$senha = "";

$mysqli = new mysqli($hostname, $usuario, $senha, $bancodedados );
if ($mysqli->connect_errno){
	echo "Falha ao conectar: (" . $mysqli->connect_errno . ")" . $mysqli->connect_error;
}
?>



//painel.php


<?php

include("protect.php");

?>





<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>PAINEL</title>
</head>
<body>

<center>
<h1>Bem vindo ao Painel! </h1>
<h2>Usuário: <?php echo $_SESSION['cpf'];?></h2>
<h2>Senha: <?php echo $_SESSION['senha'];?> </h2>
</center>

	<p><center>
	<a href="logout.php">Logout</a>
	</p></center>
</body>
</html>




//protect.php


<?php

if(!isset($_SESSION)){
	session_start();
	
}

if(!isset($_SESSION['id'])){
die("Você não pode acessar essa página pois não está logado.<p><a href=\"index.php\">Entrar</a></p>");
}	
?>



//logout.php

<?php

if(!isset($_SESSION)){
	session_start();
}

session_destroy();

header("Location: index.php");
	
?>
