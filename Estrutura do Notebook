---
Agora a estrutura completa:

---
Estrutura do Notebook

Célula 1 — Markdown (Título e Definição do Problema)

# Análise da Cota para o Exercício da Atividade Parlamentar (CEAP)
## Câmara dos Deputados Federais do Brasil

**Aluno:** Matheus
**Disciplina:** Análise de Dados e Boas Práticas
**Instituição:** PUC-Rio
**Data:** 2026

---

## 1. Definição do Problema

### Contexto
A Cota para o Exercício da Atividade Parlamentar (CEAP) é um benefício mensal
concedido a cada deputado federal para cobrir despesas relacionadas ao exercício
do mandato. Em 2023, o limite mensal por parlamentar era de aproximadamente
R$ 45.600,00, variando conforme o estado de origem do deputado.

A transparência no uso desse recurso é fundamental para o controle social.
Os dados são públicos e disponibilizados pela Câmara dos Deputados, mas sua
análise sistemática ainda é pouco acessível ao cidadão comum.

### Descrição do Problema
Este trabalho busca responder: **Como os deputados federais utilizam a CEAP?**
Existem padrões de gasto por partido, estado ou tipo de despesa?
Há valores atípicos que merecem atenção?

### Tipo de Aprendizado
Este é um problema de **aprendizado não supervisionado** — não há uma variável-alvo
definida. O objetivo é explorar e compreender os dados, identificar padrões e
anomalias por meio de análise exploratória e pré-processamento.

### Premissas e Hipóteses
- Deputados de estados mais distantes de Brasília tendem a gastar mais com passagens aéreas
- Existe variação significativa no total de gastos entre partidos
- Uma parcela pequena de deputados concentra os maiores gastos
- Existem valores atípicos (outliers) que podem indicar uso irregular da cota

### Restrições e Condições de Seleção dos Dados
- Dados referentes ao ano de 2023 (legislatura vigente)
- Considerados apenas registros com valor líquido positivo (despesas válidas)
- Excluídas despesas com valor zero ou negativo (reembolsos/estornos)

### Atributos do Dataset
| Atributo | Tipo | Descrição |
|---|---|---|
| txNomeParlamentar | Qualitativo Nominal | Nome do deputado |
| sgUF | Qualitativo Nominal | Estado de origem |
| sgPartido | Qualitativo Nominal | Partido político |
| txtDescricao | Qualitativo Nominal | Tipo/categoria da despesa |
| txtFornecedor | Qualitativo Nominal | Nome do fornecedor |
| txtCNPJCPF | Qualitativo Nominal | CNPJ ou CPF do fornecedor |
| datEmissao | Quantitativo Discreto | Data de emissão do documento |
| vlrDocumento | Quantitativo Contínuo | Valor bruto do documento |
| vlrGlosa | Quantitativo Contínuo | Valor glosado (não reembolsado) |
| vlrLiquido | Quantitativo Contínuo | Valor líquido reembolsado |
| numMes | Quantitativo Discreto | Mês de referência |
| numAno | Quantitativo Discreto | Ano de referência |

---
Célula 2 — Código (Imports e Configurações)

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.ticker as mticker
import seaborn as sns
from sklearn.preprocessing import MinMaxScaler, StandardScaler
import warnings

warnings.filterwarnings('ignore')

plt.rcParams['figure.figsize'] = (14, 6)
plt.rcParams['font.size'] = 12
sns.set_theme(style='whitegrid', palette='muted')

---
Célula 3 — Markdown

## 2. Análise de Dados

### 2.1 Carregamento e Visão Geral dos Dados

---
Célula 4 — Código (Carregamento)

# Leitura do dataset via URL raw do GitHub
URL = 'https://raw.githubusercontent.com/matheusd09/SEU-REPO/main/ceap_2023.csv'

df = pd.read_csv(URL, sep=';', encoding='utf-8', decimal=',')

print(f"Shape do dataset: {df.shape}")
print(f"\nNúmero de instâncias: {df.shape[0]}")
print(f"Número de atributos: {df.shape[1]}")

---
Célula 5 — Código (Primeiras linhas)

df.head()

---
Célula 6 — Código (Tipos e valores ausentes)

print("=== Tipos de dados ===")
print(df.dtypes)

print("\n=== Valores ausentes por coluna ===")
missing = df.isnull().sum()
missing_pct = (missing / len(df) * 100).round(2)
pd.DataFrame({'Ausentes': missing, '% Ausentes': missing_pct})[missing > 0]

---
Célula 7 — Markdown

### 2.2 Estatísticas Descritivas

---
Célula 8 — Código (Describe numérico)

colunas_numericas = ['vlrDocumento', 'vlrGlosa', 'vlrLiquido']
df[colunas_numericas].describe().round(2)

---
Célula 9 — Markdown

A tabela acima apresenta o resumo estatístico das variáveis numéricas do dataset.
[Após executar, escreva aqui as observações: valor mínimo, máximo, média, mediana,
desvio padrão e o que cada um indica sobre os dados.]

---
Célula 10 — Código (Describe categórico)

colunas_cat = ['sgUF', 'sgPartido', 'txtDescricao']

for col in colunas_cat:
    print(f"\n=== {col} ===")
    print(f"Valores únicos: {df[col].nunique()}")
    print(df[col].value_counts().head(10))

---
Célula 11 — Markdown

### 2.3 Visualizações

#### Gráfico 1 — Total de gastos por tipo de despesa

---
Célula 12 — Código (Gráfico 1: Gastos por tipo)

gastos_tipo = (df.groupby('txtDescricao')['vlrLiquido']
                 .sum()
                 .sort_values(ascending=True))

fig, ax = plt.subplots(figsize=(14, 8))
bars = ax.barh(gastos_tipo.index, gastos_tipo.values / 1e6, color='steelblue')
ax.set_xlabel('Total Gasto (R$ milhões)')
ax.set_title('Total de Gastos por Tipo de Despesa — CEAP 2023', fontsize=14, pad=15)
ax.xaxis.set_major_formatter(mticker.FuncFormatter(lambda x, _: f'R$ {x:.0f}M'))

for bar in bars:
    width = bar.get_width()
    ax.text(width + 0.5, bar.get_y() + bar.get_height()/2,
            f'R$ {width:.1f}M', va='center', fontsize=9)

plt.tight_layout()
plt.show()

---
Célula 13 — Markdown

**Análise:** [Escreva aqui após ver o gráfico. Exemplo:]
O gráfico revela que as maiores categorias de despesa são... A categoria X concentra
aproximadamente X% do total de gastos. Destaca-se que despesas com [tipo] representam
um valor expressivo, o que pode indicar... Por outro lado, despesas com [tipo] são
proporcionalmente baixas, sugerindo...

---
Célula 14 — Markdown

#### Gráfico 2 — Total de gastos por partido

---
Célula 15 — Código (Gráfico 2: Gastos por partido)

gastos_partido = (df.groupby('sgPartido')['vlrLiquido']
                    .sum()
                    .sort_values(ascending=False)
                    .head(15))

fig, ax = plt.subplots(figsize=(14, 6))
sns.barplot(x=gastos_partido.values / 1e6, y=gastos_partido.index,
            palette='Blues_r', ax=ax)
ax.set_xlabel('Total Gasto (R$ milhões)')
ax.set_ylabel('Partido')
ax.set_title('Top 15 Partidos — Total de Gastos com CEAP (2023)', fontsize=14, pad=15)
ax.xaxis.set_major_formatter(mticker.FuncFormatter(lambda x, _: f'R$ {x:.0f}M'))
plt.tight_layout()
plt.show()

---
Célula 16 — Markdown

**Análise:** [Escreva aqui após ver o gráfico.]

---
Célula 17 — Markdown

#### Gráfico 3 — Distribuição dos valores líquidos (Histograma)

---
Célula 18 — Código (Gráfico 3: Histograma)

# Filtrando outliers extremos para melhor visualização
df_hist = df[df['vlrLiquido'] <= df['vlrLiquido'].quantile(0.99)]

fig, ax = plt.subplots(figsize=(14, 5))
ax.hist(df_hist['vlrLiquido'], bins=80, color='steelblue', edgecolor='white', alpha=0.8)
ax.axvline(df['vlrLiquido'].mean(), color='red', linestyle='--', label=f"Média: R$ {df['vlrLiquido'].mean():.2f}")
ax.axvline(df['vlrLiquido'].median(), color='orange', linestyle='--', label=f"Mediana: R$ {df['vlrLiquido'].median():.2f}")
ax.set_xlabel('Valor Líquido (R$)')
ax.set_ylabel('Frequência')
ax.set_title('Distribuição dos Valores Líquidos das Despesas', fontsize=14, pad=15)
ax.legend()
plt.tight_layout()
plt.show()

---
Célula 19 — Markdown

**Análise:** [Escreva aqui. Analise a assimetria da distribuição, o que a diferença entre
média e mediana indica, e por que a maioria das despesas se concentra em valores baixos.]

---
Célula 20 — Markdown

#### Gráfico 4 — Boxplot do valor líquido por partido (Top 10)

---
Célula 21 — Código (Gráfico 4: Boxplot)

top10_partidos = (df.groupby('sgPartido')['vlrLiquido']
                    .sum()
                    .sort_values(ascending=False)
                    .head(10)
                    .index.tolist())

df_box = df[df['sgPartido'].isin(top10_partidos) & (df['vlrLiquido'] <= df['vlrLiquido'].quantile(0.99))]

fig, ax = plt.subplots(figsize=(14, 6))
sns.boxplot(data=df_box, x='sgPartido', y='vlrLiquido',
            order=top10_partidos, palette='Set2', ax=ax)
ax.set_xlabel('Partido')
ax.set_ylabel('Valor Líquido (R$)')
ax.set_title('Distribuição dos Valores por Despesa — Top 10 Partidos', fontsize=14, pad=15)
ax.yaxis.set_major_formatter(mticker.FuncFormatter(lambda x, _: f'R$ {x:,.0f}'))
plt.tight_layout()
plt.show()

---
Célula 22 — Markdown

**Análise:** [Analise a variabilidade entre partidos, os outliers visíveis nos whiskers,
e o que as medianas comparadas revelam sobre o perfil de gasto de cada partido.]

---
Célula 23 — Markdown

#### Gráfico 5 — Evolução mensal dos gastos

---
Célula 24 — Código (Gráfico 5: Linha temporal)

gastos_mes = (df.groupby('numMes')['vlrLiquido']
                .sum()
                .reset_index()
                .sort_values('numMes'))

meses = ['Jan','Fev','Mar','Abr','Mai','Jun',
         'Jul','Ago','Set','Out','Nov','Dez']
gastos_mes['mes_nome'] = gastos_mes['numMes'].apply(lambda x: meses[x-1])

fig, ax = plt.subplots(figsize=(14, 5))
ax.plot(gastos_mes['mes_nome'], gastos_mes['vlrLiquido'] / 1e6,
        marker='o', linewidth=2.5, color='steelblue', markersize=8)
ax.fill_between(gastos_mes['mes_nome'], gastos_mes['vlrLiquido'] / 1e6,
                alpha=0.15, color='steelblue')
ax.set_xlabel('Mês')
ax.set_ylabel('Total Gasto (R$ milhões)')
ax.set_title('Evolução Mensal dos Gastos com CEAP — 2023', fontsize=14, pad=15)
ax.yaxis.set_major_formatter(mticker.FuncFormatter(lambda x, _: f'R$ {x:.0f}M'))
plt.tight_layout()
plt.show()

---
Célula 25 — Markdown

**Análise:** [Identifique picos e vales. Meses próximos a eleições ou recessos parlamentares
costumam apresentar comportamentos distintos. O que o padrão temporal revela?]

---
Célula 26 — Markdown

#### Gráfico 6 — Gastos por Unidade Federativa (UF)

---
Célula 27 — Código (Gráfico 6: Mapa de gastos por UF)

gastos_uf = (df.groupby('sgUF')['vlrLiquido']
               .sum()
               .sort_values(ascending=False))

fig, ax = plt.subplots(figsize=(14, 6))
cores = ['#d73027' if v == gastos_uf.max() else 'steelblue' for v in gastos_uf.values]
ax.bar(gastos_uf.index, gastos_uf.values / 1e6, color=cores)
ax.set_xlabel('Estado (UF)')
ax.set_ylabel('Total Gasto (R$ milhões)')
ax.set_title('Total de Gastos com CEAP por Estado — 2023', fontsize=14, pad=15)
ax.yaxis.set_major_formatter(mticker.FuncFormatter(lambda x, _: f'R$ {x:.0f}M'))
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()

---
Célula 28 — Markdown

**Análise:** [Analise se estados com mais deputados (SP, MG, RJ) lideram os gastos,
e se estados mais distantes de Brasília apresentam gastos maiores per capita.]

---
Célula 29 — Markdown

#### Gráfico 7 — Top 20 deputados por total gasto

---
Célula 30 — Código (Gráfico 7: Top deputados)

top_dep = (df.groupby(['txNomeParlamentar', 'sgPartido'])['vlrLiquido']
             .sum()
             .reset_index()
             .sort_values('vlrLiquido', ascending=False)
             .head(20))

top_dep['label'] = top_dep['txNomeParlamentar'] + ' (' + top_dep['sgPartido'] + ')'

fig, ax = plt.subplots(figsize=(14, 8))
ax.barh(top_dep['label'][::-1], top_dep['vlrLiquido'][::-1] / 1e3, color='steelblue')
ax.set_xlabel('Total Gasto (R$ mil)')
ax.set_title('Top 20 Deputados — Maior Gasto Total com CEAP (2023)', fontsize=14, pad=15)
ax.xaxis.set_major_formatter(mticker.FuncFormatter(lambda x, _: f'R$ {x:.0f}k'))
plt.tight_layout()
plt.show()

---
Célula 31 — Markdown

**Análise:** [Analise a concentração de gastos. Os top deputados estão próximos ou
acima do limite da CEAP? Há concentração em algum partido ou estado específico?]

---
Célula 32 — Markdown

#### Gráfico 8 — Matriz de correlação entre variáveis numéricas

---
Célula 33 — Código (Gráfico 8: Correlação)

corr = df[['vlrDocumento', 'vlrGlosa', 'vlrLiquido', 'numMes']].corr()

fig, ax = plt.subplots(figsize=(8, 6))
sns.heatmap(corr, annot=True, fmt='.2f', cmap='coolwarm',
            center=0, ax=ax, linewidths=0.5)
ax.set_title('Matriz de Correlação — Variáveis Numéricas', fontsize=14, pad=15)
plt.tight_layout()
plt.show()

---
Célula 34 — Markdown

**Análise:** [Analise as correlações encontradas. A correlação alta entre vlrDocumento
e vlrLiquido é esperada — o que ela significa? O vlrGlosa tem correlação com o valor
do documento?]

---
Célula 35 — Markdown

## 3. Pré-processamento de Dados

O pré-processamento visa preparar os dados para análises mais avançadas e futuros
modelos de machine learning. As operações abaixo foram selecionadas com base nos
problemas identificados na etapa anterior.

> **Boa prática:** antes de qualquer transformação, preservamos uma cópia do dataset original.

---
Célula 36 — Código (Cópia do dataset)

df_original = df.copy()
df_proc = df.copy()

print(f"Dataset original preservado: {df_original.shape}")
print(f"Dataset para processamento: {df_proc.shape}")

---
Célula 37 — Markdown

### 3.1 Limpeza dos Dados

#### Remoção de duplicatas

---
Célula 38 — Código (Duplicatas)

duplicatas = df_proc.duplicated().sum()
print(f"Registros duplicados encontrados: {duplicatas}")

df_proc = df_proc.drop_duplicates()
print(f"Shape após remoção: {df_proc.shape}")

---
Célula 39 — Markdown

#### Tratamento de valores ausentes

---
Célula 40 — Código (Missings)

missing_antes = df_proc.isnull().sum()
print("Valores ausentes antes do tratamento:")
print(missing_antes[missing_antes > 0])

# Preencher fornecedor ausente com 'Não Informado'
df_proc['txtFornecedor'] = df_proc['txtFornecedor'].fillna('Não Informado')
df_proc['txtCNPJCPF'] = df_proc['txtCNPJCPF'].fillna('Não Informado')

# Preencher partido ausente com 'S/PARTIDO'
df_proc['sgPartido'] = df_proc['sgPartido'].fillna('S/PARTIDO')

print("\nValores ausentes após o tratamento:")
print(df_proc.isnull().sum()[df_proc.isnull().sum() > 0])
print("Sem valores ausentes!" if df_proc.isnull().sum().sum() == 0 else "")

---
Célula 41 — Markdown

#### Tratamento de valores inválidos

Valores líquidos negativos ou zerados representam estornos ou registros inválidos
para fins de análise de gastos. Optamos por removê-los do dataset processado,
mantendo apenas despesas efetivamente reembolsadas.

---
Célula 42 — Código (Valores inválidos)

print(f"Registros com vlrLiquido <= 0: {(df_proc['vlrLiquido'] <= 0).sum()}")

df_proc = df_proc[df_proc['vlrLiquido'] > 0].reset_index(drop=True)
print(f"Shape após remoção de valores inválidos: {df_proc.shape}")

---
Célula 43 — Markdown

### 3.2 Discretização — Faixas de Valor de Despesa

Transformamos o valor líquido (variável contínua) em categorias (bins), o que
facilita análises segmentadas e pode ser útil em modelos de classificação futuros.

---
Célula 44 — Código (Discretização)

bins = [0, 500, 2000, 10000, 50000, float('inf')]
labels = ['Baixo (até R$500)', 'Médio-baixo (R$500-2k)',
          'Médio (R$2k-10k)', 'Alto (R$10k-50k)', 'Muito alto (>R$50k)']

df_proc['faixa_valor'] = pd.cut(df_proc['vlrLiquido'], bins=bins, labels=labels)

print("Distribuição por faixa de valor:")
print(df_proc['faixa_valor'].value_counts().sort_index())

# Visualização da discretização
fig, ax = plt.subplots(figsize=(12, 5))
df_proc['faixa_valor'].value_counts().sort_index().plot(kind='bar', ax=ax,
                                                         color='steelblue', edgecolor='white')
ax.set_xlabel('Faixa de Valor')
ax.set_ylabel('Quantidade de Despesas')
ax.set_title('Distribuição das Despesas por Faixa de Valor', fontsize=14, pad=15)
plt.xticks(rotation=30, ha='right')
plt.tight_layout()
plt.show()

---
Célula 45 — Markdown

**Análise:** [Analise o resultado da discretização. A maioria das despesas se
concentra em qual faixa? O que isso revela sobre o padrão de uso da CEAP?]

---
Célula 46 — Markdown

### 3.3 Normalização — MinMaxScaler

Aplicamos normalização Min-Max ao valor líquido para colocá-lo na escala [0, 1],
útil para algoritmos de machine learning sensíveis à magnitude das variáveis.

---
Célula 47 — Código (Normalização)

scaler_minmax = MinMaxScaler()
df_proc['vlrLiquido_normalizado'] = scaler_minmax.fit_transform(df_proc[['vlrLiquido']])

print("Antes da normalização:")
print(f"  Min: R$ {df_proc['vlrLiquido'].min():.2f} | Max: R$ {df_proc['vlrLiquido'].max():.2f}")
print("\nApós normalização (Min-Max):")
print(f"  Min: {df_proc['vlrLiquido_normalizado'].min():.4f} | Max: {df_proc['vlrLiquido_normalizado'].max():.4f}")
print(f"  Média: {df_proc['vlrLiquido_normalizado'].mean():.4f}")

---
Célula 48 — Markdown

### 3.4 Padronização — StandardScaler

Aplicamos padronização Z-score ao valor do documento para que a variável tenha
média 0 e desvio padrão 1, adequada para algoritmos que assumem distribuição normal.

---
Célula 49 — Código (Padronização)

scaler_std = StandardScaler()
df_proc['vlrDocumento_padronizado'] = scaler_std.fit_transform(df_proc[['vlrDocumento']])

print("Antes da padronização:")
print(f"  Média: R$ {df_proc['vlrDocumento'].mean():.2f} | Desvio padrão: {df_proc['vlrDocumento'].std():.2f}")
print("\nApós padronização (Z-score):")
print(f"  Média: {df_proc['vlrDocumento_padronizado'].mean():.4f}")
print(f"  Desvio padrão: {df_proc['vlrDocumento_padronizado'].std():.4f}")

---
Célula 50 — Markdown

### 3.5 One-Hot Encoding — Variável Região

Criamos a variável `regiao` agrupando os estados por região geográfica,
e aplicamos one-hot encoding para transformá-la em variáveis binárias,
adequadas para algoritmos de machine learning.

---
Célula 51 — Código (One-Hot Encoding)

regioes = {
    'Norte': ['AC','AM','AP','PA','RO','RR','TO'],
    'Nordeste': ['AL','BA','CE','MA','PB','PE','PI','RN','SE'],
    'Centro-Oeste': ['DF','GO','MS','MT'],
    'Sudeste': ['ES','MG','RJ','SP'],
    'Sul': ['PR','RS','SC']
}

def mapear_regiao(uf):
    for regiao, estados in regioes.items():
        if uf in estados:
            return regiao
    return 'Não identificado'

df_proc['regiao'] = df_proc['sgUF'].apply(mapear_regiao)

# One-Hot Encoding
df_ohe = pd.get_dummies(df_proc['regiao'], prefix='regiao')
df_proc = pd.concat([df_proc, df_ohe], axis=1)

print("Colunas criadas pelo One-Hot Encoding:")
print([col for col in df_proc.columns if col.startswith('regiao_')])
print(f"\nDistribuição por região:\n{df_proc['regiao'].value_counts()}")

---
Célula 52 — Markdown

### 3.6 Análise Exploratória Pós-Processamento

Após o pré-processamento, retornamos à análise exploratória para verificar se
novos insights surgem com os dados tratados.

---
Célula 53 — Código (Análise pós-processamento)

# Gasto médio por região
gasto_regiao = df_proc.groupby('regiao')['vlrLiquido'].agg(['mean', 'sum', 'count'])
gasto_regiao.columns = ['Média por Despesa', 'Total', 'Nº Despesas']
gasto_regiao['Média por Despesa'] = gasto_regiao['Média por Despesa'].round(2)
gasto_regiao = gasto_regiao.sort_values('Total', ascending=False)

print("Gastos por Região:")
print(gasto_regiao)

fig, axes = plt.subplots(1, 2, figsize=(14, 5))

# Total por região
gasto_regiao['Total'].plot(kind='bar', ax=axes[0], color='steelblue', edgecolor='white')
axes[0].set_title('Total de Gastos por Região', fontsize=13)
axes[0].set_xlabel('Região')
axes[0].set_ylabel('R$')
axes[0].tick_params(axis='x', rotation=30)
axes[0].yaxis.set_major_formatter(mticker.FuncFormatter(lambda x, _: f'R$ {x/1e6:.0f}M'))

# Média por região
gasto_regiao['Média por Despesa'].plot(kind='bar', ax=axes[1], color='coral', edgecolor='white')
axes[1].set_title('Valor Médio por Despesa — por Região', fontsize=13)
axes[1].set_xlabel('Região')
axes[1].set_ylabel('R$')
axes[1].tick_params(axis='x', rotation=30)
axes[1].yaxis.set_major_formatter(mticker.FuncFormatter(lambda x, _: f'R$ {x:,.0f}'))

plt.tight_layout()
plt.show()

---
Célula 54 — Markdown

**Análise:** [Após o agrupamento por região, analise se a hipótese de que estados
mais distantes de Brasília gastam mais (especialmente Norte e Nordeste) se confirma.
Compare total vs. média por despesa para entender se é volume ou valor unitário.]

---
Célula 55 — Markdown

### 3.7 Dataset Final Processado

---
Célula 56 — Código (Visão final)

colunas_finais = [
    'txNomeParlamentar', 'sgPartido', 'sgUF', 'regiao',
    'txtDescricao', 'numMes', 'vlrDocumento', 'vlrGlosa',
    'vlrLiquido', 'vlrLiquido_normalizado', 'vlrDocumento_padronizado',
    'faixa_valor'
] + [col for col in df_proc.columns if col.startswith('regiao_')]

df_final = df_proc[colunas_finais].copy()

print(f"Shape do dataset final: {df_final.shape}")
print(f"\nValores ausentes: {df_final.isnull().sum().sum()}")
print("\nPrimeiras linhas:")
df_final.head()

---
Célula 57 — Markdown (Conclusão)

## 4. Conclusões

Ao longo desta análise, exploramos o uso da Cota para o Exercício da Atividade
Parlamentar (CEAP) pelos deputados federais em 2023.

### Principais achados:
- [Preencha após executar todas as células]
- Tipo de despesa mais frequente: ...
- Partido com maior gasto total: ...
- Comportamento temporal: meses X e Y concentram maiores gastos
- Outliers identificados: deputados com gastos acima de R$ X mil/mês

### Operações de pré-processamento realizadas:
| Operação | Variável | Justificativa |
|---|---|---|
| Remoção de duplicatas | Todas | Garantir integridade dos dados |
| Preenchimento de missings | txtFornecedor, sgPartido | Manter registros com informação parcial |
| Remoção de valores inválidos | vlrLiquido ≤ 0 | Focar em despesas efetivas |
| Discretização (bins) | vlrLiquido | Segmentação por faixa de valor |
| Normalização Min-Max | vlrLiquido | Escala [0,1] para ML |
| Padronização Z-score | vlrDocumento | Média 0, desvio padrão 1 |
| One-Hot Encoding | regiao | Variável categórica → binária |

### Próximos passos sugeridos:
Com o dataset pré-processado, seria possível aplicar algoritmos de clusterização
(K-Means) para identificar perfis de deputados, ou treinar modelos de detecção
de anomalias para sinalizar gastos suspeitos automaticamente.
