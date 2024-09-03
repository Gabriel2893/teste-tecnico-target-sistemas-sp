# teste-tecnico-target-sistemas-sp

1) Observe o trecho de código abaixo: int INDICE = 13, SOMA = 0, K = 0;
Enquanto K < INDICE faça { K = K + 1; SOMA = SOMA + K; }
Imprimir(SOMA);
Ao final do processamento, qual será o valor da variável SOMA?

```java
int INDICE = 13, SOMA = 0, K = 0;
while (K < INDICE) {
    K = K + 1;
    SOMA = SOMA + K;
}
System.out.println(SOMA);
```
2) Dado a sequência de Fibonacci, onde se inicia por 0 e 1 e o próximo valor sempre será a soma dos 2 valores anteriores (exemplo: 0, 1, 1, 2, 3, 5, 8, 13, 21, 34...), escreva um programa na linguagem que desejar onde, informado um número, ele calcule a sequência de Fibonacci e retorne uma mensagem avisando se o número informado pertence ou não a sequência.

IMPORTANTE: Esse número pode ser informado através de qualquer entrada de sua preferência ou pode ser previamente definido no código;

```java
package org.example;

import java.util.Scanner;

public class FibonacciChecker {

    public static void main(String[] args) {
        // Scanner para ler a entrada do usuário
        Scanner scanner = new Scanner(System.in);
        System.out.print("Informe um número para verificar se pertence à sequência de Fibonacci: ");
        int numero = scanner.nextInt();

        // Variáveis iniciais da sequência de Fibonacci
        int a = 0;
        int b = 1;

        // Caso especial para 0 e 1
        if (numero == a || numero == b) {
            System.out.println("O número " + numero + " pertence à sequência de Fibonacci.");
            return;
        }

        // Variável que armazenará o próximo número da sequência
        int c = a + b;

        // Gerar a sequência até que c seja maior ou igual ao número informado
        while (c <= numero) {
            if (c == numero) {
                System.out.println("O número " + numero + " pertence à sequência de Fibonacci.");
                return;
            }

            // Atualizar a sequência
            a = b;
            b = c;
            c = a + b;
        }

        // Se o loop terminar, significa que o número não foi encontrado na sequência
        System.out.println("O número " + numero + " não pertence à sequência de Fibonacci.");
    }
}
```

3) Dado um vetor que guarda o valor de faturamento diário de uma distribuidora, faça um programa, na linguagem que desejar, que calcule e retorne:
• O menor valor de faturamento ocorrido em um dia do mês;
• O maior valor de faturamento ocorrido em um dia do mês;
• Número de dias no mês em que o valor de faturamento diário foi superior à média mensal.

IMPORTANTE:
a) Usar o json ou xml disponível como fonte dos dados do faturamento mensal;
b) Podem existir dias sem faturamento, como nos finais de semana e feriados. Estes dias devem ser ignorados no cálculo da média;

``` java
import com.fasterxml.jackson.core.type.TypeReference;
import com.fasterxml.jackson.databind.ObjectMapper; //para mapear os dados do JSON em objetos Java.
import java.io.File;
import java.io.IOException;
import java.util.List;

class Faturamento {
    public int dia;
    public double valor;
}

public class FaturamentoDistribuidora {
    public static void main(String[] args) {
        try {
            // Carregando o arquivo JSON
            ObjectMapper objectMapper = new ObjectMapper();
            List<Faturamento> faturamentoDiario = objectMapper.readValue(
                    new File("faturamento.json"), new TypeReference<List<Faturamento>>() {});

            // Variáveis para armazenar os resultados
            double menorFaturamento = Double.MAX_VALUE;
            double maiorFaturamento = Double.MIN_VALUE;
            double somaFaturamento = 0.0;
            int diasComFaturamento = 0;

            // Calculando o menor e maior faturamento, e somando os valores para calcular a média
            for (Faturamento faturamento : faturamentoDiario) {
                if (faturamento.valor > 0) {  // Considera apenas dias com faturamento
                    if (faturamento.valor < menorFaturamento) {
                        menorFaturamento = faturamento.valor;
                    }
                    if (faturamento.valor > maiorFaturamento) {
                        maiorFaturamento = faturamento.valor;
                    }
                    somaFaturamento += faturamento.valor;
                    diasComFaturamento++;
                }
            }

            // Calculando a média mensal (somente dias com faturamento)
            double mediaMensal = somaFaturamento / diasComFaturamento;

            // Contando quantos dias tiveram faturamento superior à média
            int diasAcimaDaMedia = 0;
            for (Faturamento faturamento : faturamentoDiario) {
                if (faturamento.valor > mediaMensal) {
                    diasAcimaDaMedia++;
                }
            }

            // Exibindo os resultados
            System.out.println("Menor valor de faturamento: " + menorFaturamento);
            System.out.println("Maior valor de faturamento: " + maiorFaturamento);
            System.out.println("Número de dias com faturamento acima da média: " + diasAcimaDaMedia);

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

Para o JSON fornecido, a saída será algo como:
Menor valor de faturamento: 373.7838
Maior valor de faturamento: 48924.2448
Número de dias com faturamento acima da média: 10

```
4) Dado o valor de faturamento mensal de uma distribuidora, detalhado por estado:
• SP – R$67.836,43
• RJ – R$36.678,66
• MG – R$29.229,88
• ES – R$27.165,48
• Outros – R$19.849,53

Escreva um programa na linguagem que desejar onde calcule o percentual de representação que cada estado teve dentro do valor total mensal da distribuidora.  

``` java
public class PercentualFaturamento {
    public static void main(String[] args) {
        // Valores de faturamento por estado
        double sp = 67836.43;
        double rj = 36678.66;
        double mg = 29229.88;
        double es = 27165.48;
        double outros = 19849.53;

        // Calculando o valor total de faturamento
        double faturamentoTotal = sp + rj + mg + es + outros;

        // Calculando os percentuais de cada estado
        double percentualSP = (sp / faturamentoTotal) * 100;
        double percentualRJ = (rj / faturamentoTotal) * 100;
        double percentualMG = (mg / faturamentoTotal) * 100;
        double percentualES = (es / faturamentoTotal) * 100;
        double percentualOutros = (outros / faturamentoTotal) * 100;

        // Exibindo os resultados
        System.out.println("Percentual de representação por estado:");
        System.out.printf("SP: %.2f%%\n", percentualSP);
        System.out.printf("RJ: %.2f%%\n", percentualRJ);
        System.out.printf("MG: %.2f%%\n", percentualMG);
        System.out.printf("ES: %.2f%%\n", percentualES);
        System.out.printf("Outros: %.2f%%\n", percentualOutros);
    }
}
```
5) Escreva um programa que inverta os caracteres de um string.

IMPORTANTE:
a) Essa string pode ser informada através de qualquer entrada de sua preferência ou pode ser previamente definida no código;
b) Evite usar funções prontas, como, por exemplo, reverse;

``` java
import java.util.Scanner;

public class InverterString {
    public static void main(String[] args) {
        // Solicita ao usuário que informe a string a ser invertida
        Scanner scanner = new Scanner(System.in);
        System.out.print("Digite uma string para inverter: ");
        String original = scanner.nextLine();
        
        // Fecha o scanner
        scanner.close();

        // Converte a string para um array de caracteres
        char[] caracteres = original.toCharArray();
        
        // Variável para armazenar a string invertida
        String invertida = "";
        
        // Percorre o array de caracteres de trás para frente
        for (int i = caracteres.length - 1; i >= 0; i--) {
            invertida += caracteres[i];
        }
        
        // Exibe a string invertida
        System.out.println("String invertida: " + invertida);
    }
}
```




