[![IFPUG Member](https://img.shields.io/badge/IFPUG-Member-blue)](https://www.ifpug.org)
# 📌 Covertendo o texto de uma EF em APF

## 📊 EF do PJ#1427 (Desatualizada)

```
Requisitos Funcionais (RF)

RF-01. Mapear o novo meio de pagamento "C" Preparar o Sotreqlink Interno para receber boletos FIDC (Tipo C), de acordo com as mudanças da interface com o SAP.

O SAP identificará a partida memo referenciada ao boleto “Tipo C”, que caracteriza o boleto FIDC.   
• As particularidades da alteração na interface com o SAP estão descritas no documento “RT-SOT-2020-01’2-25004-PJ1427-FIDCSprint05
EspecificacaoFuncional SAP”. 
• Permitir que o sistema receba a alteração da forma de pagamento exclusivamente de “C” para “A”.  
• Ao receber um pagador diferente do inicial, o sistema deve manter o pagador inicial.

RF-02. Criar template de e-mail para boletos FIDC 

Um novo template de e-mail deve ser criado para exibir mensagem nos boletos FIDC. 

Nos boletos FIDC, o sistema deve exibir uma mensagem informativa no campo instruções. A mensagem será informada pelo time de negócios em tempo de desenvolvimento.  
• Não haverá QR Code para boletos FIDC. O pagamento será exclusivamente via código de barras.  Os cenários descritos no requisito serão validados em tempo de desenvolvimento.

RF-03. Incluir boletos FIDC no fluxo de envio de e-mails automáticos

O sistema deve enviar automaticamente o boleto para o e-mail do cliente.  
• Boletos com meio de pagamento “C” serão enviados automaticamente para o cliente através de e-mail cadastrado no SAP (ZFIN). 
• Boletos com meio de pagamento “C”, assim que criados no SAP e sincronizados com o Sotreqlink (Painel Financeiro), serão enviados para o cliente via e-mail.  
• O fluxo de envio dos demais tipos de boletos não será alterado. 
• Na falta de e-mail cadastrado, o Sotreqlink interno manterá o registro do não envio de e-mail por falta de contato na tabela BILLET_SEND_FAILURE, conforme regra atual.

RF-04. Novo layout para boleto FIDC 

O sistema deve ser alterado para exibir mensagem nos boletos FIDC.

Nos boletos FIDC, o sistema deve exibir uma mensagem informativa no campo instruções. A mensagem será informada pelo time de negócios em tempo de desenvolvimento.  
• Não haverá QR Code para boletos FIDC. O pagamento será exclusivamente via código de barras.

Comportamentos de tela (CT):

CT-4.1. 
Ao acessar “Painel Financeiro”, “Boletos” e o ícone na coluna “Pré-visualizar”, se FIDC, então o sistema exibirá um boleto com a mensagem definida pela área de negócio.  

RF-05. Incluir “Tipo C” na pesquisa do Painel Financeiro 
O sistema deve ser alterado para exibir no filtro Tipo de Pagamento o “Tipo C”, que corresponde aos boletos FIDC.

Comportamento de Tela (CT):

CT-5.1. 
Após preencher o filtro tipo de pagamento com “Tipo C” e selecionar o botão Buscar, deverá ser exibida a lista de boletos que correspondem a configuração inserida pelo usuário. 

CT-5.2. 
Após preencher o filtro tipo de pagamento com “Todos” e selecionar o botão 

Buscar, deverá ser exibida a lista de boletos de todos os tipos, incluindo o “Tipo C”.  

CT-5.3. 
Se não houver dados para os filtros inseridos, o sistema deverá exibir a mensagem “Sem dados para o filtro buscado”, conforme comportamento atual.

RF-06. Exibir boleto FIDC no Painel Financeiro

O sistema deverá exibir boletos “Tipo C” no resultado da consulta do Painel Financeiro. 
• Os boletos serão exibidos conforme as regras atuais do painel financeiro.  
• O sistema deverá identificar, através das colunas existentes atualmente, se o boleto está vencido e/ou compensado.  
• Se o boleto estiver compensado, não permitir o reenvio, conforme comportamento atual.  
• Em caso de devolução ou cancelamento, o boleto deixará de ser exibido na consulta do Painel Financeiro, conforme comportamento atual.
```

## 🧩 Consolidação RF ↔ CT ↔ FT/FD com DERs/RLRs + Tipo de Função

| RF ID  | PFs Estimado | CTs Relacionadas         | PFs via CTs | FT/FD Envolvidas                                 | Tipo FT | Tipo FD | PFs por FT/FD | DERs / RLRs     | Situação             |
|:------:|:-------------:|:------------------------:|:-----------:|:------------------------------------------------:|:-------:|:--------:|:--------------:|:----------------:|:--------------------:|
| RF-01  |      20       | —                        |     —       | FT-01: Receber boletos do SAP (Tipo C)           |   EE    |   —     |      10       | 6 DERs / 2 RLRs  | ✅ Sem CT            |
|        |               |                          |             | FD-01: Boletos FIDC                              |   —     |  AIE    |      10       | 10 DERs / 2 RLRs |                      |
| RF-02  |      10       | —                        |     —       | FT-02: Criar template de e-mail FIDC             |   EE    |   —     |       5       | 5 DERs / 2 RLRs  | ✅ Sem CT            |
|        |               |                          |             | FD-02: Template de e-mail FIDC                   |   —     |  ALI    |       5       | 7 DERs / 2 RLRs  |                      |
| RF-03  |      14       | —                        |     —       | FT-03: Enviar boleto FIDC por e-mail             |   SE    |   —     |       6       | 6 DERs / 2 RLRs  | ✅ Sem CT            |
|        |               |                          |             | FD-03: Registro falha envio (BILLET_SEND_FAILURE)|   —     |  ALI    |       8       | 8 DERs / 2 RLRs  |                      |
| RF-04  |      6        | CT-4.1                   |     6       | FD-02: Layout de boleto FIDC                     |   SE    |  ALI    |       6       | — (já contados)  | ✅ CT direta         |
| RF-05  |      9        | CT-5.1 / 5.2 / 5.3       |     9       | FT-04: Filtro por tipo de pagamento “C”          |   CE    |   —     |       9       | 6 DERs / 1 RLR   | ✅ CT fragmentada     |
| RF-06  |      5        | —                        |     —       | FT-05: Exibir status e controle de reenvio       |   CE    |   —     |       5       | 6 DERs / 2 RLRs  | ✅ Sem CT explícita  |

---

### 📘 Resumo

- **Total de PFs estimado**: `64 PFs`
- **Total de PFs via CTs (CT-4.1, CT-5.1~5.3)**: `15 PFs`
- **Funções de Transação (FT)**:
  - EE (Entradas Externas): `2`
  - SE (Saídas Externas): `2`
  - CE (Consultas Externas): `2`
- **Funções de Dados (FD)**:
  - **ALI** (Arquivo Lógico Interno): `2` → Template de e-mail + registro de falhas
  - **AIE** (Arquivo de Interface Externa): `1` → Boletos FIDC vindos do SAP

# 📌 Estimativa de Esforço com Base em Pontos de Função

## 🔧 Parâmetros da estimativa
- **Baixa produtividade**: 2 horas por PF
- **Escopo em refinamento**: Fator de risco de 20%
- **Distribuição por tipo de PF e por RF**
- **Categorização por perfil profissional**

---

## 💪 Esforço total base

- **PFs totais**: 64
- **Horas base (2h/PF)**: 128 horas
- **Fator de risco (20%)**: 25.6 horas
- 🎯 **Esforço total estimado**: 153.6 horas
- **Arredondado**: ~154 horas

---

## 💡 Distribuição por tipo de função

| Tipo de PF | Qtd PFs | Horas (2h/PF) | % do total | Perfil predominante            |
|------------|---------|----------------|------------|-------------------------------|
| EE         | 20      | 40 h           | 31.25%     | Backend + Integração          |
| SE         | 12      | 24 h           | 18.75%     | Backend + Frontend + QA       |
| CE         | 14      | 28 h           | 21.88%     | Frontend + QA                 |
| ALI        | 16      | 32 h           | 25.00%     | Backend + DB/Modelagem        |
| AIE        | 2       | 4 h            | 3.12%      | Integração externa (SAP)      |

| **Total**  | **64 PFs** | **128 h**    | **100%**   | —                             |

---

## 📂 Distribuição por Requisito Funcional

| RF ID  | PFs | Horas | Perfil predominante                                     |
|--------|-----|-------|---------------------------------------------------------|
| RF-01  | 20  | 40 h  | Backend + Integração SAP + Análise funcional            |
| RF-02  | 10  | 20 h  | Backend + Modelagem de Template + Frontend              |
| RF-03  | 14  | 28 h  | Backend + Infraestrutura de e-mail + QA                 |
| RF-04  | 6   | 12 h  | Frontend (layout do boleto) + Análise funcional         |
| RF-05  | 9   | 18 h  | Frontend (filtro, busca e lista) + QA                   |
| RF-06  | 5   | 10 h  | Backend + Regras de reenvio e exibição condicional      |

| **Total** | **64 PFs** | **128 h** | —                                                 |

---

## 🧮 Esforço final com risco aplicado

- Esforço base: 128 h
- Fator de risco: +25.6 h (20%)
- 🎯 **Esforço total estimado: 153.6 horas**
- ✅ Arredondando: **~154 horas**

---

# 📌 Próximos passos

## 💰 Levantamento de Custos

- Definir "Carga H" por tipo de profissional (Dev, QA, PO etc.)
- Definr "Custo HH" por tipo de profissional (Fonte Externa 1)
- Calcular custo total para o Esforço Total ([Custo HH Dev * Carga H Dev] + [Custo HH QA * Carga H QA] + [Custo HH PO * Carga H PO] + etc)

## ⏳ Levantamento de Prazos

- Mapear "Esforço por fase/profissional" ([desenvolvimento, testes, homologação] x [Dev, QA, PO, etc]), conforme % de esforço por histórico IFPUG
- Definir "% Alocação" por tipo de profissional por fase/sprint (Fonte Externa 2)
- Cacular "Prazo por profissional por fase/sprint" ([% Alocação] * [Esforço por fase/profissional])
- Estabelecer nível de paralelismo entre alocações de profissionais por fase/sprint
- Calcular os prazos por fase

## 📐 Validar PoC e estabelecer Prototipagem (incluindo padronização de coleta de requisitos)




