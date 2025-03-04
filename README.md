# target-systems

```csharp
//1) Observe o trecho de código abaixo: int INDICE = 13, SOMA = 0, K = 0;

using System;

public class IndexSum
{
    public static void Main(string[] args)
    {
        int INDICE = 13;
        int SOMA = 0;
        int K = 0;
        
        while(K < INDICE) {
            K = K + 1; 
            SOMA = SOMA + K;
        }
        
        Console.WriteLine($"Soma: {SOMA}");
    }
}

//2) Dado a sequência de Fibonacci, onde se inicia por 0 e 1 e o próximo valor sempre será a soma dos 2 valores anteriores (exemplo: 0, 1, 1, 2, 3, 5, 8, 13, 21, 34...), escreva um programa na linguagem que desejar onde, informado um número, ele calcule a sequência de Fibonacci e retorne uma mensagem avisando se o número informado pertence ou não a sequência.

//IMPORTANTE: Esse número pode ser informado através de qualquer entrada de sua preferência ou pode ser previamente definido no código;

using System;

public class Fibonacci
{
    public static void Main(string[] args)
    {
        Console.WriteLine($"Informe um numero: ");
        int input = int.Parse(Console.ReadLine());
        
        int la = 0, lb = 1, lc = 0;
        
        while(lc < input) {
            lc = la + lb;
            la = lb;
            lb = lc;
        }
        
        if(input == lc)
            Console.WriteLine($"O numero pertence a sequencia de fibonnaci: {input}");
        else 
            Console.WriteLine($"O numero nao pertence a sequencia de fibonnaci: {input}");
    }
}

* OBSERVAÇÃO - NÃO CONCLUÍDO. *
//3) Dado um vetor que guarda o valor de faturamento diário de uma distribuidora, faça um programa, na linguagem que desejar, que calcule e retorne:
//• O menor valor de faturamento ocorrido em um dia do mês;
//• O maior valor de faturamento ocorrido em um dia do mês;
//• Número de dias no mês em que o valor de faturamento diário foi superior à média mensal.

//IMPORTANTE:
//a) Usar o json ou xml disponível como fonte dos dados do faturamento mensal;
//b) Podem existir dias sem faturamento, como nos finais de semana e feriados. Estes dias devem ser ignorados no cálculo da média;

using System;
using System.Collections.Generic;

public class DailyBillingData
{
    public static void Main(string[] args)
    {
        // JSON com os dados de faturamento diário
        string jsonString = "[{\"dia\": 1, \"valor\": 22174.1664}, {\"dia\": 2, \"valor\": 24537.6698}, {\"dia\": 3, \"valor\": 26139.6134}, {\"dia\": 4, \"valor\": 0.0}, {\"dia\": 5, \"valor\": 0.0}, {\"dia\": 6, \"valor\": 26742.6612}, {\"dia\": 7, \"valor\": 0.0}, {\"dia\": 8, \"valor\": 42889.2258}, {\"dia\": 9, \"valor\": 46251.174}, {\"dia\": 10, \"valor\": 11191.4722}, {\"dia\": 11, \"valor\": 0.0}, {\"dia\": 12, \"valor\": 0.0}, {\"dia\": 13, \"valor\": 3847.4823}, {\"dia\": 14, \"valor\": 373.7838}, {\"dia\": 15, \"valor\": 2659.7563}, {\"dia\": 16, \"valor\": 48924.2448}, {\"dia\": 17, \"valor\": 18419.2614}, {\"dia\": 18, \"valor\": 0.0}, {\"dia\": 19, \"valor\": 0.0}, {\"dia\": 20, \"valor\": 35240.1826}, {\"dia\": 21, \"valor\": 43829.1667}, {\"dia\": 22, \"valor\": 18235.6852}, {\"dia\": 23, \"valor\": 4355.0662}, {\"dia\": 24, \"valor\": 13327.1025}, {\"dia\": 25, \"valor\": 0.0}, {\"dia\": 26, \"valor\": 0.0}, {\"dia\": 27, \"valor\": 25681.8318}, {\"dia\": 28, \"valor\": 1718.1221}, {\"dia\": 29, \"valor\": 13220.495}, {\"dia\": 30, \"valor\": 8414.61}]";

        // Chama o método para obter os dados de faturamento
        List<Tuple<int, double>> billing = GetBillingData(jsonString);

        // Exibe os valores
        foreach (var item in billing)
        {
            Console.WriteLine($"Dia: {item.Item1}, Faturamento: {item.Item2}");
        }
    }

    public static List<Tuple<int, double>> GetBillingData(string jsonString)
    {
        List<Tuple<int, double>> billing = new List<Tuple<int, double>>();

        // Removendo os colchetes e separando os itens
        jsonString = jsonString.Trim('[', ']');
        string[] items = jsonString.Split("}, {");

        for (int i = 0; i < items.Length; i++)
        {
            // Removendo as chaves { e }
            items[i] = items[i].Trim('{', '}');

            // Separando as partes chave:valor
            string[] keyValuePairs = items[i].Split(", ");

            int dia = 0;
            double valor = 0.0;

            // Extraindo os valores para "dia" e "valor"
            foreach (var keyValue in keyValuePairs)
            {
                var keyValueParts = keyValue.Split(": ");
                string key = keyValueParts[0].Trim('"');
                string value = keyValueParts[1].Trim('"');

                if (key == "dia")
                {
                    dia = int.Parse(value);
                }
                else if (key == "valor")
                {
                    valor = double.Parse(value);
                }
            }

            // Armazenando os valores no vetor
            billing.Add(Tuple.Create(dia, valor));
        }

        return billing;
    }
}

//4) Dado o valor de faturamento mensal de uma distribuidora, detalhado por estado:
//• SP – R$67.836,43
//• RJ – R$36.678,66
//• MG – R$29.229,88
//• ES – R$27.165,48
//• Outros – R$19.849,53
//Escreva um programa na linguagem que desejar onde calcule o percentual de representação que cada estado teve dentro do valor total mensal da distribuidora.

using System;
using System.Collections.Generic;

public class MonthlyBilling
{
    public static void Main(string[] args)
    {
        List<Tuple<string, double>> faturamentoMensal = new List<Tuple<string, double>>
        {
            Tuple.Create("SP", 67836.43),
            Tuple.Create("RJ", 36678.66),
            Tuple.Create("MG", 29229.88),
            Tuple.Create("ES", 27165.48),
            Tuple.Create("Outros", 19849.53),
        };
        
        double valorTotal = 0;
        foreach (var tupla in faturamentoMensal)
        {
            valorTotal += tupla.Item2;
        }
        
        foreach (var tupla in faturamentoMensal)
        {
            double tempValue = 0;
            
            tempValue = (tupla.Item2 / valorTotal) * 100;
            Console.WriteLine($"Porcentagem de {tupla.Item1}: {tempValue}");
        }
    }
}

//5) Escreva um programa que inverta os caracteres de um string.

//IMPORTANTE:
//a) Essa string pode ser informada através de qualquer entrada de sua preferência ou pode ser previamente definida no código;
//b) Evite usar funções prontas, como, por exemplo, reverse;

using System;

public class StringReverse
{
    public static void Main(string[] args)
    {
        Console.Write("Digite uma palavra: ");
        string entrada = Console.ReadLine();
        char[] caracteres = new char[entrada.Length];

        for (int i = 0; i < entrada.Length; i++)
        {
            caracteres[i] = entrada[i];
        }
        
        entrada = "";
        for (int i = caracteres.Length - 1; i >= 0; i--) {
           entrada += caracteres[i];
        }
        
        Console.WriteLine($"String invertida: {entrada}");
    }
}
