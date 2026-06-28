# Guia Teorico — Estatistica Descritiva com Python

> **Como usar este guia:** Leia cada secao antes de executar o bloco correspondente
> no notebook `estatistica_descritiva_futebol.ipynb`. Os conceitos aqui sao a base
> para entender o que o codigo faz e por que funciona.

---

## Sumario

1. [O que e Estatistica Descritiva?](#1-o-que-e-estatistica-descritiva)
2. [Tipos de Variaveis](#2-tipos-de-variaveis)
3. [As Bibliotecas](#3-as-bibliotecas)
4. [Medidas de Tendencia Central](#4-medidas-de-tendencia-central)
5. [Medidas de Dispersao](#5-medidas-de-dispersao)
6. [Distribuicao de Frequencias](#6-distribuicao-de-frequencias)
7. [Visualizacoes Estatisticas](#7-visualizacoes-estatisticas)
8. [Correlacao](#8-correlacao)
9. [Guia Rapido de Referencia](#9-guia-rapido-de-referencia)

---

## 1. O que e Estatistica Descritiva?

Imagine que voce recebe uma planilha com 92.000 jogadores de futebol. Cada linha e
um jogador, cada coluna e uma informacao: altura, pais, posicao, valor de mercado,
gols marcados. Como voce **resume** tudo isso de forma que faca sentido?

E exatamente para isso que existe a **Estatistica Descritiva**: um conjunto de
tecnicas para **organizar, resumir e apresentar dados** de maneira significativa.

> A estatistica descritiva **descreve o que ja aconteceu**. Ela nao faz previsoes —
> isso e papel da estatistica inferencial, que fica para outra disciplina.

### O que ela responde?

| Pergunta | Tecnica usada |
|----------|---------------|
| Qual e a altura tipica de um jogador? | Medidas de tendencia central |
| As alturas sao parecidas entre si ou muito diferentes? | Medidas de dispersao |
| Qual posicao tem mais jogadores? | Distribuicao de frequencias |
| Altura e posicao estao relacionadas? | Correlacao e visualizacao |
| Existem jogadores com altura fora do padrao? | Deteccao de outliers |

---

## 2. Tipos de Variaveis

Antes de calcular qualquer coisa, precisamos entender o **tipo** de cada variavel.
O tipo define quais tecnicas fazem sentido aplicar.

```
VARIAVEIS
|
+-- Quantitativas (numericas)
|   |
|   +-- Continuas: qualquer valor dentro de um intervalo
|   |   Exemplos: altura (1,84 m), valor de mercado (1.250.000,75 EUR)
|   |   Tecnicas: media, mediana, desvio padrao, histograma
|   |
|   +-- Discretas: valores inteiros e contaveis
|       Exemplos: gols marcados (3), numero de lesoes (2)
|       Tecnicas: media, mediana, moda, grafico de barras
|
+-- Qualitativas (categoricas)
    |
    +-- Nominais: categorias SEM ordem natural
    |   Exemplos: posicao (atacante, goleiro), pais (Brasil, Alemanha)
    |   Tecnicas: moda, frequencia absoluta/relativa, pizza, barras
    |
    +-- Ordinais: categorias COM ordem definida
        Exemplos: divisao do campeonato (1a, 2a, 3a), nivel de risco
        Tecnicas: mediana, percentis, frequencia
```

### Por que isso importa?

Tentar calcular a **media de posicoes** (atacante = 1, goleiro = 2, zagueiro = 3...)
nao faz nenhum sentido matematico. Posicao e uma variavel nominal — so podemos contar
quantos jogadores tem em cada posicao (frequencia) ou dizer qual e a mais comum (moda).

Da mesma forma, um **histograma** e ideal para altura (continua), mas um
**grafico de barras** e o correto para posicao (nominal).

---

## 3. As Bibliotecas 

Uma **biblioteca** (tambem chamada de pacote ou modulo) e uma colecao de funcoes
prontas que estendem o Python basico. Em vez de programar do zero uma funcao que
calcula a media de 90 mil numeros, importamos o Pandas e chamamos `.mean()`.

### 3.1 Pandas — O "Excel" do Python

**O que e:** A biblioteca mais importante para analise de dados em Python.
Trabalha com tabelas chamadas `DataFrame` — pense nelas como planilhas programaveis:
linhas sao observacoes (jogadores), colunas sao variaveis (altura, posicao, gols).

**Quando usar:** Sempre que precisar carregar, limpar, filtrar, agrupar ou
transformar dados em formato tabular.

**Principais operacoes:**

| O que fazer | Codigo | O que retorna |
|-------------|--------|---------------|
| Carregar CSV | `pd.read_csv('arquivo.csv')` | DataFrame com os dados |
| Ver primeiras linhas | `df.head(5)` | As 5 primeiras linhas |
| Resumo estatistico | `df.describe()` | Media, desvio padrao, quartis |
| Filtrar linhas | `df[df['gols'] > 10]` | So linhas onde gols > 10 |
| Agrupar | `df.groupby('posicao').mean()` | Media por posicao |
| Contar valores | `df['posicao'].value_counts()` | Contagem por categoria |
| Verificar nulos | `df.isnull().sum()` | Quantos NaN por coluna |
| Ordenar | `df.sort_values('valor', ascending=False)` | Do maior para o menor |
| Unir tabelas | `df1.merge(df2, on='player_id')` | Tabela combinada |

```python
import pandas as pd

# Criar um DataFrame simples
jogadores = pd.DataFrame({
    'nome':    ['Neymar',   'Mbappe',   'Vini Jr'],
    'gols':    [77,         45,         24],
    'posicao': ['atacante', 'atacante', 'atacante']
})

print(jogadores['gols'].mean())          # 48.67 — media de gols
print(jogadores['posicao'].value_counts())  # atacante    3
```

---

### 3.2 NumPy — Matematica de Alta Performance

**O que e:** Biblioteca para computacao numerica eficiente. Trabalha com
**arrays** (vetores e matrizes) e executa operacoes matematicas muito mais
rapido que listas Python comuns.

**Quando usar:** Operacoes matematicas sobre grandes conjuntos de numeros,
distribuicoes de probabilidade, algebra linear, e em qualquer calculo que
o Pandas faz internamente.

**Relacao com o Pandas:** O Pandas e construido em cima do NumPy. Quando
voce chama `df['altura'].mean()`, o Pandas usa NumPy por baixo dos panos.

**Principais operacoes:**

| O que fazer | Codigo | O que retorna |
|-------------|--------|---------------|
| Criar sequencia | `np.linspace(0, 1, 100)` | 100 pontos entre 0 e 1 |
| Numeros aleatorios | `np.random.normal(0, 1, 1000)` | 1000 nums com dist. normal |
| Raiz quadrada | `np.sqrt(array)` | Raiz de cada elemento |
| Logaritmo | `np.log(array)` | Log natural de cada elemento |
| Percentil | `np.percentile(array, 75)` | Terceiro quartil |
| Ajustar reta | `np.polyfit(x, y, 1)` | Coeficientes da linha de tendencia |

```python
import numpy as np

alturas = np.array([1.75, 1.82, 1.68, 1.90, 1.77])
print(np.mean(alturas))    # 1.784
print(np.std(alturas))     # 0.074
print(np.max(alturas))     # 1.90
```

---

### 3.3 Matplotlib — O Motor dos Graficos

**O que e:** A biblioteca base para criacao de graficos em Python. Oferece
controle total sobre cada elemento visual: eixos, titulos, cores, tamanhos,
fontes, anotacoes.

**Quando usar diretamente:**
- Graficos simples e rapidos (histograma, barras, dispersao)
- Quando precisar de controle total sobre o layout (posicao de textos, setas)
- Para criar dashboards com multiplos paineis (`plt.subplots`)

**Principais tipos de grafico:**

| Grafico | Funcao | Uso tipico |
|---------|--------|------------|
| Histograma | `plt.hist(x)` | Distribuicao de variavel numerica |
| Dispersao | `plt.scatter(x, y)` | Relacao entre dois numeros |
| Barras verticais | `plt.bar(cats, vals)` | Comparar categorias |
| Barras horizontais | `plt.barh(cats, vals)` | Categorias com nomes longos |
| Linha | `plt.plot(x, y)` | Evolucao ao longo do tempo |
| Boxplot | `plt.boxplot(x)` | Quartis e outliers |
| Multiplos paineis | `plt.subplots(2, 3)` | Dashboard com varios graficos |

```python
import matplotlib.pyplot as plt

gols = [3, 7, 2, 15, 4, 1, 9, 22, 6]

plt.hist(gols, bins=5, color='steelblue', edgecolor='white')
plt.xlabel('Gols')
plt.ylabel('Frequencia')
plt.title('Distribuicao de Gols')
plt.show()
```

**Conceitos essenciais:**

- `figure`: a "tela" onde o grafico existe
- `axes` (ax): o espaco dentro da figura onde os dados aparecem —
  uma figura pode ter varios axes (multiplos paineis)
- `plt.tight_layout()`: ajusta os espacamentos automaticamente
- `plt.savefig('arquivo.png', dpi=150)`: salva o grafico como imagem

---

### 3.4 Seaborn — Graficos Estatisticos com Menos Codigo

**O que e:** Biblioteca de visualizacao construida sobre o Matplotlib.
Produz graficos mais elaborados com menos codigo, especialmente para
analise exploratoria — graficos agrupados, heatmaps, distribuicoes com
curva de densidade.

**Quando preferir o Seaborn ao Matplotlib:**
- Boxplot agrupado por categoria (ex: altura por posicao)
- Mapa de calor de correlacao
- Quando o visual precisa ser mais sofisticado sem muitas linhas de codigo

**Comparacao pratica:**

Para criar um boxplot de altura agrupado por posicao:

```python
# Com Matplotlib puro: ~15 linhas de codigo
positions = df['posicao'].unique()
data = [df[df['posicao'] == p]['altura_m'].values for p in positions]
plt.boxplot(data, labels=positions)
# ... mais ajustes de ordenacao, cores, rotulos...

# Com Seaborn: 1 linha
sns.boxplot(data=df, x='posicao', y='altura_m', order=ordem, palette='Blues_d')
```

**Principais funcoes:**

| Grafico | Funcao | Uso tipico |
|---------|--------|------------|
| Boxplot agrupado | `sns.boxplot(data=df, x='cat', y='num')` | Distribuicao por grupo |
| Mapa de calor | `sns.heatmap(matriz, annot=True)` | Matriz de correlacao |
| Histograma + KDE | `sns.histplot(x, kde=True)` | Distribuicao suavizada |
| Dispersao | `sns.scatterplot(data=df, x='a', y='b')` | Dois numeros |
| Violin | `sns.violinplot(...)` | Como boxplot + forma da distribuicao |

```python
import seaborn as sns

sns.set_theme(style='whitegrid')  # tema visual global — define para toda a sessao

sns.boxplot(data=df, x='posicao', y='altura_m')
plt.show()
```

---

### 3.5 SciPy — Ciencia e Matematica Avancada

**O que e:** Biblioteca para computacao cientifica. Oferece funcoes avancadas
de estatistica, algebra linear, otimizacao e processamento de sinais — o que
vai alem do que o Pandas oferece.

**Quando usar:** Quando precisar de funcoes estatisticas que o Pandas nao tem.

**O que usamos no notebook:**

| Funcao | O que faz |
|--------|-----------|
| `stats.gaussian_kde(dados)` | Estima a curva de densidade KDE dos dados |
| `stats.pearsonr(x, y)` | Correlacao de Pearson com valor-p |
| `stats.normaltest(dados)` | Testa se os dados seguem distribuicao normal |
| `stats.ttest_ind(a, b)` | Testa se dois grupos tem medias diferentes |

```python
from scipy import stats

alturas = [1.75, 1.82, 1.68, 1.90, 1.77, 1.85, 1.72, 1.80]

# KDE: estima a forma da distribuicao sem assumir que e normal
kde = stats.gaussian_kde(alturas)

# Avaliar a densidade em 200 pontos entre o minimo e o maximo
import numpy as np
x = np.linspace(1.65, 1.95, 200)
densidade = kde(x)
# Cada valor de densidade[i] e a "altura" da curva em x[i]
```

**O que e KDE?** Imagine um histograma, mas no lugar de barras brutas, voce
suaviza o grafico com uma curva continua. O KDE (Kernel Density Estimation)
e essa curva — representa a "forma" da distribuicao sem as quebras artificiais
criadas pela escolha do numero de bins.

---

## 4. Medidas de Tendencia Central

Uma **medida de tendencia central** responde a pergunta: "Se eu tivesse que
escolher um unico numero para representar todos esses dados, qual seria?"

### 4.1 Media (Mean)

**O que e:** A soma de todos os valores dividida pela quantidade.

```
Media = (x1 + x2 + x3 + ... + xn) / n

Exemplo:
  Alturas: [1.75, 1.82, 1.68, 1.90, 1.77]
  Soma = 8.92
  n = 5
  Media = 8.92 / 5 = 1.784 m
```

**Quando usar:**
- Dados numericos sem valores extremos (outliers)
- Distribuicao relativamente simetrica
- Quando voce quer levar em conta TODOS os valores igualmente

**Quando NAO usar:**
- Salarios (alguns poucos muito altos distorcem para cima)
- Precos de imoveis
- Qualquer variavel com distribuicao muito assimetrica

**Exemplo critico:** Imagine 5 jogadores com salarios:
`R$ 5mil, R$ 5mil, R$ 5mil, R$ 5mil, R$ 980mil`

- Media = R$ 200 mil
- Nenhum dos 5 recebe perto disso!

**Codigo:**
```python
media = df['altura_m'].mean()
```

---

### 4.2 Mediana (Median)

**O que e:** O valor que fica exatamente no meio quando os dados estao ordenados.
Metade dos valores esta abaixo dela, metade acima.

```
Dados ordenados: [1.70,  1.75,  [1.80],  1.85,  1.90]
                                   ^
                                Mediana = 1.80
                        (valor central — 2 abaixo, 2 acima)

Para numero PAR de elementos:
  [1.70, 1.75, [1.80,  1.82], 1.85, 1.90]
               media dos dois = 1.81
```

**Quando usar:**
- Dados com outliers (valores muito extremos)
- Distribuicoes assimetricas: salarios, precos, valores de mercado
- Quando voce quer o "jogador do meio", nao o "jogador medio"

**Quando a mediana e melhor que a media:**

| Cenario | Media | Mediana | Qual usar? |
|---------|-------|---------|------------|
| Salarios com estrelas | Distorcida para cima | Representa o tipico | Mediana |
| Alturas dos jogadores | Funciona bem | Funciona bem | Qualquer uma |
| Dias de lesao | Distorcida por lesoes longas | Representa o tipico | Mediana |

**Codigo:**
```python
mediana = df['valor'].median()
```

---

### 4.3 Moda (Mode)

**O que e:** O valor que aparece com maior frequencia no conjunto.

```
Posicoes: [Atacante, Meia, Goleiro, Atacante, Zagueiro, Atacante, Meia]
Moda = Atacante  (aparece 3 vezes, mais do que qualquer outro)
```

**Quando usar:**
- Variaveis qualitativas (qual posicao e mais comum?)
- Variaveis discretas com poucos valores (numero de gols por jogo)
- Quando voce quer saber "o que acontece mais frequentemente"

**Quando NAO usar:**
- Variaveis continuas com muitos valores unicos (cada altura pode ser diferente)

**Codigo:**
```python
# mode() retorna uma Series mesmo com um unico valor
# [0] pega o primeiro (mais frequente)
moda = df['posicao'].mode()[0]
```

---

### 4.4 Resumo: Qual Medida Usar?

| Situacao | Medida recomendada |
|----------|--------------------|
| Alturas, temperaturas, notas de prova | Media |
| Salarios, precos, valores de mercado | Mediana |
| Posicao dos jogadores, pe dominante | Moda |
| Dados com outliers | Mediana |
| Distribuicao simetrica | Media |

**Regra pratica:** Compare media e mediana.
- Se sao **proximas**: a distribuicao e simetrica, use a media.
- Se a **media e bem maior**: ha valores altos extremos puxando para cima
  (distribuicao assimetrica a direita), prefira a mediana.
- Se a **mediana e bem maior**: ha valores baixos extremos puxando para baixo
  (assimetrica a esquerda).

---

## 5. Medidas de Dispersao

As medidas de tendencia central nos dizem **onde** os dados estao concentrados.
Mas duas turmas podem ter a mesma media de nota e serem completamente diferentes:
uma onde todos tiraram entre 6,0 e 7,0, outra com metade tirando 2,0 e metade 10,0.

As medidas de dispersao capturam esse **espalhamento**.

### 5.1 Amplitude (Range)

**O que e:** A diferenca entre o maior e o menor valor.

```
Amplitude = Maximo - Minimo

Alturas: [1.65, 1.70, 1.75, 1.80, 1.85, 1.90]
Amplitude = 1.90 - 1.65 = 0.25 m  (25 cm)
```

**Vantagem:** Simples e intuitiva.

**Desvantagem grave:** Um unico outlier distorce tudo.
Se um jogador medisse 2,20m por erro de digitacao, a amplitude seria 0,55m
sem refletir a real variacao dos outros jogadores.

**Codigo:**
```python
amplitude = df['altura_m'].max() - df['altura_m'].min()
```

---

### 5.2 Variancia (Variance)

**O que e:** A media dos quadrados dos desvios em relacao a media.
Mede o quao espalhados os dados estao em torno da media.

```
Para cada valor xi:
  Desvio = xi - media
  Desvio ao quadrado = (xi - media)^2

Variancia = soma de todos os (xi - media)^2  /  (n - 1)
```

**Por que elevar ao quadrado?**
1. Desvios positivos e negativos se cancelariam sem o quadrado
   (valores acima e abaixo da media se anulariam)
2. O quadrado penaliza valores muito afastados da media com mais intensidade

**Desvantagem:** A unidade fica ao quadrado (metros^2, gols^2), o que
dificulta a interpretacao pratica. Por isso, na maioria dos casos usamos
o desvio padrao (raiz quadrada da variancia).

**Codigo:**
```python
variancia = df['altura_m'].var()
```

---

### 5.3 Desvio Padrao (Standard Deviation)

**O que e:** A raiz quadrada da variancia. Fica na mesma unidade dos dados
originais (metros, gols, euros) — muito mais facil de interpretar.

```
Desvio Padrao = raiz_quadrada(Variancia)

Media de altura = 1.80m, Desvio Padrao = 0.065m:
  A maioria dos jogadores tem altura entre 1.735m e 1.865m
```

**Interpretacao:**
- Desvio padrao **pequeno**: dados concentrados perto da media (pouca variacao)
- Desvio padrao **grande**: dados espalhados (muita variacao)

**Regra 68-95-99,7 (para distribuicoes normais — em forma de sino):**

```
68% dos dados estao dentro de  1 desvio padrao da media
95% dos dados estao dentro de  2 desvios padrao da media
99,7% dos dados estao dentro de 3 desvios padrao da media
```

**Exemplo com altura** (media=1.80m, dp=0.065m):
```
1 dp: entre 1.735m e 1.865m  ->  68% dos jogadores
2 dp: entre 1.670m e 1.930m  ->  95% dos jogadores
3 dp: entre 1.605m e 1.995m  ->  99,7% dos jogadores
```

**Codigo:**
```python
desvio = df['altura_m'].std()
```

---

### 5.4 Quartis e IQR (Intervalo Interquartil)

**Quartis** dividem os dados ordenados em quatro partes iguais de 25% cada:

```
0%      25%      50%      75%     100%
|--------|--------|--------|--------|
        Q1      Q2/Med   Q3
                (Mediana)

Q1 (primeiro quartil):   25% dos dados estao abaixo
Q2 (segundo quartil):    50% dos dados estao abaixo = Mediana
Q3 (terceiro quartil):   75% dos dados estao abaixo
```

**IQR (Intervalo Interquartil):**

```
IQR = Q3 - Q1
```

O IQR representa os **50% do meio** dos dados — os 25% menores e os 25%
maiores sao excluidos. Por isso, o IQR e **resistente a outliers**: mesmo
que haja jogadores com alturas absurdas, o IQR so olha para o meio.

**Exemplo:** Para alturas com Q1=1,75m e Q3=1,85m:
```
IQR = 1,85 - 1,75 = 0,10m  (10 cm)
A "caixa central" dos jogadores tem 10cm de variacao
```

**Codigo:**
```python
q1  = df['altura_m'].quantile(0.25)
q3  = df['altura_m'].quantile(0.75)
iqr = q3 - q1
```

---

### 5.5 Deteccao de Outliers — Regra 1,5 x IQR

Um **outlier** e um valor que esta anormalmente distante dos demais.
Pode ser erro de digitacao, caso genuinamente excepcional, ou problema
no dataset.

**Como identificar usando o IQR:**

```
Limite inferior = Q1 - 1,5 x IQR
Limite superior = Q3 + 1,5 x IQR

Valores fora desses limites sao considerados outliers
```

**Exemplo com alturas** (Q1=1,75m, Q3=1,85m, IQR=0,10m):

```
Limite inferior = 1,75 - 1,5 x 0,10 = 1,75 - 0,15 = 1,60m
Limite superior = 1,85 + 1,5 x 0,10 = 1,85 + 0,15 = 2,00m

Jogadores com menos de 1,60m ou mais de 2,00m sao outliers
```

**O que fazer com outliers?**

| Acao | Quando aplicar |
|------|----------------|
| Investigar | Sempre — entender por que o valor e extremo |
| Remover | Se for claramente erro de digitacao (ex: altura de 18,4m) |
| Manter | Se for real e parte importante do fenomeno |
| Separar | Analisar o conjunto com e sem outliers para comparar |

**Codigo:**
```python
limite_inf = q1 - 1.5 * iqr
limite_sup = q3 + 1.5 * iqr
outliers = df[(df['altura_m'] < limite_inf) | (df['altura_m'] > limite_sup)]
```

---

### 5.6 O metodo describe() — Todas as Medidas de Uma Vez

O Pandas oferece um atalho que calcula as principais medidas descritivas
de uma variavel so:

```python
df['altura_m'].describe()
```

```
count    92671.000   <-- quantos valores validos (sem NaN)
mean         1.812   <-- media
std          0.065   <-- desvio padrao
min          1.550   <-- menor valor
25%          1.770   <-- Q1 (primeiro quartil)
50%          1.810   <-- Q2 = mediana
75%          1.860   <-- Q3 (terceiro quartil)
max          2.100   <-- maior valor
```

---

## 6. Distribuicao de Frequencias

Uma **distribuicao de frequencias** mostra quantas vezes cada valor (ou faixa
de valores) aparece nos dados. E a forma mais basica de entender o que um
conjunto de dados contem.

### 6.1 Frequencia Absoluta

Conta diretamente o numero de ocorrencias de cada valor.

**Exemplo:** Quantos jogadores ha em cada posicao?

```
Centre-Back (Zagueiro):    15.230 jogadores
Central Midfield (Meia):   12.870 jogadores
Centre-Forward (Atacante):  9.540 jogadores
```

**Codigo:**
```python
freq_abs = df['posicao'].value_counts()
# Retorna contagem ordenada do mais frequente para o menos
```

---

### 6.2 Frequencia Relativa

Mostra a proporcao (percentual) de cada valor em relacao ao total.

```
Zagueiro: 15.230 / 92.671 = 16,4%
```

**Quando usar frequencia relativa em vez de absoluta:**
- Para comparar grupos de tamanhos diferentes
- Quando o percentual comunica melhor que o numero bruto
- Para relatar resultados para um publico geral

**Codigo:**
```python
freq_rel = df['posicao'].value_counts(normalize=True) * 100
# normalize=True retorna proporcoes entre 0 e 1
# multiplicamos por 100 para exibir como percentual
```

---

### 6.3 Agrupamento com groupby

O `groupby` e uma das operacoes mais poderosas do Pandas. Permite calcular
estatisticas separadamente para cada grupo.

**Analogia com Excel:** e como usar Tabela Dinamica — voce define a coluna
de agrupamento e qual calculo fazer nos outros campos.

```python
# Media de gols por posicao
media_por_posicao = df.groupby('posicao')['gols'].mean()

# Multiplas estatisticas de uma vez
resumo = df.groupby('posicao')['gols'].agg(['mean', 'median', 'max', 'count'])
```

**Operacoes mais comuns apos groupby:**

| Funcao | O que calcula |
|--------|---------------|
| `.mean()` | Media do grupo |
| `.median()` | Mediana do grupo |
| `.sum()` | Soma total do grupo |
| `.count()` | Numero de linhas no grupo |
| `.max()` / `.min()` | Maior / menor valor |
| `.size()` | Numero de linhas (incluindo NaN) |
| `.agg(['mean','std'])` | Multiplas estatisticas de uma vez |

---

## 7. Visualizacoes Estatisticas

Um grafico bem feito comunica em segundos o que uma tabela levaria minutos
para transmitir. Cada tipo de grafico responde a uma pergunta especifica.

### 7.1 Histograma

**O que mostra:** Como os valores de uma variavel numerica se distribuem.
Divide o intervalo de valores em faixas (**bins**) e mostra quantas
observacoes caem em cada faixa.

**Quando usar:** Variavel **numerica continua** — altura, valor de mercado,
tempo de lesao, minutos jogados.

**Como ler:**

```
Frequencia
   |
 5 |    ##
 4 |   ####
 3 |  ######
 2 | ##########
 1 |############
   |_______________
   1.65  1.75  1.85  1.95   Altura (m)
         ^
      A maioria dos jogadores tem entre 1.75 e 1.85
```

**O que procurar:**
- **Forma:** simetrica (sino), assimetrica a direita (cauda longa a direita),
  assimetrica a esquerda, bimodal (dois picos)
- **Pico:** onde esta a concentracao dos dados
- **Caudas:** ha valores extremos?

**Codigo:**
```python
plt.hist(df['altura_m'], bins=40, color='steelblue', edgecolor='white')
# bins=40: quantas faixas dividem o intervalo
# density=True: normaliza para que a area total seja 1 (probabilidade)
```

**Curva KDE (Kernel Density Estimation):**
Uma versao suavizada do histograma que estima a forma da distribuicao
sem as "bordas" artificiais criadas pela escolha do numero de bins.
E util para ver se a distribuicao tem formato de sino (normal) ou
outro padrao.

```python
from scipy import stats
kde = stats.gaussian_kde(df['altura_m'].dropna())
x = np.linspace(xmin, xmax, 200)
plt.plot(x, kde(x), 'r-', linewidth=2)
```

---

### 7.2 Boxplot (Diagrama de Caixa)

**O que mostra:** A distribuicao de uma variavel numerica usando os quartis.
Resume em um so grafico: mediana, IQR, amplitude e outliers.

**Anatomia do boxplot:**

```
                |
        ________|________
        |               |     <- Caixa = IQR (de Q1 a Q3)
Bigode--|--[Q1----Med----Q3]--|--Bigode
        |_______________|
                |
               
        *   *              *  <- Outliers (pontos alem dos bigodes)

Bigodes: valor mais extremo que AINDA esta dentro de 1.5 x IQR
```

**Quando usar:**
- Para comparar distribuicoes entre **varios grupos** (posicoes, paises)
- Para identificar outliers visualmente
- Quando voce quer ver Q1, Q2 e Q3 de forma grafica

**Codigo (Seaborn para agrupamento):**
```python
# Seaborn cria boxplots agrupados por categoria em uma linha
sns.boxplot(data=df, x='posicao', y='altura_m', order=ordem, palette='Blues_d')
```

**Vantagem sobre o histograma:**
O histograma mostra um grupo de cada vez.
O boxplot permite comparar **muitos grupos lado a lado** de forma compacta.

---

### 7.3 Grafico de Barras

**O que mostra:** O valor de uma variavel numerica para diferentes categorias.

**Quando usar:** Comparar quantidades entre **variaveis qualitativas**
(paises, posicoes, clubes, temporadas).

**Barras verticais vs horizontais:**

| Use vertical quando... | Use horizontal quando... |
|------------------------|--------------------------|
| Os rotulos sao curtos (2-3 letras) | Os rotulos sao longos (nomes de paises) |
| Ha poucas categorias (< 8) | Ha muitas categorias |
| Quer enfatizar altura | Quer facilitar leitura dos nomes |

**Codigo:**
```python
# Barras horizontais — melhor para nomes de paises
bars = ax.barh(paises[::-1], contagens[::-1], color=cores)

# Adicionar valor no final de cada barra
for bar, val in zip(bars, contagens[::-1]):
    ax.text(bar.get_width() + 50,
            bar.get_y() + bar.get_height()/2,
            f'{val:,}', va='center')
```

**Erro comum:** Usar grafico de barras para mostrar distribuicao de uma
variavel numerica continua (altura, renda). Para isso, use **histograma**.
Barras sao para **categorias**, histograma e para **numeros continuos**.

---

### 7.4 Grafico de Dispersao (Scatter Plot)

**O que mostra:** A relacao entre **duas variaveis numericas**. Cada ponto
representa uma observacao (um jogador), com sua posicao no eixo X e Y
definida pelos valores das duas variaveis.

**Quando usar:** Para investigar se duas variaveis numericas estao relacionadas.
Ex: gols vs. assistencias, altura vs. valor de mercado.

**Como ler:**

```
Assistencias
     |             .  .
  10 |          .    .
   8 |       .  .
   6 |     .  .
   4 |   . .
   2 | . .
     |____________
     0  2  4  6  8  10    Gols

Pontos alinhados em diagonal ascendente = correlacao positiva
(quem marca mais gols tende a dar mais assistencias)
```

**Codigo:**
```python
scatter = plt.scatter(
    df['gols'], df['assists'],
    alpha=0.4,    # transparencia — ajuda quando pontos se sobrepoem
    s=20,         # tamanho dos pontos
    c=df['gols'] + df['assists'],  # cor pelo total ofensivo
    cmap='YlOrRd'
)
plt.colorbar(scatter, label='Gols + Assistencias')
```

**Linha de tendencia:**
```python
z = np.polyfit(x, y, 1)  # coeficientes da reta (grau 1 = reta)
p = np.poly1d(z)          # transforma em funcao avaliavel
x_line = np.linspace(x.min(), x.max(), 100)
plt.plot(x_line, p(x_line), 'b--', linewidth=2)
```

---

## 8. Correlacao

**Correlacao** mede o grau e a direcao da relacao linear entre duas
variaveis numericas: quando uma sobe, a outra tende a subir tambem (positiva),
a cair (negativa), ou nao ha padrao (zero)?

### 8.1 Coeficiente de Pearson (r)

O **coeficiente de correlacao de Pearson** e um numero entre -1 e +1:

```
r = +1,0  -> correlacao positiva perfeita
              quando X sobe, Y sobe proporcionalmente

r = +0,7  -> correlacao positiva forte

r = +0,3  -> correlacao positiva fraca

r =  0,0  -> sem correlacao linear

r = -0,3  -> correlacao negativa fraca

r = -0,7  -> correlacao negativa forte

r = -1,0  -> correlacao negativa perfeita
              quando X sobe, Y cai proporcionalmente
```

**Escala para interpretar o valor de |r|:**

| Valor de |r| | Interpretacao |
|-----------|--------------|
| 0,90 a 1,00 | Muito forte |
| 0,70 a 0,89 | Forte |
| 0,50 a 0,69 | Moderada |
| 0,30 a 0,49 | Fraca |
| 0,00 a 0,29 | Muito fraca ou ausente |

**Codigo:**
```python
# Correlacao entre duas colunas
r = df['gols'].corr(df['assists'])

# Matriz de correlacao entre TODAS as colunas numericas
matriz_corr = df.corr()
```

---

### 8.2 Matriz de Correlacao

Quando temos varias variaveis numericas, calculamos a correlacao entre
todos os pares — o resultado e uma **matriz simetrica**.

```
               gols  assists  yellow_cards  minutos
gols          1.000    0.420         0.120    0.350
assists       0.420    1.000         0.080    0.290
yellow_cards  0.120    0.080         1.000    0.110
minutos       0.350    0.290         0.110    1.000
```

- A diagonal principal e sempre 1,000 (toda variavel tem correlacao perfeita
  consigo mesma)
- A matriz e simetrica: o valor acima da diagonal e igual ao abaixo
  (`corr(gols, assists) = corr(assists, gols)`)

---

### 8.3 Heatmap — Visualizando a Matriz

O **heatmap** transforma a matriz de numeros em um grafico de cores,
muito mais intuitivo:

```
               gols  assists  yellow  minutos
gols          [  1  ][ 0.42 ][ 0.12 ][ 0.35 ]
assists       [ 0.42][  1   ][ 0.08 ][ 0.29 ]
yellow_cards  [ 0.12][ 0.08 ][  1   ][ 0.11 ]
minutos       [ 0.35][ 0.29 ][ 0.11 ][  1   ]

Vermelho intenso = correlacao positiva alta
Branco = sem correlacao
Azul intenso = correlacao negativa alta
```

**Codigo:**
```python
mask = np.triu(np.ones_like(matriz_corr, dtype=bool))
# mask oculta o triangulo SUPERIOR (que e identico ao inferior)
# assim vemos cada par so uma vez

sns.heatmap(
    matriz_corr,
    mask=mask,
    annot=True,     # exibe os numeros dentro de cada celula
    fmt='.2f',      # 2 casas decimais
    cmap='RdBu_r',  # vermelho-branco-azul
    center=0,       # zero fica na cor branca (neutra)
    vmin=-1, vmax=1 # escala fixa entre -1 e +1
)
```

---

### 8.4 ATENCAO: Correlacao nao e Causalidade

Este e o erro mais importante a evitar em estatistica:

> **Correlacao indica que duas variaveis se movem juntas.
> NAO significa que uma CAUSA a outra.**

**Exemplo classico:** O numero de sorvetes vendidos e fortemente correlacionado
com o numero de afogamentos em praias. Ninguem concluiria que comer sorvete
causa afogamento — ambos aumentam no verao (terceira variavel oculta).

**No futebol:**
- Jogadores com valor de mercado alto tendem a marcar mais gols: *correlacao*
- Mas pagar mais caro por um jogador nao garante que ele marcara mais gols
- Pode ser que times mais ricos ja tenham jogadores melhores por outros motivos

**Para estabelecer causalidade, voce precisaria de:**
1. Hipotese logica que explique o mecanismo de causa e efeito
2. Controle de outras variaveis (que podem explicar a relacao)
3. Tecnicas mais avancadas (regressao, experimentos controlados)

---

## 9. Guia Rapido de Referencia

### Qual medida de resumo usar?

```
Tenho uma variavel numerica e quero saber...

+-- Onde os dados estao concentrados?
|   +-- Distribuicao simetrica, sem outliers   --> Media
|   +-- Distribuicao assimetrica ou com outliers --> Mediana
|   +-- Variavel qualitativa ou discreta        --> Moda
|
+-- O quanto os dados variam?
|   +-- Visao geral na mesma unidade            --> Desvio Padrao
|   +-- Resistente a outliers                   --> IQR (Q3 - Q1)
|   +-- Valores extremos                        --> Amplitude
|
+-- Como os dados se distribuem?
    +-- Forma e frequencia                      --> Histograma + describe()
```

### Qual grafico usar?

```
Quero visualizar...

+-- Distribuicao de 1 variavel numerica
|   +-- Forma, picos, caudas                    --> Histograma (+ curva KDE)
|   +-- Quartis e outliers                      --> Boxplot
|
+-- Comparar grupos
|   +-- Distribuicao numerica por categoria     --> Boxplot agrupado (Seaborn)
|   +-- Contagem ou total por categoria         --> Grafico de barras
|   +-- Proporcao de cada categoria             --> Grafico de pizza (pizza)
|
+-- Relacao entre 2 variaveis numericas
|   +-- Ver se ha correlacao                    --> Scatter plot
|   +-- Tendencia geral                         --> Scatter + linha de regressao
|
+-- Correlacoes entre muitas variaveis
    +-- Ver todos os pares de uma vez           --> Heatmap da matriz de correlacao
```

### Biblioteca certa para cada tarefa

| Tarefa | Biblioteca |
|--------|------------|
| Carregar e manipular dados | Pandas |
| Calculos matematicos, aleatorios | NumPy |
| Graficos simples ou controle total | Matplotlib |
| Graficos agrupados, heatmap | Seaborn |
| KDE, testes estatisticos | SciPy |

---


