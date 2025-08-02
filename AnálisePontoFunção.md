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

# Casos de Uso X APF

## 📘 Caso de Uso: [Nome do Caso de Uso]

### 🔹 Informações Gerais
- **Ator Primário:** [Ex: Vendedor]
- **Objetivo:** [Ex: Registrar um pedido de venda]
- **Pré-condição:** [Ex: Cliente deve existir no sistema]
- **Pós-condição:** [Ex: Pedido registrado com status “em aberto”]

---

### 🔄 Fluxo Principal

| Etapa | Tipo de Função | DERs envolvidos | RLRs envolvidos | Observações |
|-------|----------------|------------------|------------------|-------------|
| [Selecionar cliente] | EE | [ID do cliente, Nome] | [Cliente] | Entrada simples |
| [Escolher produtos] | EE | [Código, Quantidade, Preço] | [Itens] | Entrada com regras |
| [Visualizar total] | SE | [Valor Total, Impostos] | [Itens, Pagamento] | Exige cálculo |
| [Confirmar pagamento] | EE | [Tipo, Parcelamento] | [Pagamento] | Nova entrada |

---

### 📂 Funções de Dados Relacionadas

| Nome do Arquivo | Tipo (ALI/AIE) | DERs | RLRs | Complexidade | PFs estimado |
|------------------|----------------|------|------|--------------|---------------|
| Pedido | ALI | [ID, Data, Total, Cliente, Itens, Pagamento] | [Cliente, Itens, Pagamento] | Média | 10 |
| Produto | AIE | [Código, Nome, Preço, Estoque] | [Categoria] | Baixa | 5 |

---

### 📊 Contagem Final Estimada

| Função | Tipo | Complexidade | Pontuação |
|--------|------|--------------|-----------|
| Selecionar cliente | EE | Baixa | 3 |
| Inserir itens | EE | Média | 4 |
| Visualizar cálculo total | SE | Média | 5 |
| Confirmar pagamento | EE | Baixa | 3 |
| Pedido (dados internos) | ALI | Média | 10 |
| Produto (dados externos) | AIE | Baixa | 5 |

**🧮 Total estimado: 30 PFs**

---

### 📌 Observações Complementares
- [Insira aqui decisões ou extensões relevantes do Caso de Uso]
- [Observações específicas de stakeholders que influenciaram os requisitos]
