# Trabalhando com Várias Fontes de Dados e Arquivos



## 1. Por que existem tantos formatos de arquivo?

Antes de começar a programar, é importante entender que cada formato de arquivo existe para resolver um problema diferente:

- **TXT**: o formato mais simples possível, apenas texto puro, sem estrutura. Bom para logs, anotações e textos corridos.
- **CSV**: texto organizado em linhas e colunas, separado por vírgulas (ou outro caractere). Ideal para tabelas simples, muito usado para troca de dados entre sistemas diferentes.
- **JSON**: formato que organiza dados em estruturas aninhadas (como dicionários e listas dentro de outros dicionários). É o padrão usado pela maioria das APIs da internet.
- **Excel (.xlsx)**: formato de planilha, com múltiplas abas, fórmulas e formatação visual. Muito usado em ambientes corporativos.
- **XML**: formato que organiza dados em tags hierárquicas, usado em configurações, trocas de dados estruturadas e documentos.
- **PDF**: formato de documento portável, preserva formatação e layout visual. Ideal para relatórios e documentos finalizados.
- **DOCX**: formato de documento do Microsoft Word, permite texto formatado, tabelas e imagens. Usado para documentos editáveis.
- **Banco de dados (SQLite)**: arquivo que armazena dados organizados em tabelas relacionadas, permitindo consultas complexas (filtros, junções, agregações) com a linguagem SQL.
- **APIs**: não são arquivos, mas serviços na internet que retornam dados (geralmente em JSON) quando fazemos uma requisição.

Saber escolher o formato certo é tão importante quanto saber programar a leitura e escrita dele.

---

## 2. Arquivos de texto simples (.txt)

### Explicação

Para trabalhar com arquivos no Python usamos a função embutida `open()`. Ela recebe o caminho do arquivo e um **modo de abertura**:

| Modo | Significado |
|------|-------------|
| `'w'` | Escrita: cria o arquivo do zero (apaga o que existia) |
| `'r'` | Leitura: o arquivo precisa existir |
| `'a'` | Append: adiciona conteúdo ao final do arquivo, sem apagar o que já existe |
| `'r+'` | Leitura e escrita ao mesmo tempo |

É uma boa prática **sempre** usar o arquivo dentro de um bloco `with`. Esse bloco garante que o arquivo será fechado automaticamente, mesmo que ocorra um erro no meio do caminho. Sem o `with`, você pode deixar arquivos abertos sem querer.

### Exemplo: escrevendo em um arquivo

```python
# Abrindo o arquivo em modo escrita ('w')
# encoding='utf-8' garante que acentos e caracteres especiais funcionem corretamente
with open('exemplo.txt', 'w', encoding='utf-8') as f:
    f.write('Primeira linha\n')   # \n significa "quebra de linha"
    f.write('Segunda linha\n')
    f.write('Terceira linha\n')

print('Arquivo criado com sucesso!')
```

**O que aconteceu:** criamos o arquivo `exemplo.txt` na pasta atual do Colab e escrevemos três linhas. O `\n` no final de cada string é o que separa as linhas — sem ele, tudo ficaria em uma única linha.

### Exemplo: lendo o arquivo inteiro

```python
with open('exemplo.txt', 'r', encoding='utf-8') as f:
    conteudo = f.read()  # lê tudo de uma vez como uma única string

print(conteudo)
```

### Exemplo: lendo linha por linha

```python
with open('exemplo.txt', 'r', encoding='utf-8') as f:
    for linha in f:
        print(linha.strip())  # strip() remove o '\n' e espaços extras do início/fim
```

**Por que ler linha por linha?** Quando o arquivo é muito grande (milhões de linhas), `read()` carregaria tudo na memória de uma vez, o que pode travar o programa. Ler linha por linha processa o arquivo em pequenas porções.

### Exemplo: adicionando conteúdo sem apagar o que existe

```python
with open('exemplo.txt', 'a', encoding='utf-8') as f:
    f.write('Quarta linha adicionada depois\n')

with open('exemplo.txt', 'r', encoding='utf-8') as f:
    print(f.read())
```

---

## 3. CSV (Comma-Separated Values)

### Explicação

CSV é o formato de "planilha em texto puro": cada linha do arquivo é uma linha da tabela, e os valores de cada coluna são separados por vírgula (ou ponto e vírgula, dependendo da configuração regional).

Vamos ver duas formas de trabalhar com CSV:

1. **Módulo `csv`**, que já vem com o Python (não precisa instalar nada). Bom para entender o que está acontecendo "por baixo dos panos".
2. **Biblioteca `pandas`**, que é o padrão da indústria para análise de dados. Ela transforma o CSV em uma tabela chamada **DataFrame**, que tem métodos prontos para filtrar, calcular, ordenar e visualizar dados.

### Exemplo: criando um CSV com o módulo `csv`

```python
import csv

# Cada item da lista representa uma linha da tabela
dados = [
    ['nome', 'idade', 'cidade'],          # esta linha será o cabeçalho
    ['Ana', 28, 'São Paulo'],
    ['Bruno', 35, 'Rio de Janeiro'],
    ['Carla', 22, 'Belo Horizonte']
]

# newline='' evita que o Windows insira linhas em branco extras
with open('pessoas.csv', 'w', newline='', encoding='utf-8') as f:
    writer = csv.writer(f)
    writer.writerows(dados)  # escreve todas as linhas de uma vez

print('CSV criado!')
```

### Exemplo: lendo um CSV com o módulo `csv`

```python
with open('pessoas.csv', 'r', encoding='utf-8') as f:
    reader = csv.reader(f)
    for linha in reader:
        print(linha)  # cada 'linha' é uma lista de strings, ex: ['Ana', '28', 'São Paulo']
```

Observe que, ao ler um CSV, **todos os valores chegam como texto (string)**, mesmo que pareçam números. Se quiser somar ou comparar números, é preciso converter explicitamente com `int()` ou `float()`.

### Exemplo: usando `DictReader` para ler cada linha como um dicionário

```python
with open('pessoas.csv', 'r', encoding='utf-8') as f:
    reader = csv.DictReader(f)
    for linha in reader:
        # 'linha' agora é um dicionário, ex: {'nome': 'Ana', 'idade': '28', 'cidade': 'São Paulo'}
        print(linha['nome'], '-', linha['cidade'])
```

### Exemplo: a forma mais prática — usando pandas

```python
import pandas as pd

df = pd.read_csv('pessoas.csv')
df  # no Colab, basta escrever o nome da variável para visualizar a tabela formatada
```

### Exemplo: operações comuns com o DataFrame

```python
# Informações gerais sobre as colunas e tipos de dados
df.info()

# Estatísticas básicas da coluna 'idade' (média, mínimo, máximo etc.)
print(df['idade'].describe())

# Filtrando: somente pessoas com mais de 25 anos
maiores_25 = df[df['idade'] > 25]
print(maiores_25)
```

**Explicação do filtro:** `df['idade'] > 25` cria uma lista de valores `True`/`False`, um para cada linha. Ao colocar essa lista dentro de `df[...]`, o pandas retorna apenas as linhas marcadas como `True`.

### Exemplo: criando uma nova coluna e salvando o resultado

```python
df['idade_em_5_anos'] = df['idade'] + 5  # cria uma nova coluna calculada
df.to_csv('pessoas_atualizado.csv', index=False)  # index=False evita salvar a coluna de índice

print('Arquivo salvo!')
df
```

---

## 4. JSON (JavaScript Object Notation)

### Explicação

JSON é o formato ideal para dados que têm **estrutura hierárquica** — ou seja, dados dentro de dados. Por exemplo, uma lista de alunos, onde cada aluno tem nome e nota, é mais natural em JSON do que em CSV.

No Python, o módulo `json` faz a conversão entre:

- **Dicionários e listas do Python** (estruturas que já conhecemos)
- **Texto em formato JSON** (o que fica salvo no arquivo ou é enviado por uma API)

As funções principais são:

| Função | O que faz |
|--------|-----------|
| `json.dump(dados, arquivo)` | Salva um dicionário/lista do Python em um **arquivo** JSON |
| `json.load(arquivo)` | Lê um **arquivo** JSON e transforma em dicionário/lista do Python |
| `json.dumps(dados)` | Converte um dicionário/lista em **string** JSON (sem salvar em arquivo) |
| `json.loads(texto)` | Converte uma **string** JSON em dicionário/lista do Python |

(Dica para lembrar: as funções com "s" no final — `dumps` e `loads` — trabalham com **strings**.)

### Exemplo: criando e salvando dados em JSON

```python
import json

dados_json = {
    'curso': 'Python para Análise de Dados',
    'alunos': [
        {'nome': 'Ana', 'nota': 9.5},
        {'nome': 'Bruno', 'nota': 7.8}
    ],
    'ativo': True
}

# indent=4 formata o arquivo com identação, facilitando a leitura
# ensure_ascii=False permite salvar acentos corretamente
with open('curso.json', 'w', encoding='utf-8') as f:
    json.dump(dados_json, f, indent=4, ensure_ascii=False)

print('JSON salvo!')
```

### Exemplo: lendo um arquivo JSON

```python
with open('curso.json', 'r', encoding='utf-8') as f:
    dados_lidos = json.load(f)

print(dados_lidos)
print('Nome do curso:', dados_lidos['curso'])
print('Primeiro aluno:', dados_lidos['alunos'][0]['nome'])
```

Note como acessamos os dados: `dados_lidos['alunos']` retorna a lista de alunos, e `[0]` pega o primeiro item dessa lista, que é um dicionário com `'nome'` e `'nota'`.

### Exemplo: transformando uma lista de dicionários JSON em uma tabela (DataFrame)

```python
df_alunos = pd.DataFrame(dados_lidos['alunos'])
df_alunos
```

Isso é extremamente útil: sempre que você tiver uma lista de "registros" (dicionários com as mesmas chaves), o pandas consegue transformar diretamente em uma tabela.

### Exemplo: convertendo entre dicionário e string JSON (sem usar arquivo)

```python
# Transformando o dicionário Python em uma string JSON
texto_json = json.dumps(dados_json, indent=2, ensure_ascii=False)
print(texto_json)
print(type(texto_json))  # <class 'str'>

# Transformando a string JSON de volta em dicionário Python
objeto = json.loads(texto_json)
print(type(objeto))  # <class 'dict'>
print(objeto['curso'])
```

---

## 5. Excel (.xlsx)

### Explicação

Para ler e escrever arquivos Excel usamos o `pandas` junto com a biblioteca `openpyxl`, que faz o trabalho de converter entre o DataFrame e o formato `.xlsx`. No Colab, o `openpyxl` geralmente já vem instalado.

As funções principais são `pd.read_excel()` para ler e `df.to_excel()` para salvar — muito parecidas com as que já vimos para CSV.

### Exemplo: instalando a dependência com uv (se necessário)

```bash
uv pip install openpyxl
```

### Exemplo: criando e salvando um Excel

```python
import pandas as pd

df_vendas = pd.DataFrame({
    'produto': ['Notebook', 'Mouse', 'Teclado', 'Monitor'],
    'preco': [3500, 80, 150, 900],
    'quantidade': [10, 50, 30, 15]
})

df_vendas.to_excel('vendas.xlsx', index=False, sheet_name='Vendas')
print('Excel salvo!')
```

### Exemplo: lendo um Excel e fazendo um cálculo

```python
df_lido = pd.read_excel('vendas.xlsx', sheet_name='Vendas')
df_lido['total'] = df_lido['preco'] * df_lido['quantidade']
df_lido
```

### Exemplo: salvando várias planilhas (abas) em um único arquivo

```python
df_clientes = pd.DataFrame({
    'cliente': ['Loja A', 'Loja B'],
    'cidade': ['Curitiba', 'Salvador']
})

# ExcelWriter permite escrever múltiplas abas no mesmo arquivo
with pd.ExcelWriter('relatorio.xlsx') as writer:
    df_lido.to_excel(writer, sheet_name='Vendas', index=False)
    df_clientes.to_excel(writer, sheet_name='Clientes', index=False)

print('Relatório com múltiplas abas salvo!')
```

---

## 6. XML (eXtensible Markup Language)

### Explicação

XML é um formato baseado em **tags hierárquicas**, similar ao HTML. É muito usado em configurações, troca de dados entre sistemas e em padrões como SOAP (webservices). Diferente do JSON que usa chaves e valores, XML usa tags aninhadas.

No Python, usamos a biblioteca `xml.etree.ElementTree` (já vem instalada) para ler e manipular arquivos XML.

### Exemplo: criando um arquivo XML

```python
import xml.etree.ElementTree as ET

# Criando a estrutura de forma manual
root = ET.Element('escola')
root.set('nome', 'Escola ABC')

# Adicionando um aluno
aluno1 = ET.SubElement(root, 'aluno')
nome1 = ET.SubElement(aluno1, 'nome')
nome1.text = 'Ana Silva'
nota1 = ET.SubElement(aluno1, 'nota')
nota1.text = '9.5'

# Adicionando outro aluno
aluno2 = ET.SubElement(root, 'aluno')
nome2 = ET.SubElement(aluno2, 'nome')
nome2.text = 'Bruno Costa'
nota2 = ET.SubElement(aluno2, 'nota')
nota2.text = '8.2'

# Salvando em arquivo
tree = ET.ElementTree(root)
tree.write('escola.xml', encoding='utf-8', xml_declaration=True)
print('XML criado!')
```

### Exemplo: lendo um arquivo XML

```python
tree = ET.parse('escola.xml')
root = tree.getroot()

print(f'Escola: {root.get("nome")}')
print('\nAlunos:')

for aluno in root.findall('aluno'):
    nome = aluno.find('nome').text
    nota = aluno.find('nota').text
    print(f'  {nome}: {nota}')
```

### Exemplo: extraindo dados XML para um DataFrame

```python
dados = []
for aluno in root.findall('aluno'):
    nome = aluno.find('nome').text
    nota = float(aluno.find('nota').text)
    dados.append({'nome': nome, 'nota': nota})

df_alunos = pd.DataFrame(dados)
print(df_alunos)
```

### Exemplo: modificando e salvando XML

```python
# Encontrar um aluno e alterar sua nota
for aluno in root.findall('aluno'):
    if aluno.find('nome').text == 'Ana Silva':
        aluno.find('nota').text = '9.8'

# Salvar as mudanças
tree = ET.ElementTree(root)
tree.write('escola.xml', encoding='utf-8', xml_declaration=True)
print('XML atualizado!')
```

---

## 7. PDF (Portable Document Format)

### Explicação

PDF é um formato que preserva exatamente como um documento deve ser apresentado — fontes, layout, imagens etc. É ótimo para relatórios finalizados, mas **difícil de extrair dados estruturados**.

Para trabalhar com PDF em Python usamos principalmente:
- **pdfplumber**: melhor para extrair dados estruturados (tabelas, texto)
- **PyPDF2**: para manipular a estrutura do PDF (dividir, juntar, extrair páginas)
- **reportlab**: para **criar** PDFs do zero

### Exemplo: instalando as dependências com uv

```bash
uv pip install pdfplumber PyPDF2 reportlab
```

### Exemplo: instalando as dependências com Google Colab

```python
import sys
!{sys.executable} -m pip install pdfplumber
```

### Exemplo: lendo texto de um PDF (pdfplumber)

```python
import pdfplumber

with pdfplumber.open('relatorio.pdf') as pdf:
    print(f'Total de páginas: {len(pdf.pages)}')
    
    # Extrair texto da primeira página
    primeira_pagina = pdf.pages[0]
    texto = primeira_pagina.extract_text()
    print(texto)
```

### Exemplo: extraindo tabelas de um PDF

```python
import pdfplumber

with pdfplumber.open('relatorio.pdf') as pdf:
    # Se a PDF tem tabelas, pdfplumber consegue extrair
    primeira_pagina = pdf.pages[0]
    tabelas = primeira_pagina.extract_tables()
    
    if tabelas:
        # Converter a primeira tabela em DataFrame
        df = pd.DataFrame(tabelas[0][1:], columns=tabelas[0][0])
        print(df)
```

### Exemplo: instalando as dependências com Google Colab

```python
import sys
!{sys.executable} -m pip install PyPDF2
```

### Exemplo: manipulando PDFs com PyPDF2

```python
from PyPDF2 import PdfReader, PdfWriter

# Ler um PDF existente
pdf_reader = PdfReader('documento.pdf')
pdf_writer = PdfWriter()

# Extrair páginas específicas (ex: apenas as 3 primeiras)
for i in range(min(3, len(pdf_reader.pages))):
    pdf_writer.add_page(pdf_reader.pages[i])

# Salvar novo PDF
with open('documento_resumido.pdf', 'wb') as saida:
    pdf_writer.write(saida)

print('PDF dividido e salvo!')
```

```python
import sys
!{sys.executable} -m pip install reportlab
```

### Exemplo: criando um PDF do zero (reportlab)

```python
from reportlab.lib.pagesizes import letter
from reportlab.pdfgen import canvas

# Criar um PDF
c = canvas.Canvas('relatorio_novo.pdf', pagesize=letter)
width, height = letter

# Adicionar título
c.setFont('Helvetica-Bold', 16)
c.drawString(50, height - 50, 'Relatório de Vendas')

# Adicionar conteúdo
c.setFont('Helvetica', 12)
c.drawString(50, height - 80, 'Empresa: Venda ABC Ltda')
c.drawString(50, height - 100, 'Período: Janeiro 2024')
c.drawString(50, height - 120, 'Total de vendas: R$ 150.000,00')

# Salvar
c.save()
print('PDF criado!')
```

### Exemplo: criando um PDF com tabela a partir de um DataFrame

```python
from reportlab.lib.pagesizes import letter
from reportlab.platypus import SimpleDocTemplate, Table, TableStyle, Paragraph
from reportlab.lib.styles import getSampleStyleSheet
from reportlab.lib import colors

# Preparar dados da tabela
dados = [
    ['Produto', 'Quantidade', 'Preço'],
    ['Notebook', '10', 'R$ 3.500,00'],
    ['Mouse', '50', 'R$ 80,00'],
    ['Teclado', '30', 'R$ 150,00']
]

# Criar PDF
doc = SimpleDocTemplate('tabela_vendas.pdf', pagesize=letter)
story = []

# Adicionar título
styles = getSampleStyleSheet()
titulo = Paragraph('Relatório de Vendas', styles['Heading1'])
story.append(titulo)

# Criar tabela
tabela = Table(dados)
tabela.setStyle(TableStyle([
    ('BACKGROUND', (0, 0), (-1, 0), colors.grey),
    ('TEXTCOLOR', (0, 0), (-1, 0), colors.whitesmoke),
    ('ALIGN', (0, 0), (-1, -1), 'CENTER'),
    ('FONTNAME', (0, 0), (-1, 0), 'Helvetica-Bold'),
    ('FONTSIZE', (0, 0), (-1, 0), 14),
    ('BOTTOMPADDING', (0, 0), (-1, 0), 12),
    ('GRID', (0, 0), (-1, -1), 1, colors.black)
]))
story.append(tabela)

# Salvar
doc.build(story)
print('PDF com tabela criado!')
```

---

## 8. DOCX (Microsoft Word Document)

### Explicação

DOCX é o formato de documentos do Microsoft Word. É perfeito quando você precisa criar documentos **formatados e editáveis** — com fontes, cores, tabelas, cabeçalhos, etc.

Para trabalhar com DOCX usamos a biblioteca **python-docx**.

### Exemplo: instalando a dependência com uv

```bash
uv pip install python-docx
```

```python
import sys
!{sys.executable} -m pip install python-docx
```

### Exemplo: lendo um arquivo DOCX

```python
from docx import Document

doc = Document('documento.docx')

# Extrair todos os parágrafos
print('Conteúdo do documento:')
for paragrafo in doc.paragraphs:
    if paragrafo.text.strip():  # pula parágrafos vazios
        print(paragrafo.text)
```

### Exemplo: criando um DOCX do zero

```python
from docx import Document
from docx.shared import Pt, RGBColor

doc = Document()

# Adicionar título
titulo = doc.add_heading('Relatório de Vendas', 0)

# Adicionar um parágrafo
doc.add_paragraph('Este é um relatório de vendas gerado automaticamente.')

# Adicionar um parágrafo com formatação
paragrafo = doc.add_paragraph('Período: Janeiro 2024')
paragrafo_run = paragrafo.runs[0]
paragrafo_run.bold = True
paragrafo_run.font.size = Pt(14)

# Adicionar uma tabela
tabela = doc.add_table(rows=4, cols=3)
tabela.style = 'Light Grid Accent 1'

# Preenchendo o cabeçalho
celulas = tabela.rows[0].cells
celulas[0].text = 'Produto'
celulas[1].text = 'Quantidade'
celulas[2].text = 'Preço'

# Preenchendo dados
dados = [
    ['Notebook', '10', 'R$ 3.500,00'],
    ['Mouse', '50', 'R$ 80,00'],
    ['Teclado', '30', 'R$ 150,00']
]

for i, linha in enumerate(dados, 1):
    celulas = tabela.rows[i].cells
    celulas[0].text = linha[0]
    celulas[1].text = linha[1]
    celulas[2].text = linha[2]

# Salvar documento
doc.save('relatorio_novo.docx')
print('DOCX criado!')
```

### Exemplo: extraindo tabelas de um DOCX para DataFrame

```python
from docx import Document

doc = Document('relatorio.docx')

# Encontrar a primeira tabela
if doc.tables:
    tabela = doc.tables[0]
    
    # Extrair dados
    dados = []
    for linha in tabela.rows:
        dados.append([celula.text for celula in linha.cells])
    
    # Converter em DataFrame
    df = pd.DataFrame(dados[1:], columns=dados[0])
    print(df)
```

### Exemplo: adicionando imagens ao DOCX

```python
from docx import Document
from docx.shared import Pt

doc = Document()

doc.add_heading('Relatório com Imagem', 0)
doc.add_paragraph('Veja a imagem abaixo:')

# Adicionar uma imagem (deve existir no seu sistema)
doc.add_picture('grafico.png', width=Pt(400))

doc.add_paragraph('Fim do relatório.')

doc.save('relatorio_com_imagem.docx')
print('DOCX com imagem criado!')
```

### Exemplo: adicionando estilos e formatação

```python
from docx import Document
from docx.shared import Pt, RGBColor
from docx.enum.text import WD_ALIGN_PARAGRAPH

doc = Document()

# Título centralizado
titulo = doc.add_heading('Relatório Executivo', 0)
titulo_paragraf = titulo.paragraph_format
titulo_paragraf.alignment = WD_ALIGN_PARAGRAPH.CENTER

# Parágrafo com estilo
doc.add_paragraph(
    'Este documento foi gerado automaticamente.',
    style='List Bullet'
)

# Texto com múltiplos estilos
p = doc.add_paragraph()
p.add_run('Negrito: ').bold = True
p.add_run('Este é um texto em negrito. ')
p.add_run('Itálico: ').italic = True
p.add_run('Este é um texto em itálico.')

# Parágrafo vermelho
p_vermelho = doc.add_paragraph('Texto em cor vermelha')
for run in p_vermelho.runs:
    run.font.color.rgb = RGBColor(255, 0, 0)

doc.save('relatorio_formatado.docx')
print('DOCX com formatação criado!')
```

---

## 9. Banco de dados SQLite

### Explicação

SQLite é um banco de dados que vive em **um único arquivo** no disco — diferente de bancos como MySQL ou PostgreSQL, que exigem um servidor rodando. É perfeito para aprendizado, prototipagem e projetos pequenos/médios.

O fluxo básico de trabalho é:

1. **Conectar** ao arquivo do banco (criando-o se não existir) com `sqlite3.connect()`.
2. Criar um **cursor**, que é o objeto usado para executar comandos SQL.
3. Executar comandos SQL com `cursor.execute()` (para um comando) ou `cursor.executemany()` (para vários comandos repetidos).
4. Confirmar as alterações com `conexao.commit()`.
5. **Fechar a conexão** com `conexao.close()` ao terminar.

### Exemplo: criando uma tabela

```python
import sqlite3

# Conecta ao arquivo 'banco.db' (cria se não existir)
conexao = sqlite3.connect('banco.db')
cursor = conexao.cursor()

# Comando SQL para criar uma tabela, caso ela ainda não exista
cursor.execute('''
CREATE TABLE IF NOT EXISTS funcionarios (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    nome TEXT NOT NULL,
    cargo TEXT,
    salario REAL
)
''')

conexao.commit()  # salva as alterações no arquivo
print('Tabela criada!')
```

### Exemplo: inserindo vários registros de uma vez

```python
funcionarios = [
    ('Mariana', 'Analista', 5000),
    ('Pedro', 'Gerente', 9000),
    ('Joana', 'Desenvolvedora', 7000)
]

# Os '?' são marcadores de posição, substituídos pelos valores da tupla
# Essa técnica evita um problema de segurança chamado "SQL Injection"
cursor.executemany('INSERT INTO funcionarios (nome, cargo, salario) VALUES (?, ?, ?)', funcionarios)
conexao.commit()

print(f'{cursor.rowcount} registros inseridos!')
```

### Exemplo: consultando dados com SQL puro

```python
cursor.execute('SELECT * FROM funcionarios WHERE salario > 6000')
resultados = cursor.fetchall()  # retorna todas as linhas que combinam com a consulta

for linha in resultados:
    print(linha)
```

### Exemplo: trazendo o resultado direto para um DataFrame

```python
df_funcionarios = pd.read_sql_query('SELECT * FROM funcionarios', conexao)
df_funcionarios
```

Essa é uma combinação muito poderosa: usar SQL para filtrar/agregar dados diretamente no banco, e o pandas para análise e visualização do resultado.

### Exemplo: fechando a conexão

```python
conexao.close()
print('Conexão encerrada')
```

**Por que fechar a conexão é importante?** Enquanto a conexão está aberta, o arquivo do banco pode ficar bloqueado para outras operações, e alterações pendentes podem não ser salvas corretamente.

---

## 10. Consumindo dados de uma API

### Explicação

Uma **API** (Interface de Programação de Aplicações) é um serviço na internet que recebe pedidos (requisições) e devolve respostas, geralmente em formato JSON. Para fazer essas requisições usamos a biblioteca `requests`.

O fluxo básico é:

1. Fazer a requisição com `requests.get(url)`.
2. Verificar o `status_code` da resposta (200 significa sucesso).
3. Converter a resposta para JSON com `.json()`.
4. Transformar o resultado em um DataFrame, se necessário.

### Exemplo: fazendo uma requisição GET

```python
import requests

# API pública gratuita com dados fictícios, usada para testes e aprendizado
resposta = requests.get('https://jsonplaceholder.typicode.com/users')

print('Status code:', resposta.status_code)  # 200 = sucesso, 404 = não encontrado, etc.

if resposta.status_code == 200:
    dados_api = resposta.json()  # converte o corpo da resposta de JSON para lista/dicionário Python
    print(f'{len(dados_api)} usuários retornados')
    print(dados_api[0])  # mostra o primeiro usuário
```

### Exemplo: transformando a resposta em uma tabela

```python
# json_normalize "achata" estruturas aninhadas em colunas, ex: endereco.cidade
df_usuarios = pd.json_normalize(dados_api)
df_usuarios[['id', 'name', 'email', 'address.city']]
```

### Exemplo: salvando os dados da API localmente

```python
df_usuarios.to_csv('usuarios_api.csv', index=False)
print('Dados da API salvos em CSV!')
```

A partir daqui, você pode tratar esses dados exatamente como qualquer outro DataFrame: filtrar, calcular, exportar para Excel etc.

---

## 11. Upload e download de arquivos no Colab

### Explicação

O Google Colab roda em uma máquina remota, então ele não tem acesso direto aos arquivos do seu computador. Para isso, o módulo `google.colab.files` oferece duas funções:

- `files.upload()`: abre uma janela para você escolher arquivos do seu computador e enviá-los para o ambiente do Colab.
- `files.download(caminho)`: baixa um arquivo do ambiente do Colab para o seu computador.

### Exemplo: enviando um arquivo do computador

```python
from google.colab import files

uploaded = files.upload()  # abre a janela de seleção de arquivos

for nome_arquivo in uploaded.keys():
    print(f'Arquivo "{nome_arquivo}" enviado, {len(uploaded[nome_arquivo])} bytes')
```

Após o upload, o arquivo fica disponível na pasta atual do Colab e pode ser lido normalmente com `pd.read_csv()`, `open()`, etc.

### Exemplo: baixando um arquivo gerado

```python
from google.colab import files

files.download('relatorio.xlsx')
```

---

## 12. Conectando ao Google Drive

### Explicação

Arquivos criados no Colab são **temporários**: quando a sessão termina, eles são apagados. Para guardar dados de forma permanente, podemos "montar" o Google Drive, o que faz seus arquivos do Drive ficarem acessíveis no Colab.

Após montar o Drive, qualquer arquivo salvo dentro de `/content/drive/My Drive/` ficará disponível na sua conta do Google Drive, mesmo depois que a sessão do Colab terminar.

### Exemplo: montando o Drive

```python
from google.colab import drive

drive.mount('/content/drive')
# Uma janela pedirá autorização de acesso à sua conta Google na primeira vez
```

### Exemplo: salvando um arquivo direto no Drive

```python
caminho_drive = '/content/drive/My Drive/pessoas_drive.csv'

df.to_csv(caminho_drive, index=False)
print('Arquivo salvo no Google Drive!')
```

### Exemplo: lendo um arquivo do Drive

```python
df_do_drive = pd.read_csv(caminho_drive)
df_do_drive
```

---

## 13. Resumo: qual formato usar em cada situação

| Formato | Quando usar |
|---------|-------------|
| TXT | Logs, anotações simples, textos corridos |
| CSV | Tabelas simples, troca de dados entre sistemas diferentes |
| JSON | Dados hierárquicos, configurações, respostas de APIs |
| Excel | Relatórios para humanos, múltiplas abas, formatação visual |
| XML | Configurações, troca de dados estruturados, padrões corporativos |
| PDF | Relatórios finalizados, documentos não-editáveis, preservação de layout |
| DOCX | Documentos editáveis, relatórios formatados, textos com imagens |
| SQLite | Dados relacionais, consultas complexas, projetos pequenos/médios |
| API + requests | Dados em tempo real, integração com serviços externos |

---

## 14. Exercícios práticos

1. **CSV**: crie um arquivo CSV com uma lista de produtos (nome, preço, quantidade em estoque). Usando pandas, calcule o valor total do estoque (preço × quantidade) e salve o resultado em um novo CSV.

2. **JSON + API**: faça uma requisição para `https://jsonplaceholder.typicode.com/posts`, transforme o resultado em DataFrame e salve apenas as colunas `userId` e `title` em um arquivo Excel.

3. **SQLite**: crie um banco com duas tabelas, `clientes` (id, nome, cidade) e `pedidos` (id, cliente_id, valor). Insira alguns registros e faça uma consulta SQL que junte (`JOIN`) as duas tabelas, mostrando nome do cliente com seus pedidos.

4. **XML**: crie um arquivo XML com uma lista de livros (título, autor, ano). Depois leia o arquivo e exporte os dados para um DataFrame.

5. **PDF + Tabelas**: crie um DataFrame com dados de vendas, exporte para Excel, depois use pdfplumber para extrair dados de um PDF existente e compare os dois.

6. **DOCX**: crie um documento Word com título, parágrafos formatados e uma tabela contendo dados de um DataFrame.

7. **Upload + Drive**: faça upload de um arquivo CSV qualquer, use pandas para remover linhas com valores nulos (`df.dropna()`), e salve o resultado limpo direto no seu Google Drive.

8. **Desafio extra**: combine tudo! Busque dados de uma API, salve-os em SQLite, depois leia do SQLite com pandas, crie um documento DOCX com tabelas, exporte para PDF com gráficos, e finalize com um arquivo Excel resumido. Use `uv pip install` para todas as dependências e `files.download()` para baixar os arquivos finais.
