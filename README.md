# 🏨 Data Warehouse -- Alojamento Local em Portugal

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Data%20Warehouse-blue)
![ETL](https://img.shields.io/badge/ETL-Incremental-green)
![OLAP](https://img.shields.io/badge/OLAP-Analysis-orange)
![Status](https://img.shields.io/badge/Status-Academic%20Project-lightgrey)

------------------------------------------------------------------------

## 📌 Descrição do Projeto

Este projeto consiste na construção de um **Data Warehouse (DW)** para
análise da oferta registada de **Alojamento Local (AL)** em Portugal
Continental, utilizando dados do RNAL (Registo Nacional de Alojamento
Local).

O objetivo é suportar:

-   📊 Planeamento turístico\
-   🏙️ Ordenamento do território\
-   🛂 Fiscalização e políticas públicas\
-   📈 Monitorização de crescimento regional

**Dataset base:**\
- 112.685 registos\
- Informação sobre localização hierárquica, capacidade, datas,
modalidade e selo Clean & Safe

------------------------------------------------------------------------

# ❓ Big Questions

O DW foi desenhado para responder às seguintes questões:

1.  Crescimento anual de AL por região\
2.  Regiões com maior capacidade de alojamento\
3.  Modalidades dominantes por região\
4.  Distribuição do selo Clean & Safe\
5.  Identificação de pressão territorial (densidade AL)

------------------------------------------------------------------------

# 🏗️ Arquitetura

## ⭐ Star Schema

                    Dim_Tempo
                         |
    Dim_Regiao —— Fact_OfertaAL —— Dim_Modalidade

------------------------------------------------------------------------

## 📊 Fact Table

**FACT_OfertaAL**\
Grão: `Ano × Concelho × Modalidade`

  Campo             Descrição
  ----------------- -------------------
  id_tempo          FK Dim_Tempo
  id_regiao         FK Dim_Regiao
  id_modalidade     FK Dim_Modalidade
  num_alojamentos   Nº AL
  total_utentes     Capacidade total
  pct_cleansafe     \% com selo

\~8.000 linhas após agregação.

------------------------------------------------------------------------

## 📅 Dimensões

### DIM_TEMPO (\~10 anos)

-   id_tempo (PK)\
-   ano\
-   mes\
-   trimestre\
-   data_registo

### DIM_REGIAO (\~300 concelhos)

-   id_regiao (PK)\
-   concelho\
-   distrito\
-   nuts_ii

### DIM_MODALIDADE (\~5 tipos)

-   id_modalidade (PK)\
-   modalidade

------------------------------------------------------------------------

# 🔄 Processo ETL

### 1️⃣ Extração

-   Fonte: `RNAL.csv`
-   Carregamento para `staging_rnal_raw`
-   Limpeza de dados (datas, nulos, normalização regional)

### 2️⃣ Construção Dimensões

``` sql
SELECT DISTINCT YEAR(data_registo);
SELECT DISTINCT concelho, distrito, nuts_ii;
SELECT DISTINCT modalidade;
```

### 3️⃣ Construção Fact

``` sql
GROUP BY ano, concelho, modalidade;
```

------------------------------------------------------------------------

## 🔁 Atualizações

-   Tipo: Incremental\
-   Frequência: Trimestral\
-   500--1000 novos AL por trimestre\
-   SCD Tipo 2 na Dim_Regiao

------------------------------------------------------------------------

# 📊 Visualização (Power BI / Tableau)

### Dashboard 1 -- Evolução Temporal

-   AL por ano × NUTS II\
-   Drill-down até concelho\
-   Mapa de densidade

### Dashboard 2 -- Comparação Regional

-   Top 10 concelhos\
-   Heatmap modalidade × região

### Dashboard 3 -- Qualidade

-   \% Clean & Safe\
-   Filtros interativos

------------------------------------------------------------------------

# ⚙️ Performance & Otimização

### Índices

-   PK composta: `(id_tempo, id_regiao, id_modalidade)`
-   Índices nas FKs

### Particionamento

-   Fact particionada por ano

### Materialized Views

-   `mv_top_concelhos`

------------------------------------------------------------------------

# 📁 Estrutura do Projeto

    dw-alojamento-local/
    │
    ├── data/
    │   └── RNAL.csv
    │
    ├── etl/
    │   ├── staging.sql
    │   ├── dimensions.sql
    │   └── fact.sql
    │
    ├── schema/
    │   └── star_schema.sql
    │
    ├── dashboards/
    │   └── powerbi.pbix
    │
    ├── docs/
    │   └── relatorio.pdf
    │
    └── README.md

------------------------------------------------------------------------

# 🚀 Instalação

### 1️⃣ Criar Base de Dados

``` sql
CREATE DATABASE dw_al;
```

### 2️⃣ Executar scripts

1.  `schema/star_schema.sql`\
2.  `etl/staging.sql`\
3.  `etl/dimensions.sql`\
4.  `etl/fact.sql`

### 3️⃣ Ligar Power BI / Tableau ao PostgreSQL

------------------------------------------------------------------------

# 📊 Métricas Técnicas

  Métrica              Valor
  -------------------- ----------------
  Registos fonte       112.685
  Fact final           \~8.000 linhas
  1ª carga             \~2 minutos
  Update incremental   \~30 segundos

------------------------------------------------------------------------

# 🛠️ Tecnologias

-   PostgreSQL\
-   SQL\
-   Power BI / Tableau\
-   ETL (SQL / Python)

------------------------------------------------------------------------

# 📌 Conclusão

Este Data Warehouse permite:

✔ Monitorizar crescimento do AL\
✔ Avaliar pressão territorial\
✔ Apoiar decisões públicas\
✔ Analisar qualidade da oferta
