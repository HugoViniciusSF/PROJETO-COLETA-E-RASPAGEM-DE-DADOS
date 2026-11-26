# Projeto 3ª Unidade - Queimadas no Brasil e o Efeito do El Niño (2024 a 2026)

Repositório do projeto final da disciplina **IMD1152 - Coleta e Raspagem de Dados** (IMD / UFRN), ministrada pela Profa. Dra. Francimaria Nascimento.

## Problema investigado

O objetivo do projeto é analisar a evolução dos focos de queimadas no Brasil entre dezembro de 2025 e novembro de 2026 e relacioná-la ao avanço do El Niño em 2026, comparando esse período com o ciclo anterior (dezembro de 2024 a novembro de 2025). A análise observa como o número de focos, os dias sem chuva, o risco de fogo, a precipitação e a intensidade das queimadas (FRP) variam por mês, bioma e estado.

## Fontes e formatos dos dados

Todos os dados utilizados são públicos, gratuitos e não contêm informações pessoais ou sensíveis (em conformidade com princípios éticos e LGPD):

| Fonte | Conteúdo | Formato | Arquivo |
| :--- | :--- | :--- | :--- |
| **INPE (BDQueimadas)** | 858.405 focos no Brasil (`30/11/2024` a `30/11/2025`, satélite `NOAA-20`) | Semiestruturado (GeoJSON) | `dados/bdqueimadas_2024-11-30_2025-11-30.geojson` |
| **INPE (BDQueimadas)** | 780.217 focos no Brasil (`01/12/2025` a `18/11/2026`, satélite `NOAA-20`) | Estruturado (CSV) | `dados/bdqueimadas_2025-12-01_2026-11-19.csv` |
| **NOAA/CPC (RONI)** | Série histórica do índice RONI por trimestre móvel, para apoiar a análise do El Niño | TXT separado por espaços, convertido para CSV | `dados/RONI.ascii.txt` e `dados/roni.csv` |
| **MapBiomas (Monitor do Fogo)** | Área queimada mensal em hectares no Brasil, por estado e por bioma | API JSON, convertida para CSV | `dados/mapbiomas_respostas_api.json` e `dados/mapbiomas_area_queimada_{brasil,estados,biomas}.csv` |


## Estrutura do repositório

```text
.
├── dados/        # Bases do INPE, RONI em TXT/CSV e MapBiomas em JSON/CSV
├── notebooks/
│   ├── 01_leitura_dados_bdqueimadas.ipynb
│   ├── 01_coleta_leitura_roni.ipynb
│   ├── 01_coleta_leitura_mapbiomas.ipynb
│   └── 02_integracao_limpeza.ipynb
├── .gitattributes
├── .gitignore
├── requirements.txt
└── README.md
```

## Notebooks

| Notebook | Objetivo |
| :--- | :--- |
| [01_leitura_dados_bdqueimadas.ipynb](notebooks/01_leitura_dados_bdqueimadas.ipynb) | Leitura e exploração inicial dos arquivos GeoJSON e CSV do INPE |
| [01_coleta_leitura_roni.ipynb](notebooks/01_coleta_leitura_roni.ipynb) | Download do TXT da NOAA, conversão para CSV e exploração inicial do RONI |
| [01_coleta_leitura_mapbiomas.ipynb](notebooks/01_coleta_leitura_mapbiomas.ipynb) | Coleta de área queimada mensal para Brasil, 27 unidades federativas e seis biomas, com respostas originais em JSON e tabelas em CSV |
| [02_integracao_limpeza.ipynb](notebooks/02_integracao_limpeza.ipynb) | Integração e limpeza dos dois períodos do INPE, com exportação para `dados/queimadas_limpo.parquet` |


## Como clonar com os arquivos de dados (Git LFS)

Os arquivos brutos do INPE na pasta `dados/` são grandes (`310,8 MB` em GeoJSON e `73,8 MB` em CSV) e estão versionados com **Git LFS**. Antes de clonar o repositório, instale o Git LFS na sua máquina:

```bash
git lfs install
git clone https://github.com/HugoViniciusSF/PROJETO-COLETA-E-RASPAGEM-DE-DADOS.git
```

Se já tiver clonado antes de instalar o Git LFS e os arquivos em `dados/` aparecerem com apenas alguns bytes, execute dentro da pasta do repositório:

```bash
git lfs pull
```

## Integrantes

- ERIK SILVA DE MEDEIROS
- MARIANA TIMBÓ DE OLIVEIRA
- GABRIEL COSTA LIMA DANTAS
- HUGO VINICIUS DA SILVA FIGUEIREDO

