#include <stdio.h>
#include <conio.h>
#include <stdlib.h>
#include <time.h>

#define VAZIO 0
#define BOLA 5
#define XIS 7

void ZeraTabuleiro(int *Tabuleiro)
{
	int i;
	for (i = 0; i<9; i++)
		Tabuleiro[i] = 0;
}

void MostraTabuleiro(int *Tabuleiro)
{
	/* Exibe o tabuleiro na tela.
	0 - espaço em branco
	5 - equivale a O
	7 - equivale a X
	*/
	int i, j;
	for (i = 0; i<3; i++)
	{
		printf("\n| ");
		for (j = 0; j<3; j++)
		{
			printf("%c ", Tabuleiro[3 * i + j] == VAZIO ? ' ' : (Tabuleiro[3 * i + j] == BOLA ? 'O' : 'X'));
		}
		printf("|");
	}
	printf("\n");
}

int VerificaVitoria(int Peca, int Linha, int Coluna, int *Tabuleiro)
{
	/* Retorna 0 ou 1, informando se a peca colocada no tabuleiro fez o jogador sair vitorioso */
	/* Se esta funcao retornar 1, a peca inserida gerou a vitoria */
	int Posicao = 3 * Linha + Coluna;
	switch (Posicao)
	{
	case 0:
		return (Tabuleiro[0] + Tabuleiro[1] + Tabuleiro[2] == 3 * Peca || Tabuleiro[0] + Tabuleiro[4] + Tabuleiro[8] == 3 * Peca || Tabuleiro[0] + Tabuleiro[3] + Tabuleiro[6] == 3 * Peca);
	case 1:
		return (Tabuleiro[1] + Tabuleiro[4] + Tabuleiro[7] == 3 * Peca || Tabuleiro[0] + Tabuleiro[1] + Tabuleiro[2] == 3 * Peca);
	case 2:
		return (Tabuleiro[0] + Tabuleiro[1] + Tabuleiro[2] == 3 * Peca || Tabuleiro[2] + Tabuleiro[4] + Tabuleiro[6] == 3 * Peca || Tabuleiro[2] + Tabuleiro[5] + Tabuleiro[8] == 3 * Peca);
	case 3:
		return (Tabuleiro[3] + Tabuleiro[4] + Tabuleiro[5] == 3 * Peca || Tabuleiro[0] + Tabuleiro[3] + Tabuleiro[6] == 3 * Peca);
	case 4:
		return (Tabuleiro[1] + Tabuleiro[4] + Tabuleiro[7] == 3 * Peca || Tabuleiro[3] + Tabuleiro[4] + Tabuleiro[5] == 3 * Peca || Tabuleiro[0] + Tabuleiro[4] + Tabuleiro[8] == 3 * Peca || Tabuleiro[2] + Tabuleiro[4] + Tabuleiro[6] == 3 * Peca);
	case 5:
		return (Tabuleiro[2] + Tabuleiro[5] + Tabuleiro[8] == 3 * Peca || Tabuleiro[3] + Tabuleiro[4] + Tabuleiro[5] == 3 * Peca);
	case 6:
		return (Tabuleiro[0] + Tabuleiro[3] + Tabuleiro[6] == 3 * Peca || Tabuleiro[6] + Tabuleiro[7] + Tabuleiro[8] == 3 * Peca || Tabuleiro[2] + Tabuleiro[4] + Tabuleiro[6] == 3 * Peca);
	case 7:
		return (Tabuleiro[6] + Tabuleiro[7] + Tabuleiro[8] == 3 * Peca || Tabuleiro[1] + Tabuleiro[4] + Tabuleiro[7] == 3 * Peca);
	case 8:
		return (Tabuleiro[2] + Tabuleiro[5] + Tabuleiro[8] == 3 * Peca || Tabuleiro[6] + Tabuleiro[7] + Tabuleiro[8] == 3 * Peca || Tabuleiro[0] + Tabuleiro[4] + Tabuleiro[8] == 3 * Peca);
	}
	return 0;
}

int pc_joga()
{
	int x, y, mx, my, linha, coluna;
	int k, j, i;
	int erro = 0;
	int Tabuleiro[3][3], TabuleiroA[3][3], TabuleiroB[3][3];

	x = (rand() % 3);
	y = (rand() % 3);

	if (Tabuleiro[x][y] != VAZIO)
	{
		pc_joga();
		return 1;
	}
	else
	{
		/**************************************
		O Computador gerará 2 números aleatórios X e Y.
		E verificará se as coordenadas do ponto (X,Y) estão livre.
		Se estiverem livres (matriz[x][y]==' ') então ele criará um
		"backup" da matriz original, e vai começas a jogar em cima delas.
		*/
		for (i = 0; i<3; i++)
			for (j = 0; j<3; j++)
				TabuleiroA[i][j] = Tabuleiro[i][j];

		for (i = 0; i<3; i++)
			for (j = 0; j<3; j++)
			{
				if (Tabuleiro[i][j] == VAZIO)
				{
					/*Neste primeiro trecho de codigo, o computador
					já criou uma nova matriz. Aqui, ele marca O num ponto
					qualquer e depois verifica se com este ponto ele consegue ganhar o jogo*/
					Tabuleiro[i][j] = BOLA;
					if (VerificaVitoria(BOLA, linha, coluna, &Tabuleiro[0][0]) != 0)
					{
						mx = i;
						my = j;
						erro = 2;
					}
					else
					{/*Caso o computador não possa marcar zero neste ponto, ele
					 fará o teste para ver se o jogador pode ganhar o jogo. Caso ele consiga vencer o jogo, o computador
					 marca esta posição para evitar a vitória adversária.

					 Logicamente, a varável erro é verificada como diferente de 2. Pois caso o computador ache
					 um ponto onde ele possa ganhar, não vale a pena ele procurar outro para evitar a vitória adversária.
					 */
						Tabuleiro[i][j] = XIS;
						if ((VerificaVitoria(XIS, linha, coluna, &Tabuleiro[0][0]) != 0) && (erro != 2))
						{
							mx = i;
							my = j;
							erro = 1;
						}

					}

					Tabuleiro[i][j] = VAZIO;
				}
			}

		//Restaura Matriz 
		for (i = 0; i<3; i++)
			for (j = 0; j<3; j++)
				Tabuleiro[i][j] = TabuleiroA[i][j];

		if (erro == 0)
			Tabuleiro[x][y] = BOLA;
		if (erro != 0)
			Tabuleiro[mx][my] = BOLA;
	}
}

int main()
{
	int Tabuleiro[3][3], Jogador = 1;
	int Ganhou = 0, Jogadas = 0, x;
	int linha, coluna;

	ZeraTabuleiro(&Tabuleiro[0][0]);
	while (1)
	{
		MostraTabuleiro(&Tabuleiro[0][0]);
		if (Jogadas == 9)
		{
			MostraTabuleiro(&Tabuleiro[0][0]);
			printf("\n\n O jogo empatou\n\n");
			return 0;
		}
		else
		{
			if (Jogador == 1)
			{
				printf("Linha: ");
				scanf("%d", &linha);
				printf("Coluna: ");
				scanf("%d", &coluna);
				if ((Tabuleiro[linha][coluna] != VAZIO) || (linha > 2) || (coluna > 2) || (linha <= -1) || (coluna <= -1))
				{
					printf("\n\n Posicao invalida, tente novamente \n\n");
					continue;
				}
				Tabuleiro[linha][coluna] = BOLA;
				Ganhou = VerificaVitoria(BOLA, linha, coluna, &Tabuleiro[0][0]);
				if (Ganhou == 1)
				{
					printf("\n\n Bola ganhou PARABENS\n\n");
					MostraTabuleiro(&Tabuleiro[0][0]);
					getche();
					return 0;
				}
				Jogadas = Jogadas++;
				Jogador = 2;
			}
			else
			{
				/*insira aqui a jogada do pc*/
				pc_joga();
				Ganhou = VerificaVitoria(XIS, linha, coluna, &Tabuleiro[0][0]);
				if (Ganhou == 1)
				{
					printf("\n\nVoce perdeu!\n\n");
					MostraTabuleiro(&Tabuleiro[0][0]);
					getche();
					return 0;
				}
				Jogadas = Jogadas++;
				Jogador = 1;
			}
		}
	}
	return 0;
}
	