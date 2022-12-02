<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN">
<HTML>
 <HEAD>
  <TITLE> Jogo da Velha </TITLE>
  <META NAME="Generator" CONTENT="EditPlus">
  <META NAME="Author" CONTENT="">
  <META NAME="Keywords" CONTENT="">
  <META NAME="Description" CONTENT="">

  <style>
    
    table {width:650px; height:430px; border:1px solid black }
	table tr { background-color: blueviolet ; font:bold 46px Verdana; color:rgb(17, 255, 0)}
	table tr td {text-align:center; width:33.33%}
	table tr td:hover {cursor:pointer}
	input[type=button] {width:150px; height:50px; font-size:20px; font-weight:bold; margin-bottom:20px}
	fieldset {font-size:20; font-weight:bold; border:2px solid blue; width: 300px; margin-bottom:25px}

  </style>

  <script>
//maioria dos codigos pego de Robson Kraemer maluco incrivel
		//DEIXA A DIV GAME ESCONDIDA
		window.onload =	function() { document.getElementById('game').style.visibility = 'hidden' };
		
		function Jogador(nome, forma) {
			this.nome = nome;
			this.forma = forma;
		}
		
		var jogador1, jogador2;
		//Jogador da rodada
		var jogadorAtual;
		var formas = ['B', 'A'];
		var index = null;

		/*
			0 1 2
			3 4 5
			6 7 8
		*/
		var tabuleiro = new Array(9);


		initGame = function() {
			var nomeJogador1 = document.getElementById('jogador1').value;
			var nomeJogador2 = document.getElementById('jogador2').value;
			jogador1 = new Jogador(nomeJogador1, 0); //X
			jogador2 = new Jogador(nomeJogador2, 1); //O

			jogadorAtual = jogador1;
			setLabelJogadorAtual();

			//APOS DEFINIÇÃO DE JOGADORES, EXIBE A DIV E INICIA JOGO
			document.getElementById('game').style.visibility = 'visible';
			
		}

		/*Reinicia a partida*/
		reset = function() { window.location.reload(); }
		
		/*Seta o nome do jogador da rodada na página HTML*/
		setLabelJogadorAtual = function() {
			document.getElementById('jogadorAtual').innerHTML = 'Jogador atual:  ' + jogadorAtual.nome;
		}

		/*Verifica se o tabuleiro está completamente preenchido, se estiver, significa que ninguém venceu a rodada*/
		tabuleiroIsFilled = function() {
			var preenchidos = 0;
				for(var i = 0; i < tabuleiro.length; i++)
					if(tabuleiro[i]	!= undefined) 
						preenchidos++;
				return preenchidos == tabuleiro.length;
		}

		/*Verifica a existência de ocorrências de um mesmo elemento(X ou O) nas linhas do tabuleiro, procurando um vencedor*/
		allElementsInSomeLine = function() {
			for( var i = 0; i < 7; i += 3) {
				if ( tabuleiro[i] == 'B' && tabuleiro[i + 1] == 'B' && tabuleiro[i + 2] == 'B' ) { 
					alert (jogador1.nome + ' Ganhou!!!');
					reset();
				}
				if ( tabuleiro[i] == 'A' && tabuleiro[i + 1] == 'A' && tabuleiro[i + 2] == 'A' ) {
					alert (jogador2.nome + ' Ganhou!!!');
					reset();
				}
			}
		}

		/*Verifica a existência de ocorrências de um mesmo elemento(X ou O) nas colunas do tabuleiro, procurando um vencedor*/
		allElementsInSomeColumn = function() {
			for( var i = 0; i < 3; i++) {
				if ( tabuleiro[i] == 'B' && tabuleiro[i + 3] == 'B' && tabuleiro[i + 6] == 'B' ) { 
					alert (jogador1.nome + ' Ganhou!!!');
					reset();
				}
				if ( tabuleiro[i] == 'A' && tabuleiro[i + 3] == 'A' && tabuleiro[i + 6] == 'A' ) {
					alert (jogador2.nome + ' Ganhou!!!');
					reset();
				}
			}

		}

		/*Verifica a existência de ocorrências de um mesmo elemento(X ou O) nas diagonais do tabuleiro, procurando um vencedor*/
		allElementsInSomeDiagonal = function() {
			if ( (tabuleiro[0] == 'B' && tabuleiro[4] == 'B' && tabuleiro[8] == 'B') ||
	 			 (tabuleiro[2] == 'B' && tabuleiro[4] == 'B' && tabuleiro[6] == 'B')) {
					alert (jogador1.nome + ' Ganhou!!!');
				reset();
			} else if ( (tabuleiro[0] == 'A' && tabuleiro[4] == 'A' && tabuleiro[8] == 'A') ||
					    (tabuleiro[2] == 'A' && tabuleiro[4] == 'A' && tabuleiro[6] == 'A') ) {
					alert (jogador2.nome + ' Ganhou!!!');
				reset();
			} 
		}

		/*Preenche a célula da tabela HTML escolhida pelo usuário ao clicar, além de cuidar do jogador atual da rodada e chamar as funções
		  de verificação de algum ganhador */
		setOnCeil = function(cel, pos) { 
				if(tabuleiro[pos] == undefined) {
					cel.innerHTML = formas[jogadorAtual.forma];
					tabuleiro[pos] = formas[jogadorAtual.forma];

					//define o jogador da rodada
					(jogadorAtual.forma == 0) ? jogadorAtual = jogador2 : jogadorAtual = jogador1;
					setLabelJogadorAtual();

				} else alert('opa algo deu errado=/');

				allElementsInSomeLine();
				allElementsInSomeColumn();
				allElementsInSomeDiagonal();
	
				if ( tabuleiroIsFilled() ) {
					alert (' deu velha ');
					reset();
				}
				
			
		}

  </script>
 </HEAD>

 <BODY>
	<fieldset>
		<legend> Jogadores </legend>
		<label for="Jogador 1"> Jogador 1: </label>
		<input type="text" id="jogador1"/>
		<label for="Jogador 1"> Jogador 2: </label>
		<input type="text" id="jogador2"/>

		<input type="button" value="Iniciar" style="margin-top:4px; height:25px; width:150px; font-size:14px" onclick="javascript: initGame();"/>
	</fieldset>

	<h3 id="jogadorAtual"> </h3>

<div id="game">
	<table cellpadding="0" cellspacing="0">
		<tr border="1"> 
			<td onclick="javascript: setOnCeil(this, 0);" onmouseover="javascript: this.style.backgroundColor = 'blue'" onmouseout="javascript: this.style.backgroundColor = 'red'"> &nbsp; </td>
			<td onclick="javascript: setOnCeil(this, 1);" onmouseover="javascript: this.style.backgroundColor = 'blue'" onmouseout="javascript: this.style.backgroundColor = 'red'"> &nbsp; </td>
			<td onclick="javascript: setOnCeil(this, 2);" onmouseover="javascript: this.style.backgroundColor = 'blue'" onmouseout="javascript: this.style.backgroundColor = 'red'"> &nbsp; </td>
		</tr>
		<tr> 
			<td onclick="javascript: setOnCeil(this, 3);" onmouseover="javascript: this.style.backgroundColor = 'blue'" onmouseout="javascript: this.style.backgroundColor = 'red'"> &nbsp; </td>
			<td onclick="javascript: setOnCeil(this, 4);" onmouseover="javascript: this.style.backgroundColor = 'blue'" onmouseout="javascript: this.style.backgroundColor = 'red'"> &nbsp; </td>
			<td onclick="javascript: setOnCeil(this, 5);" onmouseover="javascript: this.style.backgroundColor = 'blue'" onmouseout="javascript: this.style.backgroundColor = 'red'"> &nbsp; </td>
		</tr>
		<tr> 
			<td onclick="javascript: setOnCeil(this, 6);" onmouseover="javascript: this.style.backgroundColor = 'blue'" onmouseout="javascript: this.style.backgroundColor = 'red'"> &nbsp; </td>
			<td onclick="javascript: setOnCeil(this, 7);" onmouseover="javascript: this.style.backgroundColor = 'blue'" onmouseout="javascript: this.style.backgroundColor = 'red'"> &nbsp; </td>
			<td onclick="javascript: setOnCeil(this, 8);" onmouseover="javascript: this.style.backgroundColor = 'blue'" onmouseout="javascript: this.style.backgroundColor = 'red'"> &nbsp; </td>
		</tr>
	</table>
</div>
  
 </BODY>
</HTML>
