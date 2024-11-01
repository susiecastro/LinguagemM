<h1>Dicas de PowerQuery</h1>

<b>Calendário com Feriados Automáticos</b>

1 - Abra o PowerBI e clique em Transformar Dados

2 - Clinque no botão - Nova Fonte - Consulta Nula 

3 - Ao lado esquerdo da tela aparecerá Consulta 1

4 - Na caixa de ferramentas clique no botão - Editor Avançado

5 - Apaque tudo que aparece na tela que abre e cole o código abaixo:
        
        //DI = Data Início
        //DF = Data Fim
        
        let dCalendario =(DI as date, DF as date) as table =>
        
        let
        //Contar número de dias entre a data de início e fim
         Dias = Duration.Days(DF - DI) +1,
        //Criando uma lista de datas
         Datas = List.Dates(DI, Dias, 
         #duration(1,0,0,0)),
        //Converter Lista em Tabela
         ListaparaTabela = Table.FromList(Datas, 
         Splitter.SplitByNothing(), {"Data"}, null, ExtraValues.Error ),
        AlterarTipo = Table.TransformColumnTypes(ListaparaTabela,{{"Data", type date}}),
        //Criando Colunas adicionais
         //Coluna Ano
         Ano = Table.AddColumn(AlterarTipo, "Ano", 
         each Date.Year([Data]), Int64.Type),
        //Criando Trimestre
         Trimestre = Table.AddColumn(Ano , "Trimestre", 
         each "Trim " & Number.ToText(Date.QuarterOfYear([Data])), type text),
        //Número da Semana
         NumeroSemana = Table.AddColumn(Trimestre , "Número Semana", 
         each Date.WeekOfYear([Data]), Int64.Type),
        //Numero Mês
         MesNumero = Table.AddColumn(NumeroSemana, "Número Mês", 
         each Date.Month([Data]), Int64.Type),
         DataINT = Table.AddColumn(MesNumero, "DateInt", each [Ano]*100 + [Número Mês], Int64.Type),
        //Nome do Mes
         NomeMes = Table.AddColumn(DataINT , "Mês", 
         each Date.ToText([Data],"MMM"), type text),
         MesMaiusculo = Table.TransformColumns(NomeMes,{{"Mês", Text.Proper, type text}}),
        //Dia da Semana
         DiaDaSemana = Table.AddColumn(MesMaiusculo , "Dia da Semana", 
         each Date.ToText([Data],"dddd"), type text),
        //Mês-Ano
         #"MesAno" = Table.AddColumn(DiaDaSemana, "Mês Ano", each Text.Combine({Text.From([Número Mês], "pt-BR"), Text.From([Ano], "pt-BR")}, "-"), type text),
        //NomeMês-Ano
         #"NomeMesAno2" = Table.AddColumn(MesAno, "Nome Mês e Ano", each Text.Combine({Date.ToText([Data],"MMM"),Text.From([Ano], "pt-BR")}, "-"),type text)
        
        in
         NomeMesAno2
        
        in dCalendario


6 - Defina a Data incial da sua tabela calendario e a Data final - após digitar clique em invocar. Segue o exemplo:

![image](https://github.com/user-attachments/assets/11f4807e-66c5-4f19-9516-7aac5f7d6837)

7 - Ao lado esquerdo renomeie a função e a tabela calendário (passo sugestão - para organização)

![image](https://github.com/user-attachments/assets/69d2e776-3cd5-41cf-9b90-d0c220c9b115)

8 - Iremos automatizar nossos calendários com base na tabela feriados da ANBIMA. 
Para isto vá em Nova Fonte - Página da Web - Cole o endereço abaixo:

https://www.anbima.com.br/feriados/fer_nacionais/2019.asp

9 - O site irá carregar e aparecerá uma tela com a tabela de Feriados Nacionais, selecione e de um OK

![image](https://github.com/user-attachments/assets/4c0c780e-701b-4c55-86d0-2986ca8048fe)

10 - Renomeie esta tabela para fnFeriados

11 - Clique em Gerenciar Parâmetros - Novo Parâmetros

![image](https://github.com/user-attachments/assets/e8bde809-bc37-409d-9bd4-9c4655a5cc76)

12 - Retorne para a fnFeriados e do lado direito clique na primeira etapa (FONTE)

![image](https://github.com/user-attachments/assets/b7791fae-36ea-4e60-90dd-b39d20208d45)

13 - Em frente a palavra Fonte há o desenho de uma engrenagem, clique neste botão para aparecer esta tela:

![image](https://github.com/user-attachments/assets/45dac74d-4f2d-4def-8818-8ebec3161fc0)

14 - Na tela acima está marcado a opção Básico, altere para Avançadas

15 - Faça então as seguintes alterações abaixo (para adicionar uma linha mais clique no botão Adicionar Parte)

![image](https://github.com/user-attachments/assets/c62e98c9-4c77-4fa4-9d4c-3f7425cbc4c4)

16 - Em fonte o endereço está assim: = Web.BrowserContents("https://www.anbima.com.br/feriados/fer_nacionais/" & Ano & ".asp")

![image](https://github.com/user-attachments/assets/927fef73-05d3-4e7a-b1a1-d177a0dc8507)

17 - Remova os espaços que estão entre as apas (") e o (&) e comercial que estão entre a palavra Ano.  
= Web.BrowserContents("https://www.anbima.com.br/feriados/fer_nacionais/"& Ano &".asp")

![image](https://github.com/user-attachments/assets/01be41a8-2c93-4926-9cac-c8f2d3afaa2a)

18 - Com a fnFeriados Selecionada clique em Editor Avançado

19 - Adiciona a seguinte linha antes do código que está escrito: (Ano as text) =>

20 - Clique no paramêtro Ano e exclua, ele só serviu de base para criarmos a função.

21 - Clique com o botão direito na tabela Calendario e clique em refêrencia.

22 - Na consulta referencia clique com o botão direito na coluna Ano e clique em Remover outras colunas.

23 - Clique novamente com o botão direito e na opção Remover Duplicadas

24 - Altere o tipo de dados de Número Inteiro para Texto

25 - Selecione a Coluna Ano - Clique em Adicionar Coluna - Invocar Função Personalizada

26 - Nome da Coluna - Feriados - Invocar Função: fnFeriados

27 - Clique no botão Expandir na nova Coluna

28 - Altere os tipos de Dados conforme desejar





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

<b> Transformando planilha anual de orçamento em planilha mensal no PowerQuery </b>

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

<b>Dividindo Linhas e Colunas</b>

Tabela a ser tratada:
![image](https://github.com/user-attachments/assets/c424fc84-67b7-49f1-8c67-0a516a87132a)

1 - Importe para o PowerQuery

2 - Clique na coluna resultados

3 - Em Página Inicial cliquem no botão Dividir colunas Por delimitador - delimitador: virgula - Cada Ocorrência.
Clique em Opções Avançadas - marque Linhas

4 - A tabela ficará assim:

![image](https://github.com/user-attachments/assets/a29bf67a-37ab-489c-9c4a-b1143c4459fd)

5 - Clique novamente em Resultados

6 - Acione novamente o botão Dividir Colunas

7 - Delimitador : dois pontos - Cada Ocorrência

8 - A tabela ficará assim:

![image](https://github.com/user-attachments/assets/3e9bafa4-2749-4394-a931-a05ee79c399c)

9 - Observe que na coluna há 4 valores distintos. Isto deve-se a algum espaço que está no inicio ou no final de cada palavra

10 - Para corrigir  isto vá em Transformar - Formato - Cortar

11 - Então a tabela ficará assim:

![image](https://github.com/user-attachments/assets/ca215ba4-2005-409c-86c2-b10923ad8f88)

<b> Transposição de Tabela e Convertendo Tabela em linhas</b>

Desafio transformar a tabela abaixo na estrutura tabular:


![image](https://github.com/user-attachments/assets/8e822f43-906a-479f-ae04-2ca4bc069a2f)

1 - Importe a tabela para o PowerQuery

2 - Remova as etapas feitas automaticamente no carregamento da planilha - Tipo Alterado e Cabeçalhos Promovidos

3 - Na Coluna onde está Categoria 1 e Categoria 2 (primeira coluna) - Selecione e acesse - Transformar - Preencher para Baixo 

![image](https://github.com/user-attachments/assets/382e93aa-4347-4f9b-8658-74f05b98c79e)

4 - Na segunda coluna Filtre e desabilite o Total

5 - Selecione as duas primeiras colunas - Clique com o botão direito - Mesclar Colunas
![image](https://github.com/user-attachments/assets/dfc48a82-32e4-42d5-91ef-f655f02b0cb2)

6 - Em separador pode escolher um de sua preferência e clique em ok
![image](https://github.com/user-attachments/assets/45496082-b0ee-4a5b-a724-bc5ba9a98af5)

7 - Agora clique em Transformar - Transpor

8 - Novamente na primeira coluna - Transformar - Transformar - Preencher para Baixo 
![image](https://github.com/user-attachments/assets/382e93aa-4347-4f9b-8658-74f05b98c79e)

9 - Promova os Cabeçalhos da estrutura existente
![image](https://github.com/user-attachments/assets/110eb01b-ffb8-47f8-9689-318b1950205c)

10 - Selecione as duas primeiras colunas e clique em - Transformar outras colunas em linhas
![image](https://github.com/user-attachments/assets/f359dbd4-d6f6-4941-a2eb-33f1118d5e98)

12 - Clique na Coluna Atributo - Dividir Coluna - Por Delimitador:
![image](https://github.com/user-attachments/assets/040983d9-56b9-4dcf-b707-56d2e4969ee2)

13 - Coloque o mesmo delimitador que você colocou no passo 3:
![image](https://github.com/user-attachments/assets/46ce7d8c-4bd4-43b0-872a-1181833d15cc)

14 - Renomeie as colunas e configure os tipos de dados. Segue sugestão na imagem abaixo:
![image](https://github.com/user-attachments/assets/e2f5ff26-7485-4781-8b9b-db885bd18407)











