# Data Warehouse — Alojamento Local em Portugal

![Python](https://img.shields.io/badge/Python-3.14-blue)
![Pandas](https://img.shields.io/badge/Pandas-ETL-green)
![Status](https://img.shields.io/badge/Status-Academic%20Project-lightgrey)

---

## Descrição do Projeto

Este projeto consiste na construção de um **Data Warehouse (DW)** para análise da oferta registada de **Alojamento Local (AL)** em Portugal Continental, utilizando dados do RNAL (Registo Nacional de Alojamento Local).

O objetivo é suportar:

- Planeamento turístico
- Ordenamento do território
- Fiscalização e políticas públicas
- Monitorização de crescimento regional

**Dataset base:** Estabelecimentos de Alojamento Local com informação sobre localização hierárquica, capacidade, datas e modalidade.

---

## Big Questions

O DW foi desenhado para responder às seguintes questões:

1. Crescimento anual de AL por região
2. Regiões com maior capacidade de alojamento
3. Modalidades dominantes por região
4. Distribuição do selo Clean & Safe
5. Identificação de pressão territorial (densidade AL)

---

## Arquitetura — Star Schema

```
                  Dim_Tempo
                      |
Dim_Regiao —— Fact_OfertaAL —— Dim_Modalidade
```

### Fact Table — `Fact_OfertaAL`

Grão: `ANO × CONCELHO × MODALIDADE` (8,303 linhas após agregação)

| Campo | Descrição |
|---|---|
| `id_tempo` | FK → Dim_Tempo |
| `id_regiao` | FK → Dim_Regiao |
| `id_modalidade` | FK → Dim_Modalidade |
| `num_alojamentos` | Nº de AL registados |
| `total_utentes` | Capacidade total de utentes |

### Dimensões

**Dim_Tempo** (71 anos): `id_tempo`, `ano` (1900-2026)

**Dim_Regiao** (278 regiões): `id_regiao`, `Concelho`, `Distrito`, `NUTS_II`

**Dim_Modalidade** (5 modalidades): `id_modalidade`, `modalidade`
- Apartamento
- Moradia
- EstabelecimentoHospedagem
- Quartos
- EstabelecimentoHospedagemHostel

---

## Processo ETL

1. **Extração** — Fonte: `Estabelecimentos_de_Alojamento_Local.csv` (112,661 linhas carregadas)
2. **Limpeza** — Remoção de registos com dados essenciais em falta (encoding ISO-8859-1)
3. **Transformação** — Pandas para normalização de datas e criação de dimensão temporal anual
4. **Dimensões** — Criação de tabelas de dimensão com valores únicos:
   - Tempo: Agregação por ano (grão anual)
   - Região: Combinações únicas de Concelho/Distrito/NUTS II
   - Modalidade: Tipos únicos de alojamento
5. **Fact** — Agregação por `id_tempo × id_regiao × id_modalidade` com métricas:
   - `num_alojamentos`: Contagem de estabelecimentos
   - `total_utentes`: Soma da capacidade
6. **Exportação** — Saída em formato CSV para análise

---

## Visualização

Os dados podem ser importados para ferramentas de visualização como Power BI ou Tableau para análise:

- Evolução temporal de AL por região
- Comparação regional de capacidade
Após execução do pipeline ETL:

- **Dim_Tempo:** 71 linhas (anos de 1900 a 2026)
- **Dim_Regiao:** 278 linhas (combinações concelho/distrito/NUTS II)
- **Dim_Modalidade:** 5 linhas (tipos de alojamento)
- **Fact_OfertaAL:** 8,303 linhas (agregações ANO × CONCELHO × MODALIDADE)

### Estrutura dos Ficheiros CSV

**Dim_Tempo.csv**
```csv
id_tempo,ano
1,1900
2,1930
3,1935
...
```

**Dim_Regiao.csv**
```csv
Concelho,Distrito,NUTS_II,id_regiao
Olhão,Faro,Algarve,1
Tavira,Faro,Algarve,2
...
```

**Dim_Modalidade.csv**
```csv
id_modalidade,modalidade
1,Apartamento
2,Moradia
3,EstabelecimentoHospedagem
4,Quartos
5,EstabelecimentoHospedagemHostel
```

**Fact_OfertaAL.csv**
```csv
id_tempo,id_regiao,id_modalidade,num_alojamentos,total_utentes
1,70,1,4,14
1,70,5,1,17
...
```

---

## Estrutura do Projeto

```
miacd-tad-data-warehouse/
├── Estabelecimentos_de_Alojamento_Local.csv  # Dataset original (RNAL)
├── star-schema                                # Script ETL Python
├── requirements.txt                           # Dependências Python
├── README.md                                  # Documentação
├── Dim_Tempo.csv                              # Dimensão temporal (71 anos)
├── Dim_Regiao.csv                             # Dimensão geográfica (278 regiões)
├── Dim_Modalidade.csv                         # Dimensão de modalidade (5 tipos)
└── Fact_OfertaAL.csv                          # Tabela de factos (8,303 registos)
```

---

## Instalação e Execução

1. Criar e ativar ambiente virtual:
```bash
python -m venv .venv
source .venv/bin/activate  # Linux/Mac
# ou
.venv\Scripts\activate     # Windows
```

2. Instalar dependências:
```bash
pip install -r requirements.txt
```

3. Executar o script ETL:
```bash
python star-schema
```

4. Os ficheiros CSV serão gerados no diretório atual

---

## Métricas Técnicas

| Métrica | Valor |
|---|---|
| Dataset Original | 112,661 linhas |
| Dim_Tempo | 71 anos (1900-2026) |
| Dim_Regiao | 278 regiões |
| Dim_Modalidade | 5 modalidades |
| Fact_OfertaAL | 8,303 agregações |
| Grão da Fact | ANO × CONCELHO × MODALIDADE |
| Tempo de execução | < 5 segundos |

---

## Tecnologias

- Python 3.13
- Pandas
- NumPy
- SQLAlchemy
