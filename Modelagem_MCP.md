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
    "session": "MOACYR-PJ1427-Sprint05",
    "step": "calculo_estimativa_final",
    "domain": "ERP.Financeiro.BoletosFIDC",
    "instrucoes_ia": "Aja como um estimador técnico especializado em Function Points. Com base nas premissas informadas, gere um artefato MCO contendo: volume estimado de PF, esforço em horas, custo por fase e por skill, e cronograma sugerido. Todos os cálculos devem considerar a aplicação de risco e respeitar a distribuição percentual informada por fase e skill. Não assuma dados derivados como pré-definidos — tudo deve ser calculado com base neste MCP.",
    "context": {
      "padrao_estimativa": "IFPUG",
      "produtividade_pf": 2,
      "risco_percentual": 20,
      "stack": {
        "backend": ".NET + C#",
        "frontend": "React",
        "caracteristicas": ["API", "SAP"]
      },
      "skills_env": ["PO", "Dev", "Tester", "DataEng", "SAPDev", "SAPFunc"],
      "custos_por_hora": {
        "PO": 100.0,
        "Dev": 110.0,
        "Tester": 120.0,
        "DataEng": 130.0,
        "SAPDev": 140.0,
        "SAPFunc": 150.0
      },
      "fases": [
        {
          "nome": "Desenvolvimento",
          "proporcao_pf": 0.4,
          "distribuicao_skills": {
            "PO": 0.2,
            "Dev": 0.3,
            "SAPDev": 0.3,
            "Frontend": 0.2
          }
        },
        {
          "nome": "Testes",
          "proporcao_pf": 0.35,
          "distribuicao_skills": {
            "Tester": 0.7,
            "PO": 0.1,
            "Dev": 0.1,
            "SAPDev": 0.1
          }
        },
        {
          "nome": "Homologacao",
          "proporcao_pf": 0.25,
          "distribuicao_skills": {
            "DataEng": 0.4,
            "SAPFunc": 0.4,
            "PO": 0.1,
            "Tester": 0.1
          }
        }
      ]
    },
    "payload": {
      "requisitos_funcionais": ["RF-01", "RF-02", "RF-03", "RF-04", "RF-05", "RF-06"],
      "comportamentos_tela": ["CT-4.1", "CT-4.2", "CT-5.1", "CT-5.2", "CT-5.3"]
    }
  }
}
```

