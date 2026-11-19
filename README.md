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

## Estrutura do repositório

```text
.
├── dados/        # Bases do INPE em GeoJSON e CSV (versionadas com Git LFS)
├── notebooks/    # Notebooks (.ipynb) de leitura, limpeza, integração, preparo e análise
├── .gitattributes
├── .gitignore
└── README.md
```

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

