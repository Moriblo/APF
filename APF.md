[![IFPUG Member](https://img.shields.io/badge/IFPUG-Member-blue)](https://www.ifpug.org)

# Análise de Ponto de Função (APF)

📌 São dois tipos de função: Funções de Transação e Funções de Dados

## 🛠️ Funções de Transação:

### Entrada Externa (EE)
### Saída Externa (SE)
### Consulta Externa (CE)

## 🛠️ Funções de Dados:

### Arquivo Lógico Interno (ALI)
### Arquivo de Interface Externa (AIE)

## 🧠 Mapeamento Orientado a Objeto (OO) para Pontos de Função

📌 Na prática, quando usamos conceitos **orientados a objetos (OO)** para apoiar a contagem de **Pontos de Função**, fazemos uma equivalência conceitual entre elementos de modelagem e critérios da Análise de Pontos de Função:

| Conceito OO             | Equivalente na Análise de Pontos de Função       |
|-------------------------|--------------------------------------------------|
| **Classe**              | RLR (Tipo de Registro Lógico)                    |
| **Atributos da classe** | DER (Elemento de Dados Referenciado)            |
| **Métodos da classe**   | Funções de Transação (EE, SE, CE)               |

✔️ Ou seja:

- Cada **classe com persistência lógica** (como `Cliente`) pode representar **um RLR**.
- Cada **atributo público ou relevante para o usuário** representa **um DER**.
- Cada **método que interage com dados** pode ser analisado como uma **função de transação** (por exemplo: incluir, consultar, alterar).

```
+----------------------+
|      Cidades         |   ⇨ RLR
+----------------------+
| - nomeCidade         |   ⇨ DER
| - estado             |   ⇨ DER
| - codigoIBGE         |   ⇨ DER
+----------------------+

         ▲
         │
 IncluiCidade()    ⇨ CE
 AlteraCidade()    ⇨ SE
 ExcluiCidade()    ⇨ SE
ConsultaCidade()   ⇨ EE
```

➡ Quando o sistema mantém a tabela de cidades (cria, altera, exclui), temos um Arquivo Lógico Interno:
```
Arquivo Lógico Interno — CIDADES   ⇨ ALI
---------------------------------
+----------------------+              
|      Cidades         | ⇨ RLR
+----------------------+
| - nomeCidade         | ⇨ DER
| - estado             | ⇨ DER
| - codigoIBGE         | ⇨ DER
+----------------------+

           ▲
           │
+------------------------------+
| Funções de Transação         |
+------------------------------+
| IncluiCidade()         ⇨ CE  |
| AlteraCidade()         ⇨ SE  |
| ExcluiCidade()         ⇨ SE  |
| ConsultaCidade()       ⇨ EE  |
+------------------------------+
---------------------------------
```
🔹 O sistema tem total domínio sobre os dados da tabela Cidades: todas as funções de transação são válidas dentro do ALI.

➡ Quando a tabela é mantida externamente, e o sistema apenas consulta — temos um Arquivo de Interface Externa:
```
Arquivo de Interface Externa — CIDADES   ⇨ AIE
---------------------------------
+----------------------+    
|      Cidades         | ⇨ RLR
+----------------------+       
| - nomeCidade         | ⇨ DER
| - estado             | ⇨ DER
| - codigoIBGE         | ⇨ DER
+----------------------+
---------------------------------
           ▲
           │
+---------------------------------+
| Funções de Transação            |
+---------------------------------+
| GET/ConsultaCidade()     ⇨ EE  |
| POST/EnviaCidadeViaAPI() ⇨ CE  |
+---------------------------------+
```
🔹 A função SE não existe, pois o sistema não controla esse dado — apenas o consome como referência.

## 🧮 Tabelas de Complexidade por Tipo de Função

📌 As tabelas de pontuação são definidas combinando a quantidade de DERs e RLRs com critérios de complexidade (Baixa, Média, Alta), resultando em uma atribuição de pontos de função conforme a natureza e o volume da operação.

### Funções de Transação:

#### 🔷 Entrada Externa (EE)
| DERs   | RLRs | Complexidade | Pontos de Função |
|--------|------|--------------|------------------|
| 1–4    | 0–1  | Baixa        | 3                |
| 5–15   | 2    | Média        | 4                |
| >15    | >2   | Alta         | 6                |

#### 🔷 Saída Externa (SE)
| DERs   | RLRs | Complexidade | Pontos de Função |
|--------|------|--------------|------------------|
| 1–5    | 0–1  | Baixa        | 4                |
| 6–19   | 2–3  | Média        | 5                |
| >19    | >3   | Alta         | 7                |

#### 🔷 Consulta Externa (CE)
| DERs   | RLRs | Complexidade | Pontos de Função |
|--------|------|--------------|------------------|
| 1–5    | 0–1  | Baixa        | 3                |
| 6–19   | 2–3  | Média        | 4                |
| >19    | >3   | Alta         | 6                |

📌 **Quando os critérios de DERs e RLRs apontarem para níveis diferentes, predomina o menor nível de complexidade (IFPUG)**.

🔍 Por que a SE tem pontuação maior?
- A SE (Saída Externa) geralmente envolve processamento adicional antes da apresentação.
- Exemplos incluem: gerar um relatório, calcular valores derivados, aplicar regras de formatação, etc.
- Por isso, mesmo na complexidade baixa, ela já parte de 4 PFs — diferente da EE e CE que começam com 3 PFs.

📎 Por que CE e EE têm mesma pontuação?
- A EE altera o estado interno do sistema (ex: incluir ou atualizar dados), enquanto a CE apenas consulta.
- Embora o impacto técnico seja diferente, o esforço de desenvolvimento e testes para uma transação de leitura (CE) pode ser próximo ao de uma entrada simples (EE) — daí a pontuação semelhante.

#### ⚖️ Tabela Comparativa de Pontuação Base
| Tipo | Baixa | Média | Alta |
|------|-------|-------|------|
| EE   | 3 PFs | 4 PFs | 6 PFs |
| SE   | 4 PFs | 5 PFs | 7 PFs |
| CE   | 3 PFs | 4 PFs | 6 PFs |

🔸 A SE sempre pontua mais pela complexidade potencial de processamento — é a função mais “cara” em termos de PF.

### Funções de Dados:

#### 🔷 Arquivo Lógico Interno (ALI)
| DERs   | RLRs | Complexidade | Pontos de Função |
|--------|------|--------------|------------------|
| 1–19   | 1    | Baixa        | 7                |
| 20–50  | 2–5  | Média        | 10               |
| >50    | >5   | Alta         | 15               |

#### 🔷 Arquivo de Interface Externa (AIE)
| DERs   | RLRs | Complexidade | Pontos de Função |
|--------|------|--------------|------------------|
| 1–19   | 1    | Baixa        | 5                |
| 20–50  | 2–5  | Média        | 7                |
| >50    | >5   | Alta         | 10               |

📌 **Quando os critérios de DERs e RLRs apontarem para níveis diferentes, predomina o menor nível de complexidade (IFPUG)**.
  
🧩 **Quanto mais DERs e RLRs, maior a complexidade** do ALI ou do AIE. A diferença é que:

- Nos ALI, geralmente há mais controle sobre a estrutura interna dos dados, por isso eles têm uma pontuação maior.
- Nos AIE, como são externos, o sistema pode ter uma visão mais limitada, então a pontuação é um pouco menor.

🔎 Detalhes importantes:
- O ALI representa dados mantidos dentro do sistema, com estrutura própria e persistência. Por isso, a pontuação é mais elevada — envolve mais análise, modelagem e controle de integridade.
- O AIE são dados mantidos em outro sistema, mas referenciados pelo sistema em análise. O esforço é menor, pois não há controle direto sobre a manutenção dos dados.

#### ⚖️ Tabela Comparativa de Pontuação Base – ALI e AIE
| Tipo | Baixa | Média | Alta |
|------|-------|-------|------|
| ALI  | 7 PFs | 10 PFs| 15 PFs|
| AIE  | 5 PFs | 7 PFs | 10 PFs|

## Relação entre linhas de código (LOC) e pontos de função (PF)

A relação entre linhas de código (LOC) e pontos de função (PF) é complexa, mas existe uma forma de estimar essa conversão com base em fatores de produtividade históricos por linguagem de programação:

🔄 Relação entre Pontos de Função e Linhas de Código  
Os pontos de função medem o tamanho funcional do software (o que ele faz para o usuário), enquanto as linhas de código medem o tamanho técnico (como ele é implementado). Como são dimensões diferentes, a conversão exige fatores de equivalência por linguagem.

📐 Fórmula estimada:  
Pontos de Função ≈ Linhas de Código / Fator de Conversão da Linguagem

📊 Tabela de Fatores de Conversão (estimativas médias)  
| Linguagem         | Linhas por Ponto de Função (LOC/PF) | Pontos de Função por 1000 LOC |
|-------------------|--------------------------------------|-------------------------------|
| Assembly          | 320                                  | ~3                            |
| C                 | 128                                  | ~8                            |
| Java              | 53                                   | ~19                           |
| C# / .NET         | 50                                   | ~20                           |
| Python            | 42                                   | ~24                           |
| Ruby              | 42                                   | ~24                           |
| JavaScript        | 47                                   | ~21                           |
| SQL (procedural)  | 13                                   | ~77                           |

Fonte: estimativas baseadas em dados do ISBSG e IFPUG.

🧮 Exemplo prático  
Suponha que seu sistema tenha 25.000 linhas de código em Java:  
Pontos de Função ≈ 25.000 / 53 ≈ 472 PF

Se fosse em Python:  
Pontos de Função ≈ 25.000 / 42 ≈ 595 PF

⚠️ Importante considerar  
- Esses valores são estimativas médias. Projetos com muita lógica de negócio ou interface complexa podem ter mais PF por LOC.  
- O uso de frameworks, bibliotecas e geração automática de código pode distorcer a relação.  
- Para maior precisão, o ideal é fazer uma contagem funcional real usando APF.

Questões a considerar:  
- Qual linguagem foi usada?  
- Quantas linhas de código aproximadamente?  
- O sistema é mais voltado para interface, processamento ou banco de dados?  

## PERFORMANCE

Medir a performance de um time de desenvolvimento em horas por ponto de função é uma abordagem bastante eficaz para avaliar produtividade, especialmente em ambientes que usam métricas funcionais como Análise de Pontos de Função (APF).

🧮 O que é "horas por ponto de função"?  
É uma métrica que indica quantas horas de trabalho são necessárias para entregar um ponto de função. Quanto menor esse número, maior a produtividade do time.

🛠️ Passo a passo para medir  

1. Estime ou conte os pontos de função  
- Use a técnica de Análise de Pontos de Função (APF) para medir o tamanho funcional do software.  
- Você pode usar métodos como IFPUG ou NESMA.  
- Ferramentas como Function Point Workbench ou softwares de APF podem ajudar.  

2. Registre as horas trabalhadas  
- Use ferramentas de timesheet ou controle de horas (Jira, Toggl, Harvest, etc.).  
- Considere apenas as horas diretamente relacionadas ao desenvolvimento (codificação, testes, revisão).  

3. Calcule a métrica  
Horas por ponto de função = Total de horas gastas / Total de pontos de função entregues  

4. Compare com benchmarks  
- **Segundo o IFPUG, a média de produtividade varia entre 10 a 30 horas por ponto de função, dependendo da complexidade, tecnologia e maturidade do time**.

📊 Exemplo prático  
| Projeto   | Pontos de Função | Horas Gastas | Horas/PF |
|-----------|------------------|--------------|----------|
| Sistema A | 120 PF           | 1800 h       | 15 h/PF  |
| Sistema B | 80 PF            | 2000 h       | 25 h/PF  |

🔍 Dicas para tornar a métrica útil  
- Segmente por tipo de projeto: Web, mobile, legado, etc.  
- Considere a curva de aprendizado: Times novos tendem a ter produtividade menor.  
- Use em conjunto com outras métricas: Qualidade, retrabalho, satisfação do cliente.

