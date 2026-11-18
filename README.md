#include <stdlib.h>
#include <time.h>

// Função para limpar a tela no OnlineGDB
void limparTela() {
    #ifdef _WIN32
        system("cls");
    #else
        system("clear");
    #endif
}

void iniciarJogo() {
    int vidaJogador = 120;
    int vidaMaxima = 120;
    int pocoes = 4;
    int escolha, danoJogador, danoMonstro, cura;

    // Configuração dos 3 monstros usando vetores (arrays)
    char nomesMonstros[3][30] = {
        "Goblin Ladrao", 
        "Orc Guerreiro", 
        "Dragao Anciao (CHEFE)"
    };
    int vidasMonstros[3] = {60, 90, 140};
    int danoMinMonstro[3] = {5, 10, 15};
    int danoMaxMonstro[3] = {12, 18, 25};

    srand(time(NULL));
    limparTela();

    // Loop que passa por cada um dos 3 monstros
    for (int i = 0; i < 3; i++) {
        int vidaAtualMonstro = vidasMonstros[i];

        printf("========================================\n");
        printf(" FASE %d/3: UM %s APARECEU!\n", i + 1, nomesMonstros[i]);
        printf("========================================\n\n");

        // Loop da batalha atual
        while (vidaJogador > 0 && vidaAtualMonstro > 0) {
            printf("----------------------------------------\n");
            printf("SEU HP: %d/%d  |  POCOES: %d\n", vidaJogador, vidaMaxima, pocoes);
            printf("HP DO %s: %d\n", nomesMonstros[i], vidaAtualMonstro);
            printf("----------------------------------------\n");
            printf("Escolha sua acao:\n");
            printf("1. Atacar com espada\n");
            printf("2. Usar pocao de cura (+35 HP)\n");
            printf("3. Fugir da aventura\n");
            printf("Opcao: ");
            scanf("%d", &escolha);

            printf("\n");

            if (escolha == 1) {
                // Dano do jogador entre 18 e 28
                danoJogador = (rand() % 11) + 18;
                vidaAtualMonstro -= danoJogador;
                printf(">> Voce golpeou o %s e causou %d de dano!\n", nomesMonstros[i], danoJogador);
            } 
            else if (escolha == 2) {
                if (pocoes > 0) {
                    cura = 35;
                    vidaJogador += cura;
                    if (vidaJogador > vidaMaxima) {
                        vidaJogador = vidaMaxima;
                    }
                    pocoes--;
                    printf(">> Voce usou uma pocao e recuperou %d de vida!\n", cura);
                } else {
                    printf(">> Sem pocoes! Voce perdeu a vez procurando na bolsa.\n");
                }
            } 
            else if (escolha == 3) {
                printf(">> Voce fugiu da masmorra!\n\n");
                return; // Volta ao menu principal
            } 
            else {
                printf(">> Opcao invalida! Voce hesitou e perdeu o turno.\n");
            }

            // Turno do monstro (se ainda estiver vivo)
            if (vidaAtualMonstro > 0) {
                danoMonstro = (rand() % (danoMaxMonstro[i] - danoMinMonstro[i] + 1)) + danoMinMonstro[i];
                vidaJogador -= danoMonstro;
                printf(">> O %s atacou e causou %d de dano em voce!\n\n", nomesMonstros[i], danoMonstro);
            }
        }

        // Verifica se o jogador morreu nesta batalha
        if (vidaJogador <= 0) {
            printf("========================================\n");
            printf("GAME OVER! Voce foi derrotado pelo %s.\n", nomesMonstros[i]);
            printf("========================================\n\n");
            return; // Encerra o jogo e volta ao menu
        }

        // Se venceu e não é o último monstro, ganha uma recompensa
        printf("\n*** Voce derrotou o %s! ***\n", nomesMonstros[i]);
        if (i < 2) {
            printf(">> Voce descansou na fogueira (+20 HP) e encontrou 1 pocao!\n\n");
            vidaJogador += 20;
            if (vidaJogador > vidaMaxima) vidaJogador = vidaMaxima;
            pocoes++;
        }
    }

    // Se saiu do loop 'for' vivo, zerou o jogo!
    printf("========================================\n");
    printf(" PARABENS! VOCE ZEROU O JOGO!           \n");
    printf(" Todos os monstros foram derrotados!    \n");
    printf("========================================\n\n");
}

int main() {
    int opcaoMenu = 0;

    while (opcaoMenu != 3) {
        printf("========================================\n");
        printf("       CRONICAS DE C - A MASMORRA       \n");
        printf("========================================\n");
        printf("1. Iniciar Jornada (3 Monstros)\n");
        printf("2. Instrucoes\n");
        printf("3. Sair\n");
        printf("========================================\n");
        printf("Escolha uma opcao: ");
        scanf("%d", &opcaoMenu);

        switch (opcaoMenu) {
            case 1:
                iniciarJogo();
                break;
            case 2:
                limparTela();
                printf("=== INSTRUCOES ===\n");
                printf("- Enfrente 3 monstros em sequencia: Goblin, Orc e Dragao.\n");
                printf("- Voce comeca com 120 de HP e 4 pocoes.\n");
                printf("- Ao vencer uma batalha, voce recupera 20 de HP e ganha 1 pocao.\n\n");
                break;
            case 3:
                printf("\nSaindo do jogo... Ate a proxima!\n");
                break;
            default:
                limparTela();
                printf("\nOpcao invalida! Tente novamente.\n\n");
        }
    }

    return 0;
}
I
