#include <stdio.h>
#include <string.h>

#define QTD_ATRIBUTOS 4

// Estrutura de uma carta
typedef struct {
    char nome[30];
    int populacao;
    int area;
    int pib;
    int densidade; // menor é melhor
} Carta;

// Função para exibir os atributos disponíveis
void mostrarAtributos(int ignorar) {
    printf("\nEscolha o atributo:\n");
    if (ignorar != 1) printf("1 - População\n");
    if (ignorar != 2) printf("2 - Área\n");
    if (ignorar != 3) printf("3 - PIB\n");
    if (ignorar != 4) printf("4 - Densidade Demográfica\n");
}

int main() {
    // Cartas pré-cadastradas
    Carta carta1 = {"Brasil", 211000000, 8516000, 22000, 25};
    Carta carta2 = {"Canadá", 37590000, 9985000, 18000, 4};

    int atributo1, atributo2;
    int valor1_c1, valor2_c1, valor1_c2, valor2_c2;
    int soma_c1, soma_c2;

    printf("Comparando: %s vs %s\n", carta1.nome, carta2.nome);

    // Escolha do primeiro atributo
    do {
        mostrarAtributos(0);
        printf("Escolha o primeiro atributo (1 a 4): ");
        scanf("%d", &atributo1);
        if (atributo1 < 1 || atributo1 > 4) printf("Opção inválida!\n");
    } while (atributo1 < 1 || atributo1 > 4);

    // Escolha do segundo atributo
    do {
        mostrarAtributos(atributo1);
        printf("Escolha o segundo atributo (diferente do primeiro): ");
        scanf("%d", &atributo2);
        if (atributo2 < 1 || atributo2 > 4 || atributo2 == atributo1) {
            printf("Opção inválida ou repetida!\n");
        }
    } while (atributo2 < 1 || atributo2 > 4 || atributo2 == atributo1);

    // Função para obter valor do atributo
    int getValor(Carta c, int atributo) {
        switch (atributo) {
            case 1: return c.populacao;
            case 2: return c.area;
            case 3: return c.pib;
            case 4: return c.densidade;
            default: return 0;
        }
    }

    // Pegar valores dos atributos para ambas as cartas
    valor1_c1 = getValor(carta1, atributo1);
    valor1_c2 = getValor(carta2, atributo1);
    valor2_c1 = getValor(carta1, atributo2);
    valor2_c2 = getValor(carta2, atributo2);

    // Mostrar resultados dos atributos
    char* nomes[5] = {"", "População", "Área", "PIB", "Densidade"};
    printf("\n--- COMPARAÇÃO ---\n");
    printf("Atributo 1: %s\n", nomes[atributo1]);
    printf("%s: %d\n", carta1.nome, valor1_c1);
    printf("%s: %d\n", carta2.nome, valor1_c2);

    printf("Atributo 2: %s\n", nomes[atributo2]);
    printf("%s: %d\n", carta1.nome, valor2_c1);
    printf("%s: %d\n", carta2.nome, valor2_c2);

    // Cálculo das somas
    soma_c1 = valor1_c1 + valor2_c1;
    soma_c2 = valor1_c2 + valor2_c2;

    // Ajuste para densidade (quanto menor, melhor)
    if (atributo1 == 4)
        soma_c1 += (valor1_c2 - valor1_c1) * 2; // penaliza valor maior
    if (atributo2 == 4)
        soma_c1 += (valor2_c2 - valor2_c1) * 2;

    if (atributo1 == 4)
        soma_c2 += (valor1_c1 - valor1_c2) * 2;
    if (atributo2 == 4)
        soma_c2 += (valor2_c1 - valor2_c2) * 2;

    // Exibir soma
    printf("\nSoma dos atributos:\n");
    printf("%s: %d\n", carta1.nome, soma_c1);
    printf("%s: %d\n", carta2.nome, soma_c2);

    // Resultado final
    printf("\nResultado: ");
    if (soma_c1 > soma_c2)
        printf("%s venceu!\n", carta1.nome);
    else if (soma_c2 > soma_c1)
        printf("%s venceu!\n", carta2.nome);
    else
        printf("Empate!\n");

    return 0;
}
