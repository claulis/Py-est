# Python para Estatística Descritiva e Análise de Dados

Python focado em estatística, análise de dados e visualização.

## Tecnologias Utilizadas

Ambiente:
[![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=plastic&logo=python&logoColor=white)](https://www.python.org/)
[![Google Colab](https://img.shields.io/badge/Google%20Colab-F9AB00?style=plastic&logo=googlecolab&logoColor=white)](https://colab.research.google.com/) 
[![VS Code](https://img.shields.io/badge/VS%20Code-007ACC?style=plastic&logo=visualstudiocode&logoColor=white)](https://code.visualstudio.com/)
[![Jupyter](https://img.shields.io/badge/Jupyter-F37726?style=plastic&logo=jupyter&logoColor=white)](https://jupyter.org/)
[![uv](https://img.shields.io/badge/uv-DE5FE8?style=plastic&logo=python&logoColor=white)](https://docs.astral.sh/uv/)

Bibliotecas:
[![Pandas](https://img.shields.io/badge/Pandas-150458?style=plastic&logo=pandas&logoColor=white)](https://pandas.pydata.org/)
[![NumPy](https://img.shields.io/badge/NumPy-013243?style=plastic&logo=numpy&logoColor=white)](https://numpy.org/)
[![Matplotlib](https://img.shields.io/badge/Matplotlib-11557c?style=plastic&logo=matplotlib&logoColor=white)](https://matplotlib.org/)
[![Seaborn](https://img.shields.io/badge/Seaborn-3498DB?style=plastic&logo=seaborn&logoColor=white)](https://seaborn.pydata.org/)





---

## Estrutura do Repositório

```
Py-est/
├── README.md
├── 01_trabalhando_com_dados/
│   └── fontes_de_dados.md          # Guia sobre importação e manipulação de dados de diversas fontes
├── 02_bibliotecas_estatisticas/
│   ├── estatistica_descritiva.md           # Teoria e referência sobre estatística descritiva
│   └── estatistica_descritiva_futebol.ipynb  # Notebook aplicado com dados de futebol
└── 03_bibliotecas_graficos/
    └── Graficos_Estatistica_Descritiva.ipynb  # Notebook com visualizações estatísticas
```

### [01_trabalhando_com_dados](/01_trabalhando_com_dados/)

Contém material introdutório sobre como importar, processar e manipular dados de diferentes origens: arquivos CSV e Excel, bancos de dados relacionais, APIs web e web scraping. Inclui também orientações sobre limpeza e pré-processamento de dados.

### [02_bibliotecas_estatisticas](/02_bibliotecas_estatisticas/)

Aborda as principais bibliotecas para análise estatística. O arquivo `.md` cobre medidas de tendência central, dispersão e distribuições de frequência. O notebook aplica esses conceitos em um dataset de futebol usando Pandas e SciPy.

### [03_bibliotecas_graficos](/03_bibliotecas_graficos/)

Contém notebook dedicado à visualização de dados estatísticos com Matplotlib e Seaborn: histogramas, boxplots, gráficos de dispersão, heatmaps e outros gráficos usados na análise exploratória.

---

## Como Começar

### Pre-requisitos

- Python 3.8 ou superior
- [uv](https://docs.astral.sh/uv/getting-started/) (gerenciador de pacotes moderno)

### Instalação

1. Clone o repositório:
```bash
git clone https://github.com/claulis/Py-est.git
cd Py-est
```

2. Instale o `uv` (se ainda não tiver):
```bash
# No macOS/Linux
curl -LsSf https://astral.sh/uv/install.sh | sh

# No Windows
powershell -ExecutionPolicy BypassUser -c "irm https://astral.sh/uv/install.ps1 | iex"

# Ou usando pip como fallback
pip install uv
```

3. Crie um ambiente virtual e instale as dependências:
```bash
uv sync
```

4. Ative o ambiente virtual:
```bash
source .venv/bin/activate  # No Windows: .venv\Scripts\activate
```

5. Inicie o Jupyter Notebook:
```bash
jupyter notebook
```

---

## Como Usar

Cada módulo contém notebooks interativos com explicações teóricas, exemplos de código, exercícios práticos e datasets para praticar.

Recomenda-se seguir a ordem:
1. Comece pelo módulo 1 (Fontes de Dados)
2. Prossiga para o módulo 2 (Bibliotecas Estatísticas)
3. Finalize com o módulo 3 (Bibliotecas de Gráficos)

## Contribuições

Contribuições são bem-vindas. Sinta-se a vontade para abrir issues com sugestoes, enviar pull requests com melhorias ou reportar erros.


