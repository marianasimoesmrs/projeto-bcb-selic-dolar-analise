Análise comparativa SELIC x Dólar (Banco Central do Brasil)


Este projeto faz uma análise simples e didática da relação entre a taxa SELIC e o dólar comercial (PTAX) ao longo do tempo, usando dados públicos do Banco Central do Brasil (BCB).

O foco não é prever nada, e sim mostrar o fluxo de engenharia de dados (ingestão, camadas de dados e análise) usando Python e Jupyter Notebook.


Objetivos do projeto

- Consumir dados públicos do BCB (SELIC diária e dólar PTAX diário).
- Organizar os dados em camadas de um mini-lakehouse: silver e gold.
- Calcular médias mensais da SELIC e do dólar.
- Comparar a evolução relativa das duas séries ao longo do tempo, usando um índice base 100.
- Gerar um gráfico final que mostre como SELIC e dólar variaram no período.


Fontes de dados

Os dados vêm das APIs públicas do Banco Central:

- SELIC diária
  - Série histórica da taxa SELIC diária.
  - No projeto, é lida a partir de um arquivo tratado em `data/silver/selic_diaria.csv`.

- Dólar comercial (PTAX)
  - Cotações diárias de compra e venda do dólar comercial.
  - No projeto, é lido a partir de `data/silver/dolar_diario.csv`.

> Em projetos anteriores, esses arquivos silver foram gerados diretamente a partir das APIs do BCB.  
> Aqui objetivo foi integrar as duas fontes e construir uma camada gold para análise.



Arquitetura de dados (mini-lakehouse)

O projeto segue a ideia de camadas silver e gold:


Camada Silver (`data/silver/`)
Dados já limpos e padronizados, prontos para análise:

- `selic_diaria.csv`  
  - Colunas principais:
    - `data` (datetime)
    - `valor` (taxa SELIC diária)
    - `ano` (inteiro)
    - `mes` (inteiro)

- `dolar_diario.csv`  
  - Colunas principais:
    - `data` (datetime)
    - `cotacaoCompra`
    - `cotacaoVenda`


Camada Gold (`data/gold/`)
Tabelas derivadas, já agregadas e prontas para consumo por análise / visualização:

- `indicadores_selic_dolar_mensal.csv`
  - Colunas:
    - `data_referencia` → primeiro dia do mês (datetime)
    - `media_selic` → média mensal da SELIC
    - `media_dolar` → média mensal do dólar (cotação de venda)
    - `diferenca_selic_dolar` → `media_selic - media_dolar`
    - `relacao_selic_dolar` → `media_selic / media_dolar`
    - `selic_idx` → índice base 100 da SELIC
    - `dolar_idx` → índice base 100 do dólar



Stack utilizada:

- Linguagem: Python (via Anaconda)
- Ambiente: Jupyter Notebook
- Principais bibliotecas:
  - `pandas` → manipulação de dados tabulares
  - `requests` → chamadas HTTP para as APIs do BCB (nos projetos de ingestão)
  - `matplotlib` → geração dos gráficos



Estrutura do repositório:

projeto-bcb-selic-dolar-analise/
│
├── data/
│   ├── silver/
│   │   ├── selic_diaria.csv
│   │   └── dolar_diario.csv
│   └── gold/
│       └── indicadores_selic_dolar_mensal.csv
│
├── notebooks/
│   └── 01_ingestao_selic_dolar.ipynb
│
└── README.md
