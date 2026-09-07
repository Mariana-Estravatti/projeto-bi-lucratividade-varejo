# Projeto de Business Intelligence e Analytics

![Power BI](https://img.shields.io/badge/Power_BI-F2C811?logo=powerbi\&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-Data_Analysis-blue)

## Objetivo

Identificar os fatores que influenciam a lucratividade dos produtos no varejo utilizando Business Intelligence e Analytics.

## Ferramentas

* Power BI
* Power Query
* DAX
* GitHub

## Estrutura

* `data/`
* `powerbi/`
* `analytics/`
* `dax/`
* `docs/`
* `images/`

# Modelo de Dados

O projeto utiliza uma modelagem estrela (*Star Schema*), na qual a tabela fato concentra as transações comerciais e as tabelas dimensão organizam as informações necessárias para as análises temporais, geográficas e de produtos.

## Estrutura

| Tabela             | Função                                                      |
| ------------------ | ----------------------------------------------------------- |
| **F_Pedidos**      | Tabela fato contendo vendas, lucro, descontos e quantidade. |
| **dim_tempo**      | Dimensão calendário utilizada para análises temporais.      |
| **dim_cliente**    | Informações dos clientes e segmento.                        |
| **dim_produto**    | Categorias, subcategorias e produtos.                       |
| **dim_cep**        | Cidade, estado, país e região.                              |
| **dim_gerentes**   | Gerentes responsáveis pelas regiões.                        |
| **dim_devolucoes** | Identificação dos pedidos devolvidos.                       |

## Relacionamentos

Todos os relacionamentos seguem cardinalidade **1:N**, característica típica de uma modelagem estrela.

* `dim_tempo → F_Pedidos`
* `dim_cliente → F_Pedidos`
* `dim_produto → F_Pedidos`
* `dim_cep → F_Pedidos`
* `dim_devolucoes → F_Pedidos`
* `dim_gerentes → dim_cep`

## Justificativa da modelagem

A separação entre tabela fato e dimensões reduz redundâncias, melhora o desempenho das consultas DAX e facilita a construção de indicadores como Receita Total, Lucro, Ticket Médio e Margem Bruta.


## KPIs

* Receita Total
* Lucro
* Margem Bruta
* Ticket Médio
* Quantidade de Vendas
* Evolução Temporal
* Lucro por Categoria
* Lucro por Região

## Dashboard

O projeto possui dois dashboards executivos desenvolvidos no Power BI.



****
