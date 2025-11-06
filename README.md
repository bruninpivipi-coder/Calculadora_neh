// Calculadora goated abaixo neh

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>

static int read_int_in_range(const char *prompt, int min, int max) {
    char buf[128];
    long val;
    char *end;
    while (1) {
        printf("%s", prompt);
        if (!fgets(buf, sizeof(buf), stdin)) exit(0);
        // remove trailing newline
        buf[strcspn(buf, "\r\n")] = '\0';
        // skip leading spaces
        char *p = buf;
        while (isspace((unsigned char)*p)) p++;
        if (*p == '\0') { printf("Entrada invalida. Tente novamente.\n"); continue; }
        val = strtol(p, &end, 10);
        while (isspace((unsigned char)*end)) end++;
        if (*end != '\0') { printf("Entrada invalida. Tente novamente.\n"); continue; }
        if (val < min || val > max) { printf("Opcao invalida. Escolha um numero entre %d e %d.\n", min, max); continue; }
        return (int)val;
    }
}

static double read_double(const char *prompt) {
    char buf[128];
    char *end;
    double v;
    while (1) {
        printf("%s", prompt);
        if (!fgets(buf, sizeof(buf), stdin)) exit(0);
        buf[strcspn(buf, "\r\n")] = '\0';
        char *p = buf;
        while (isspace((unsigned char)*p)) p++;
        if (*p == '\0') { printf("Entrada invalida. Tente novamente.\n"); continue; }
        v = strtod(p, &end);
        while (isspace((unsigned char)*end)) end++;
        if (*end != '\0') { printf("Entrada invalida. Tente novamente.\n"); continue; }
        return v;
    }
}

int main(void) {
    int opc;
    char resp[8];
    for (;;) {
        printf("===============================\n");
        printf("   Calculadora Simples\n");
        printf("===============================\n");
        printf("Selecione uma operacao:\n");
        printf("1. Adicao\n");
        printf("2. Subtracao\n");
        printf("3. Multiplicacao\n");
        printf("4. Divisao\n");
        printf("5. Sair\n");
        opc = read_int_in_range("Opcao: ", 1, 5);

        if (opc == 5) {
            printf("Obrigado por usar a calculadora! Ate a proxima.\n");
            break;
        }

        double a = read_double("Digite o primeiro numero: ");
        double b = read_double("Digite o segundo numero: ");
        double res;
        char opchar = '?';
        int error = 0;

        if (opc == 1) { res = a + b; opchar = '+'; }
        else if (opc == 2) { res = a - b; opchar = '-'; }
        else if (opc == 3) { res = a * b; opchar = '*'; }
        else if (opc == 4) {
            opchar = '/';
            if (b == 0.0) {
                printf("Erro: divisao por zero.\n");
                error = 1;
            } else res = a / b;
        }

        if (!error) {
            
            printf("Resultado: %g %c %g = %g\n", a, opchar, b, res);
        }

        // perguntar se deseja outra operação
        while (1) {
            printf("Deseja realizar outra operação? (s/n): ");
            if (!fgets(resp, sizeof(resp), stdin)) exit(0);
            // trim
            char *p = resp;
            while (isspace((unsigned char)*p)) p++;
            if (*p == '\0') { printf("Resposta inválida. Por favor, digite 's' para sim ou 'n' para não.\n"); continue; }
            char c = tolower((unsigned char)p[0]);
            if (c == 's') {
#ifdef _WIN32
                system("cls");
#endif
                break; // voltar ao menu
            } else if (c == 'n') {
                printf("Obrigado por usar a calculadora! Ate a proxima.\n");
                return 0;
            } else {
                printf("Resposta invalida. Por favor, digite 's' para sim ou 'n' para não.\n");
            }
        }
    }
    return 0;
}
// Termina aqui neh
