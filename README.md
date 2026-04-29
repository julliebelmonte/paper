# State of Data Brasil — Análise de Desigualdades de Gênero (2021–2024)

Análise longitudinal das edições 2021, 2022, 2023 e 2024 do **State of Data Brasil**, com foco nas desigualdades de gênero no mercado de dados. O estudo combina análise exploratória, inferência estatística, modelagem preditiva com Machine Learning e explicabilidade.

---

## Objetivo

Investigar como gênero se relaciona com **remuneração**, **faixa salarial** e **senioridade** no mercado de dados brasileiro, observando tendências ao longo de quatro anos e projetando padrões para 2024 a partir de modelos treinados nos anos anteriores.

---

## Estrutura do repositório

```
stateofdata/
├── data/
│   ├── state_of_data_2021.csv
│   ├── state_of_data_2022.csv
│   ├── state_of_data_2023.csv
│   ├── state_of_data_2024.csv
│   ├── df_train.parquet          # 2021–2023, gerado pelo notebook
│   └── df_test.parquet           # 2024, gerado pelo notebook
├── figures/                      # Gráficos exportados (PNG)
├── state_of_data_genero.ipynb    # Notebook principal (único ponto de entrada)
├── df_full.xlsx                  # Dataset completo pós-processamento
├── resultados_estatisticos.xlsx  # Tabelas dos testes estatísticos
└── README.md
```

---

## Notebook principal

Todo o projeto está consolidado em **`state_of_data_genero.ipynb`**, organizado nas seguintes seções:

| Seção | Conteúdo |
|-------|----------|
| **0 — Configurações** | Imports, mapeamentos de colunas, constantes de ordenação e paleta de cores |
| **1 — Pré-processamento** | Pipeline de limpeza, padronização, unificação dos 4 anos e injeção do bloco de IA (2024) |
| **2 — EDA: Gênero** | 8 gráficos interativos (Plotly) — salário, senioridade, escolaridade, cargo, modalidade, área de formação, IA generativa |
| **3 — EDA: Regional** | 4 gráficos comparando representatividade feminina entre as 5 regiões brasileiras |
| **4 — Estatística Inferencial** | Qui-quadrado + V de Cramér, Mann-Whitney U, regressão logística simples (tendência temporal) e múltipla (preditores estruturais) |
| **5 — Machine Learning** | Modelo A (treino 2021–2023, predição 2024) e Modelo B (2024 com features de IA); comparação entre Regressão Logística, Random Forest e XGBoost |
| **6 — Explicabilidade (SHAP)** | SHAP values por target e por modelo, com destaque visual para features de IA |

---

## Variáveis-alvo

| Target | Tipo | Descrição |
|--------|------|-----------|
| `faixa_salarial` | Classificação multiclasse ordinal | 12 faixas de R$1k a R$40k+ |
| `nivel` | Classificação multiclasse ordinal | Júnior · Pleno · Sênior · Gestor |
| `gestor` | Classificação binária | Atua ou não como gestor/a |

> O State of Data coleta salário em **faixas**, não valor contínuo. Por isso, a abordagem de classificação ordinal é a mais adequada para esses dados.

---

## Metodologia estatística

### Testes utilizados

| Teste | Aplicação | Por quê |
|-------|-----------|---------|
| **Qui-quadrado + V de Cramér** | Associação entre gênero e variáveis categoriais | Detecta dependência; V de Cramér mede o tamanho do efeito |
| **Mann-Whitney U** | Diferença salarial entre gêneros | Não paramétrico; adequado para faixas ordinais e amostras desbalanceadas |
| **Regressão logística simples** | Tendência temporal da proporção feminina | Testa se a representatividade mudou significativamente entre 2021 e 2024 |
| **Regressão logística múltipla** | Preditores estruturais de ser mulher | Controla cargo, nível, salário, região e ano simultaneamente |

### Modelagem de ML

- **Seleção de modelo:** cross-validation estratificado (5-fold), métrica F1-macro
- **Análise de equidade (fairness):** desempenho reportado separadamente para homens e mulheres
- **Modelo A:** generalização temporal — verifica se padrões de 2021–2023 explicam 2024
- **Modelo B:** impacto das features de IA generativa (exclusivas de 2024) na predição

---

## Pré-requisitos

```bash
pip install pandas numpy plotly matplotlib seaborn scipy statsmodels \
            scikit-learn xgboost shap openpyxl pyarrow nbformat
```

Python 3.9 ou superior recomendado.

---

## Como executar

1. Clone o repositório e coloque os CSVs brutos em `data/`
2. Abra `state_of_data_genero.ipynb` no Jupyter Lab ou VS Code
3. Execute as seções em ordem — a Seção 1 gera os arquivos intermediários (`df_full.xlsx`, `df_train.parquet`, `df_test.parquet`) usados pelas seções seguintes

> **Atenção:** A Seção 1 deve ser executada antes de qualquer outra, pois as demais dependem dos arquivos que ela exporta.


