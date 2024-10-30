<b>Identificando Erros</b>

1 - Clique na coluna que apresenta erros

2 - Adicionar Coluna - Coluna personalizada

3 - Digite try([nome_coluna])

4 - Dê um OK

5 - Uma nova coluna será acrescentada

6 - Expanda a coluna

7 - Clique em HasError e dê um Ok

![image](https://github.com/user-attachments/assets/65e17d53-8e67-4afc-b21c-eb956b83b9d5)


8 - Filtre os dadospara TRUE

Agora todos os erros estão visiveis na tabela para avaliação

<b> Transformando planilha anual de orçamento em planilha mensal no PowerQuery </>

Planilha modelo:

![image](https://github.com/user-attachments/assets/c1602592-0a80-49c2-8662-0fa87649a308)

1 - Adicionar Coluna - Coluna Personalizada

2 - Digite {1..12} - (o 12 é porque é equivalente a 12 meses)

![image](https://github.com/user-attachments/assets/d50eee54-99d1-4ed9-9c7a-3eb08853eaf3)

3 - Clique em Expandir para novas linhas

![image](https://github.com/user-attachments/assets/e2c1922b-2ce2-4ccc-93a4-4158fdf7b513)

4 - Clique na coluna Valor - (a coluna que possui o total do orçamento anual) - Transformar - Padrão - Dividir - digite 12 - ok

5 - Agora clique na coluna que está com os números dos meses - pressione Shift e clique em Ano - Transformar - Mescclar Colunas

![image](https://github.com/user-attachments/assets/3b52d64b-3754-403f-bf17-dc5147f12945)

6 - Altere o tipo de dados da nova coluna para Data

7 - Agora o valor total do orçamento está distribuido por mês.




