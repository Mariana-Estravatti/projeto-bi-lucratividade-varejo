01 SOMA DAS VENDAS = 

// Calcula o total do lucro dos pedidos que não estão marcados como devolvidos, aplicando um filtro na tabela de devoluções.

CALCULATE(

&#x20;   SUMX(F\_Pedidos, F\_Pedidos\[Lucro]),

&#x20;   dim\_devolucoes\[ID\_PEDIDO] <> "Yes"

)



02 VENDAS- Ano Anterior = 

// Calcula o valor da medida "01 SOMA DAS VENDAS" para o mesmo período do ano anterior, permitindo comparações anuais de desempenho.

CALCULATE(

&#x20;   \[01 SOMA DAS VENDAS],

&#x20;   SAMEPERIODLASTYEAR(dim\_tempo\[DATA])

)



03 Vendas Esse Ano = 

// Retorna o total da medida "01 SOMA DAS VENDAS" no contexto de filtro atual, representando as vendas do período selecionado.

CALCULATE(

&#x20;   \[01 SOMA DAS VENDAS]

)



04 ACUMULADO Atual = 

// Calcula o acumulado das vendas desde o início do ano até a data selecionada, permitindo acompanhar a evolução anual das vendas.

TOTALYTD(

&#x20;   \[03 Vendas Esse Ano],

&#x20;   dim\_tempo\[DATA]

)



05 Acumulado Ano Anterior = 

// Calcula o acumulado das vendas do mesmo período do ano anterior desde o início do ano até a data selecionada, permitindo comparar a evolução anual com o período anterior.

TOTALYTD(

&#x20;   \[02 VENDAS- Ano Anterior],

&#x20;   dim\_tempo\[DATA]

)



06 Quantidade = 

// Calcula a quantidade total de itens vendidos somando a coluna de quantidade da tabela de pedidos.

SUMX(

&#x20;   F\_Pedidos,

&#x20;   F\_Pedidos\[Quantidade]

)





07 Quantidade Ano Anterior = 

// Calcula a quantidade total de itens vendidos no mesmo período do ano anterior, permitindo comparar o volume de vendas entre os anos.

CALCULATE(

&#x20;   \[06 Quantidade],

&#x20;   SAMEPERIODLASTYEAR(dim\_tempo\[DATA])

)



08 Variação da Quantidade = 

// Calcula a variação percentual da quantidade vendida em relação ao mesmo período do ano anterior, exibindo "-" quando não houver variação válida.

IF(

&#x20;   DIVIDE(\[06 Quantidade], \[07 Quantidade Ano Anterior], 0) - 1 = -1,

&#x20;   "-",

&#x20;   DIVIDE(\[06 Quantidade], \[07 Quantidade Ano Anterior], 0) - 1

)



09 Ticket Médio Ano Anterior = 

// Calcula o ticket médio do mesmo período do ano anterior, permitindo comparar o valor médio por pedido entre os anos.

CALCULATE(

&#x20;   \[10 Ticket Médio Este Ano],

&#x20;   SAMEPERIODLASTYEAR(dim\_tempo\[DATA])

)



10 Ticket Médio Este Ano = 

// Calcula o ticket médio do período atual por meio da média do valor de lucro dos pedidos, retornando "R$ 0,00" quando não houver dados disponíveis.

VAR TM =

&#x20;   AVERAGEX(

&#x20;       FILTER(F\_Pedidos, F\_Pedidos\[Lucro]),

&#x20;       F\_Pedidos\[Lucro]

&#x20;   )

RETURN

&#x20;   IF(

&#x20;       ISBLANK(TM),

&#x20;       "R$ 0,00",

&#x20;       TM

&#x20;   )



11 Variação do Ticket Médio = 

// Calcula a variação percentual do ticket médio em relação ao mesmo período do ano anterior, exibindo "-" quando não houver comparação válida.

IF(

&#x20;   DIVIDE(\[10 Ticket Médio Este Ano], \[09 Ticket Médio Ano Anterior], 0) - 1 = -1,

&#x20;   "-",

&#x20;   DIVIDE(\[10 Ticket Médio Este Ano], \[09 Ticket Médio Ano Anterior], 0) - 1

)



12 Soma do Custo = 

// Calcula o custo total das vendas subtraindo o lucro obtido do valor total das vendas, estimando o custo dos produtos comercializados.

VAR l =

&#x20;   SUM(F\_Pedidos\[Lucro])



VAR vds =

&#x20;   SUM(F\_Pedidos\[Venda])



RETURN

&#x20;   vds - l



13 Lucro = 

// Calcula o lucro total do período somando todos os valores da coluna Lucro da tabela de pedidos.

SUM(F\_Pedidos\[Lucro])



14 Lucro Ano Anterior = 

// Calcula o lucro total do mesmo período do ano anterior, permitindo comparar o desempenho financeiro entre os anos.

CALCULATE(

&#x20;   \[13 Lucro],

&#x20;   SAMEPERIODLASTYEAR(dim\_tempo\[DATA])

)



15 Variação do Lucro = 

// Calcula a variação percentual do lucro em relação ao mesmo período do ano anterior, permitindo comparar o crescimento ou a redução do resultado financeiro entre os anos.

DIVIDE(

&#x20;   \[13 Lucro],

&#x20;   \[14 Lucro Ano Anterior],

&#x20;   0

) - 1



16 Variação das Vendas = 

// Calcula a variação percentual das vendas em relação ao mesmo período do ano anterior, exibindo "-" quando não houver comparação válida.

IF(

&#x20;   DIVIDE(\[01 SOMA DAS VENDAS], \[02 VENDAS- Ano Anterior], 0) - 1 = -1,

&#x20;   "-",

&#x20;   DIVIDE(\[01 SOMA DAS VENDAS], \[02 VENDAS- Ano Anterior], 0) - 1

)



17 Margem Bruta = 

// Calcula a margem bruta dividindo o lucro bruto (vendas menos custo) pelo valor total das vendas, indicando a rentabilidade percentual das vendas.

VAR Vendas =

&#x20;   SUMX(F\_Pedidos, F\_Pedidos\[Venda])



RETURN

&#x20;   (Vendas - \[12 Soma do Custo]) / Vendas



18 Margem Bruta Ano Anterior = 

// Calcula a margem bruta do mesmo período do ano anterior, permitindo comparar a rentabilidade das vendas entre os anos.

CALCULATE(

&#x20;   \[17 Margem Bruta],

&#x20;   SAMEPERIODLASTYEAR(dim\_tempo\[DATA])

)



19 Variação da Margem Bruta = 

// Calcula a variação percentual da margem bruta em relação ao mesmo período do ano anterior, exibindo "-" quando não houver comparação válida.

IF(

&#x20;   DIVIDE(\[17 Margem Bruta], \[18 Margem Bruta Ano Anterior], 0) - 1 = -1,

&#x20;   "-",

&#x20;   DIVIDE(\[17 Margem Bruta], \[18 Margem Bruta Ano Anterior], 0) - 1

)



20 Quantidade de Vendas Este Ano = 

// Calcula a quantidade total de pedidos únicos no período atual, contando cada venda apenas uma vez por meio do identificador do pedido.

DISTINCTCOUNT(F\_Pedidos\[ID\_Pedido])



21 Quantidade de Tickets Ano Anterior = 

// Calcula a quantidade de pedidos únicos do mesmo período do ano anterior, permitindo comparar o volume de vendas entre os anos.

CALCULATE(

&#x20;   \[20 Quantidade de Vendas Este Ano],

&#x20;   SAMEPERIODLASTYEAR(dim\_tempo\[DATA])

)



22 Variação da Quantidade de Tickets = 

// Calcula a variação percentual da quantidade de pedidos únicos em relação ao mesmo período do ano anterior, exibindo "-" quando não houver comparação válida.

IF(

&#x20;   DIVIDE(\[20 Quantidade de Vendas Este Ano], \[21 Quantidade de Tickets Ano Anterior], 0) - 1 = -1,

&#x20;   "-",

&#x20;   DIVIDE(\[20 Quantidade de Vendas Este Ano], \[21 Quantidade de Tickets Ano Anterior], 0) - 1

)



