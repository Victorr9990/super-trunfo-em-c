#include <stdio.h>

#define TAM_TAB 10
#define TAM_HABIL 5

// Valores do tabuleiro
#define AGUA 0
#define NAVIO 3
#define AREA_HABIL 5

// Função para inicializar tabuleiro com água
void inicializaTabuleiro(int tab[TAM_TAB][TAM_TAB]) {
    for (int i = 0; i < TAM_TAB; i++) {
        for (int j = 0; j < TAM_TAB; j++) {
            tab[i][j] = AGUA;
        }
    }
}

// Função para colocar navios fixos para teste
void posicionaNavios(int tab[TAM_TAB][TAM_TAB]) {
    // Colocando alguns navios arbitrários para teste
    tab[2][3] = NAVIO;
    tab[3][3] = NAVIO;
    tab[4][3] = NAVIO;
    tab[5][5] = NAVIO;
    tab[6][6] = NAVIO;
}

// Cria matriz habilidade Cone (5x5), 1 nas posições que formam o cone
void criaHabilidadeCone(int cone[TAM_HABIL][TAM_HABIL]) {
    for (int i = 0; i < TAM_HABIL; i++) {
        for (int j = 0; j < TAM_HABIL; j++) {
            // Limites do cone apontando para baixo, topo no meio da linha 0
            // A "base" vai aumentando largura conforme linha desce
            // Exemplo para 5 linhas: largura linha i = 2*i +1, centralizada
            int centro = TAM_HABIL / 2;
            int largura = 2 * i + 1;
            if (j >= centro - i && j <= centro + i)
                cone[i][j] = 1;
            else
                cone[i][j] = 0;
        }
    }
}

// Cria matriz habilidade Cruz (5x5)
void criaHabilidadeCruz(int cruz[TAM_HABIL][TAM_HABIL]) {
    int centro = TAM_HABIL / 2;
    for (int i = 0; i < TAM_HABIL; i++) {
        for (int j = 0; j < TAM_HABIL; j++) {
            // Cruz: linha do centro toda 1, coluna do centro toda 1
            if (i == centro || j == centro)
                cruz[i][j] = 1;
            else
                cruz[i][j] = 0;
        }
    }
}

// Cria matriz habilidade Octaedro (losango 5x5)
void criaHabilidadeOctaedro(int octaedro[TAM_HABIL][TAM_HABIL]) {
    int centro = TAM_HABIL / 2;
    for (int i = 0; i < TAM_HABIL; i++) {
        for (int j = 0; j < TAM_HABIL; j++) {
            // Losango: pontos com distância de Manhattan <= 2 do centro
            int dist = abs(i - centro) + abs(j - centro);
            if (dist <= 2)
                octaedro[i][j] = 1;
            else
                octaedro[i][j] = 0;
        }
    }
}

// Sobrepõe a matriz de habilidade no tabuleiro, centrando em (origemLinha, origemCol)
void aplicarHabilidade(int tab[TAM_TAB][TAM_TAB], int habilidade[TAM_HABIL][TAM_HABIL], int origemLinha, int origemCol) {
    int meio = TAM_HABIL / 2;
    for (int i = 0; i < TAM_HABIL; i++) {
        for (int j = 0; j < TAM_HABIL; j++) {
            if (habilidade[i][j] == 1) {
                int linTab = origemLinha - meio + i;
                int colTab = origemCol - meio + j;

                // Verifica limites do tabuleiro
                if (linTab >= 0 && linTab < TAM_TAB && colTab >= 0 && colTab < TAM_TAB) {
                    // Marca área afetada (se não for navio)
                    if (tab[linTab][colTab] == AGUA || tab[linTab][colTab] == NAVIO) {
                        tab[linTab][colTab] = AREA_HABIL;
                    }
                }
            }
        }
    }
}

// Exibe o tabuleiro no console
void imprimeTabuleiro(int tab[TAM_TAB][TAM_TAB]) {
    printf("Tabuleiro:\n");
    for (int i = 0; i < TAM_TAB; i++) {
        for (int j = 0; j < TAM_TAB; j++) {
            if (tab[i][j] == AGUA)
                printf("~ ");     // Água
            else if (tab[i][j] == NAVIO)
                printf("N ");     // Navio
            else if (tab[i][j] == AREA_HABIL)
                printf("* ");     // Área de efeito da habilidade
            else
                printf("? ");     // Qualquer outro valor inesperado
        }
        printf("\n");
    }
}

int main() {
    int tabuleiro[TAM_TAB][TAM_TAB];
    int cone[TAM_HABIL][TAM_HABIL];
    int cruz[TAM_HABIL][TAM_HABIL];
    int octaedro[TAM_HABIL][TAM_HABIL];

    // Inicializa tabuleiro
    inicializaTabuleiro(tabuleiro);

    // Posiciona navios para teste
    posicionaNavios(tabuleiro);

    // Cria as matrizes de habilidades usando condicionais
    criaHabilidadeCone(cone);
    criaHabilidadeCruz(cruz);
    criaHabilidadeOctaedro(octaedro);

    // Define pontos de origem para as habilidades (linha, coluna)
    int origemConeLinha = 2, origemConeCol = 3;
    int origemCruzLinha = 5, origemCruzCol = 5;
    int origemOctaedroLinha = 7, origemOctaedroCol = 6;

    // Aplica habilidades no tabuleiro
    aplicarHabilidade(tabuleiro, cone, origemConeLinha, origemConeCol);
    aplicarHabilidade(tabuleiro, cruz, origemCruzLinha, origemCruzCol);
    aplicarHabilidade(tabuleiro, octaedro, origemOctaedroLinha, origemOctaedroCol);

    // Imprime o tabuleiro final
    imprimeTabuleiro(tabuleiro);

    return 0;
}
