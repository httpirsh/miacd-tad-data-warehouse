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

Grão: `Data × Região × Modalidade` (811 linhas após agregação)

| Campo | Descrição |
|---|---|
| `id_tempo` | FK → Dim_Tempo |
| `id_regiao` | FK → Dim_Regiao |
| `id_modalidade` | FK → Dim_Modalidade |
| `num_alojamentos` | Nº de AL |
| `total_utentes` | Capacidade total |

### Dimensões

**Dim_Tempo** (401 datas): `id_tempo`, `ano`, `mes`, `trimestre`, `data_registo`

**Dim_Regiao** (48 regiões): `id_regiao`, `Concelho`, `Distrito`, `NUTSII`

**Dim_Modalidade** (5 modalidades): `id_modalidade`, `modalidade`

---

## Processo ETL

1. **Extração** — Fonte: `Estabelecimentos_de_Alojamento_Local.csv` com encoding ISO-8859-1
2. **Transformação** — Pandas para limpeza e normalização de datas e regiões
3. **Dimensões** — Criação de tabelas de dimensão com valores únicos
4. **Fact** — Agregação por `id_tempo, id_regiao, id_modalidade`
5. **Exportação** — Saída em formato CSV para análise

---

## Visualização

Os dados podem ser importados para ferramentas de visualização como Power BI ou Tableau para análise:

- Evolução temporal de AL por região
- Comparação regional de capacidade
- Distribuição de modalidades por concelho

---

## Resultados

- **Dim_Tempo:** 401 linhas (datas únicas de registo)
- **Dim_Regiao:** 48 linhas (combinações concelho/distrito/NUTS II)
- **Dim_Modalidade:** 5 linhas (tipos de alojamento)
- **Fact_OfertaAL:** 811 linhas (agregações)

---

## Estrutura do Projeto

```
miacd-tad-data-warehouse/
├── Estabelecimentos_de_Alojamento_Local.csv  # Dataset original
├── star-schema                                # Script ETL Python
├── requirements.txt                           # Dependências
├── Dim_Tempo.csv                             # Dimensão de tempo
├── Dim_Regiao.csv                            # Dimensão de região
├── Dim_Modalidade.csv                        # Dimensão de modalidade
├── Fact_OfertaAL.csv                         # Tabela de factos
└── README.md
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
| Dim_Tempo | 401 linhas |
| Dim_Regiao | 48 linhas |
| Dim_Modalidade | 5 linhas |
| Fact_OfertaAL | 811 linhas |
| Tempo de execução | < 5 segundos |

---

## Tecnologias

- Python 3.14
- Pandas
- NumPy
- SQLAlchemy
