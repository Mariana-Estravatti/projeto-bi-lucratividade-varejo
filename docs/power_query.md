# Documentação do Power Query (ETL)

## Projeto de Business Intelligence e Analytics – Lucratividade no Varejo

Este documento apresenta a documentação das consultas desenvolvidas no Power Query durante o processo de ETL (*Extract, Transform and Load*). As transformações realizadas tiveram como objetivo preparar os dados da base **Sample Superstore** para a modelagem estrela utilizada no Power BI.

## Pipeline de ETL

O fluxo de preparação dos dados segue a seguinte arquitetura:

Excel (Sample Superstore)

↓

Power Query (ETL)

↓

Modelagem Estrela

↓

Medidas DAX

↓

Dashboards Power BI

Durante o ETL foram realizadas atividades de:

- Importação das planilhas do arquivo Excel;
- Padronização dos tipos de dados;
- Renomeação das colunas para português;
- Remoção de duplicidades;
- Separação entre tabela fato e dimensões;
- Preparação das tabelas para relacionamento.

---

# Índice

1. F_Pedidos
2. dim_tempo
3. dim_cliente
4. dim_produto
5. dim_cep
6. dim_devolucoes
7. dim_gerentes

---

# 1. Consulta: F_Pedidos

## Objetivo

Construir a tabela fato do projeto contendo as transações de vendas.

## Transformações realizadas

- Importação da aba Orders;
- Conversão dos tipos de dados;
- Renomeação das colunas;
- Conversão dos valores monetários;
- Remoção de colunas normalizadas.

### Código comentado

```powerquery
// Importa o arquivo Excel da base Sample Superstore.
Origem = Excel.Workbook(...)

// Seleciona a aba Orders.
#"Navegação 1" = ...

// Promove a primeira linha para cabeçalhos.
#"Cabeçalhos promovidos" = ...

// Padroniza os tipos de dados.
#"Tipo de coluna alterado" = ...

// Renomeia as colunas para português.
#"Colunas Renomeadas" = ...

// Remove colunas desnecessárias.
#"Colunas Removidas" = ...

// Converte valores financeiros para moeda.
#"Tipo Alterado" = ...

// Mantém apenas as chaves necessárias.
#"Colunas Removidas1" = ...
```

A consulta implementa a tabela fato do modelo estrela, preservando apenas as métricas e chaves necessárias para as análises.

---

# 2. Consulta: dim_tempo

## Objetivo

Criar uma dimensão calendário dinâmica baseada nas datas existentes na tabela de pedidos.

## Transformações realizadas

- Identificação da menor e maior data;
- Geração automática do calendário;
- Criação das colunas Ano, Mês, Dia;
- Criação do campo Mes_Ano.

### Código comentado

```powerquery
// Obtém a menor data.
DataMinima = ...

// Obtém a maior data.
DataMaxima = ...

// Gera o calendário.
Fonte = List.Dates(...)

// Cria Ano, Mês e Dia.
#"Ano Inserido" = ...
#"Mês Inserido" = ...
#"Dia Inserido" = ...
```

Essa dimensão permite utilizar funções temporais do DAX como `TOTALYTD` e `SAMEPERIODLASTYEAR`.

---

# 3. Consulta: dim_cliente

## Objetivo

Criar a dimensão de clientes utilizada no modelo estrela.

## Transformações realizadas

- Importação da aba Orders;
- Seleção dos atributos dos clientes;
- Remoção de duplicidades por ID_Cliente.

### Código comentado

```powerquery
// Importa a base Orders.
Origem = ...

// Mantém apenas Nome, ID e Segmento.
#"Outras Colunas Removidas" = ...

// Remove clientes duplicados.
#"Duplicatas Removidas" = ...
```

Cada cliente passa a existir apenas uma vez na dimensão.

---

# 4. Consulta: dim_produto

## Objetivo

Criar a dimensão de produtos.

## Transformações realizadas

- Importação da aba Orders;
- Seleção dos atributos dos produtos;
- Remoção de duplicidades por ID_Produto.

### Código comentado

```powerquery
// Importa a base Orders.
Origem = ...

// Mantém Categoria, Subcategoria e Produto.
#"Outras Colunas Removidas" = ...

// Remove produtos duplicados.
#"Duplicatas Removidas" = ...
```

Essa dimensão permite análises por categoria, subcategoria e produto.

---

# 5. Consulta: dim_cep

## Objetivo

Criar a dimensão geográfica do projeto.

## Transformações realizadas

- Importação da aba Orders;
- Seleção dos atributos geográficos;
- Remoção de duplicidades por CEP;
- Reordenação das colunas.

### Código comentado

```powerquery
// Importa a base Orders.
Origem = ...

// Mantém apenas as informações geográficas.
#"Outras Colunas Removidas" = ...

// Remove CEPs duplicados.
#"Duplicatas Removidas" = ...
```

A dimensão permite análises por cidade, estado e região.

---

# 6. Consulta: dim_devolucoes

## Objetivo

Identificar os pedidos devolvidos.

## Transformações realizadas

- Importação da aba Returns;
- Conversão dos tipos;
- Renomeação das colunas.

### Código comentado

```powerquery
// Importa a aba Returns.
Origem = ...

// Renomeia Order ID para ID_PEDIDO.
#"Colunas Renomeadas" = ...
```

Essa dimensão é utilizada para excluir devoluções em indicadores financeiros.

---

# 7. Consulta: dim_gerentes

## Objetivo

Relacionar cada região ao respectivo gerente regional.

## Transformações realizadas

- Importação da aba People;
- Conversão dos tipos de dados.

### Código comentado

```powerquery
// Importa a aba People.
Origem = ...

// Define os tipos das colunas.
#"Tipo Alterado" = ...
```

A dimensão complementa as análises regionais realizadas no dashboard.

---

# Considerações finais

A etapa de ETL foi responsável por transformar a base original **Sample Superstore** em uma estrutura adequada para análises de Business Intelligence.

A modelagem adotada segue o esquema **estrela**, no qual:

- `F_Pedidos` concentra as transações comerciais;
- `dim_tempo` organiza as análises temporais;
- `dim_cliente` reúne informações dos clientes;
- `dim_produto` centraliza os atributos dos produtos;
- `dim_cep` organiza a localização geográfica;
- `dim_devolucoes` identifica pedidos devolvidos;
- `dim_gerentes` relaciona regiões aos respectivos responsáveis.

Essa estrutura melhora o desempenho das consultas, reduz redundâncias e permite a construção eficiente dos indicadores desenvolvidos no Power BI.