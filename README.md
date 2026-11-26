# Projeto 3ª Unidade - Queimadas no Brasil e o Efeito do El Niño (2024 a 2026)

Repositório do projeto final da disciplina IMD1152 - Coleta e Raspagem de Dados (IMD / UFRN), ministrada pela Profa. Dra. Francimaria Nascimento.

## Problema investigado

Analisamos a evolução dos focos de queimadas e da área queimada no Brasil entre dezembro de 2025 e novembro de 2026 em relação ao avanço do El Niño em 2026, comparando esse período com o ciclo anterior (dezembro de 2024 a novembro de 2025). A análise cruza contagem de focos, dias sem chuva, risco de fogo, precipitação e potência radiativa do fogo (FRP) do INPE com o índice climático RONI da NOAA e a área queimada mensal em hectares do MapBiomas por bioma e estado.

## Fontes, métodos de coleta e formatos

Todos os dados utilizados são públicos, gratuitos e não contêm informações pessoais:

| Fonte | Conteúdo | Método / Formato | Arquivos em `dados/` |
| :--- | :--- | :--- | :--- |
| INPE (BDQueimadas) | 858.405 focos no Brasil (`30/11/2024` a `30/11/2025`, satélite `NOAA-20`) | Leitura de arquivo semiestruturado (GeoJSON) | `dados/bdqueimadas_2024-11-30_2025-11-30.geojson` |
| INPE (BDQueimadas) | 780.217 focos no Brasil (`01/12/2025` a `18/11/2026`, satélite `NOAA-20`) | Leitura de arquivo estruturado (CSV) | `dados/bdqueimadas_2025-12-01_2026-11-19.csv` |
| NOAA/CPC (RONI) | Série histórica do índice RONI por trimestre móvel (`1950` até `ASO 2026`) | Download de TXT da web e conversão para CSV | `dados/RONI.ascii.txt` e `dados/roni.csv` |
| MapBiomas (Monitor do Fogo) | Área queimada mensal em hectares no Brasil, 6 biomas e 27 UFs (`2024-12` a `2026-10`) | Consumo de API REST (JSON) convertido para CSV | `dados/mapbiomas_respostas_api.json` e `dados/mapbiomas_area_queimada_{brasil,biomas,estados}.csv` |

## Estrutura do repositório

```text
.
├── dados/
│   ├── bdqueimadas_2024-11-30_2025-11-30.geojson
│   ├── bdqueimadas_2025-12-01_2026-11-19.csv
│   ├── RONI.ascii.txt
│   ├── roni.csv
│   ├── mapbiomas_respostas_api.json
│   ├── mapbiomas_area_queimada_{brasil,biomas,estados}.csv
│   ├── mapbiomas_cobertura_mensal.csv
│   ├── queimadas_limpo.parquet
│   ├── queimadas_rotulado.parquet
│   └── integrado_mensal_{brasil,biomas,estados}.csv
├── notebooks/
│   ├── 01_leitura_dados_bdqueimadas.ipynb
│   ├── 01_coleta_leitura_roni.ipynb
│   ├── 01_coleta_leitura_mapbiomas.ipynb
│   ├── 02_integracao_limpeza.ipynb
│   ├── 03_rotulagem.ipynb
│   └── 04_analise_exploratoria.ipynb
├── .gitattributes
├── .gitignore
├── requirements.txt
└── README.md
```

## Pipeline dos notebooks

| Notebook | Objetivo |
| :--- | :--- |
| [01_leitura_dados_bdqueimadas.ipynb](notebooks/01_leitura_dados_bdqueimadas.ipynb) | Leitura e conferência estrutural dos arquivos GeoJSON e CSV do INPE |
| [01_coleta_leitura_roni.ipynb](notebooks/01_coleta_leitura_roni.ipynb) | Download do TXT da NOAA e conversão para `dados/roni.csv` |
| [01_coleta_leitura_mapbiomas.ipynb](notebooks/01_coleta_leitura_mapbiomas.ipynb) | Coleta via API REST da área queimada mensal para Brasil, 27 UFs e 6 biomas |
| [02_integracao_limpeza.ipynb](notebooks/02_integracao_limpeza.ipynb) | União dos dois períodos do INPE, limpeza, junção com o RONI (`queimadas_limpo.parquet`) e criação de `integrado_mensal_{brasil,biomas,estados}.csv` |
| [03_rotulagem.ipynb](notebooks/03_rotulagem.ipynb) | Rotulagem baseada em regras (`ciclo`, `fase_enso`, `foco_critico`) em `queimadas_rotulado.parquet` e atualização de `ciclo` e `fase_enso` nas tabelas mensais |
| [04_analise_exploratoria.ipynb](notebooks/04_analise_exploratoria.ipynb) | Leitura de `integrado_mensal_{brasil,biomas,estados}.csv`, gráficos comparativos entre os dois ciclos e matrizes de correlação |

## Dicionário resumido dos dados preparados

### Base de focos (`dados/queimadas_rotulado.parquet`, 1.633.615 registros)
| Coluna | Tipo | Origem | Descrição |
| :--- | :--- | :--- | :--- |
| `DataHora` | datetime | INPE | Data e hora UTC da detecção do foco |
| `Satelite`, `Pais` | texto | INPE | Constantes (`NOAA-20` e `Brasil`) |
| `Estado`, `Municipio`, `Bioma` | texto | INPE | Localização administrativa (UF em maiúsculas) e bioma |
| `DiaSemChuva`, `Precipitacao`, `RiscoFogo` | float | INPE | Variáveis meteorológicas do foco (`-999` e nulos imputados pela mediana) |
| `FRP` | float | INPE | Potência radiativa do fogo em MW |
| `Latitude`, `Longitude` | float | INPE | Coordenadas geográficas em graus decimais |
| `origem_formato` | texto | Derivado | Formato de origem (`GeoJSON (2024-2025)` ou `CSV (2025-2026)`) |
| `ano`, `mes`, `ano_mes` | int / texto | Derivado | Chaves temporais extraídas de `DataHora` |
| `trimestre`, `roni` | texto / float | NOAA | Trimestre móvel e anomalia RONI associada pelo mês central |
| `ciclo` | texto | Rotulagem | `2024-2025 (Sem El Niño)` ou `2025-2026 (El Niño)` |
| `fase_enso` | texto | Rotulagem | `La Niña` (`<= -0.5`), `Neutro`, `El Niño` (`>= +0.5`) ou `Sem dado RONI` |
| `foco_critico` | int (0/1) | Rotulagem | `1` quando `RiscoFogo >= 0.8` e `DiaSemChuva >= 15` |

### Tabelas mensais integradas (`dados/integrado_mensal_{brasil,biomas,estados}.csv`)
Reúnem por `ano_mes` (e `Bioma` ou `Estado`): `total_focos`, `media_dias_sem_chuva`, `media_precipitacao`, `media_risco_fogo`, `media_frp` e `soma_frp` (INPE), `trimestre`, `roni`, `ciclo` e `fase_enso` (NOAA), além de `area_queimada_ha` (MapBiomas).

## Como clonar e executar localmente

Os arquivos grandes em `dados/` (`*.geojson`, `bdqueimadas_*.csv` e `*.parquet`) estão versionados com Git LFS:

```bash
git lfs install
git clone https://github.com/HugoViniciusSF/PROJETO-COLETA-E-RASPAGEM-DE-DADOS.git
cd PROJETO-COLETA-E-RASPAGEM-DE-DADOS
pip install -r requirements.txt
```

Se já tiver clonado sem os objetos do LFS, execute `git lfs pull` na raiz do repositório antes de abrir os notebooks em `notebooks/`.

## Integrantes

- ERIK SILVA DE MEDEIROS
- MARIANA TIMBÓ DE OLIVEIRA
- GABRIEL COSTA LIMA DANTAS
- HUGO VINICIUS DA SILVA FIGUEIREDO
