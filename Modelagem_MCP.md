# 📐 Modelo de distribuição de esforço por Skill e Fase

Baseado em boas práticas de engenharia de software, com referência em modelos como:

- **PMBOK (Project Management Body of Knowledge)**
- **SCRUM** aplicado em ambientes híbridos
- **Experiência empírica** na alocação de perfis técnicos em sistemas ERP (como SAP), backend, frontend e automação de comunicação

---

### 🧱 Fase 1: Desenvolvimento

**Objetivo:** Construção técnica das funcionalidades, integração com SAP/API, layout de boletos, e fluxo de envio automático.

| Skill                    | Justificativa                                        | % do esforço da fase |
|--------------------------|------------------------------------------------------|-----------------------|
| PO                       | Refinamento dos requisitos, interface com negócio    | 20%                   |
| Dev                      | Backend .NET + lógica de negócios                    | 30%                   |
| SAPDev                   | Ajustes na interface SAP                             | 30%                   |
| Frontend (React via Dev) | Layouts e painéis internos                           | 20%                   |

📊 **Esforço total da fase:** 40% do projeto

---

### 🧪 Fase 2: Testes

**Objetivo:** Validação funcional, envio de boletos, testes automáticos e manuais de integração SAP/API.

| Skill   | Justificativa                                      | % do esforço da fase |
|---------|----------------------------------------------------|-----------------------|
| Tester  | Testes manuais, automação, regressão               | 70%                   |
| PO      | Apoio nas validações                               | 10%                   |
| Dev     | Correções e suporte técnico durante testes         | 10%                   |
| SAPDev  | Validação da interface SAP                         | 10%                   |

📊 **Esforço total da fase:** 35% do projeto

---

### ✅ Fase 3: Homologação

**Objetivo:** Validação final com cliente, dados reais, mensagens de boletos e regras SAP/negócio.

| Skill    | Justificativa                                      | % do esforço da fase |
|----------|----------------------------------------------------|-----------------------|
| DataEng  | Verificação dos dados de boletos                   | 40%                   |
| SAPFunc  | Validação do comportamento funcional no SAP        | 40%                   |
| PO       | Apresentação para área de negócio                  | 10%                   |
| Tester   | Cenários finais e suporte de validação             | 10%                   |

📊 **Esforço total da fase:** 25% do projeto

---

### 🧮 Resumo de Boas Práticas Adotadas

- **Distribuição por complexidade funcional:** mais esforço onde há lógica de negócio integrada com SAP e API
- **Separação de perfis técnicos e funcionais:** desenvolvimento técnico (Dev, SAPDev), validação de dados (DataEng), e validação funcional (SAPFunc)
- **Presença do PO em todas as fases**, com maior peso no início e menor na homologação

# Modelagem MCP

## 📦 JSON MCP — Estimativa Técnica e Financeira

Este artefato serve como entrada para agentes generativos especializados em Function Points. Ele descreve o escopo técnico e financeiro de uma solução ERP, incluindo premissas de estimativa, stack tecnológica e custos por skill.

A IA deverá responder com um JSON MCO contendo volume de PF, esforço estimado, custo por fase e skill, e cronograma projetado.

```
{
  "mcp": {
    "caller": "pf_orchestrator_v1",
    "goal": "estimativa_pf_custo_completo",
    "session": "[INSERIR IDENTIFICADOR DO PROJETO OU SPRINT]",
    "step": "calculo_estimativa_final",
    "domain": "[INSERIR DOMÍNIO FUNCIONAL DO SISTEMA]",
    "instrucoes_ia": "[UTILIZAR INSTRUÇÕES PADRONIZADAS PARA MCO – ver versão abaixo]",
    "context": {
      "padrao_estimativa": "IFPUG",
      "produtividade_horasporPF": [HORAS POR PF],
      "risco_percentual": [PERCENTUAL DE RISCO],
      "stack": {
        "backend": "[TECNOLOGIA BACKEND]",
        "frontend": "[TECNOLOGIA FRONTEND]",
        "caracteristicas": ["API", "Integração", "[OUTROS]"]
      },
      "skills_env": ["PO", "DevFront", "DevBack", "Tester", "DataEng", "SAPDev", "SAPFunc"],
      "custos_por_hora": {
        "PO": [VALOR],
        "DevFront": [VALOR],
        "DevBack": [VALOR],
        "Tester": [VALOR],
        "DataEng": [VALOR],
        "SAPDev": [VALOR],
        "SAPFunc": [VALOR]
      },
      "fases": [
        {
          "nome": "Desenvolvimento",
          "proporcao_pf": [EX: 0.4],
          "distribuicao_skills": {
            "PO": [EX: 0.2],
            "DevBack": [EX: 0.3],
            "SAPDev": [EX: 0.3],
            "DevFront": [EX: 0.2]
          }
        },
        {
          "nome": "Testes",
          "proporcao_pf": [EX: 0.35],
          "distribuicao_skills": {
            "Tester": [EX: 0.7],
            "PO": [EX: 0.1],
            "DevBack": [EX: 0.1],
            "SAPDev": [EX: 0.1]
          }
        },
        {
          "nome": "Homologacao",
          "proporcao_pf": [EX: 0.25],
          "distribuicao_skills": {
            "DataEng": [EX: 0.4],
            "SAPFunc": [EX: 0.4],
            "PO": [EX: 0.1],
            "Tester": [EX: 0.1]
          }
        }
      ]
    },
    "payload": {
      "requisitos": [
        "[LISTAR OS REQUISITOS FUNCIONAIS E COMPORTAMENTOS DE TELA]",
        "[EX: RF-01. Permitir alteração da forma de pagamento para 'C']"
      ]
    }
  }
}
```
```
Aja como um estimador técnico especializado em Function Points, seguindo rigorosamente o padrão IFPUG. Com base nas premissas informadas neste MCP, gere um artefato MCO contendo:
- Volume estimado de PFs, separado por tipo de função (EE, SE, CE, ALI, AIE).
- Para cada função identificada: descrição objetiva, tipo da função, quantidade de PFs e justificativa técnica vinculada diretamente aos requisitos funcionais e comportamentos de tela.
- Esforço em horas e custo por fase e por skill, aplicando o risco conforme percentual informado.
- Exibir todos os dados em formato tabular.
- Não assumir dados derivados como pré-definidos — tudo deve ser calculado com base exclusivamente no MCP.
- Não executar atividades fora do escopo definido.
O objetivo é obter um MCO técnico, rastreável e auditável, com aderência total às práticas de análise por pontos de função.
```
