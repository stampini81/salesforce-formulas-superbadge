# Salesforce Formulas Superbadge

<p align="center">
  <img src="./assets/images/superbadge-formulas.svg" alt="Salesforce Formulas Superbadge" width="220" />
  <br />
  <img src="./assets/images/salesforce-logo.svg" alt="Salesforce logo" width="260" />
</p>

<p align="center">
  <a href="https://github.com/stampini81/salesforce-formulas-superbadge/actions/workflows/ci.yml">
    <img src="https://github.com/stampini81/salesforce-formulas-superbadge/actions/workflows/ci.yml/badge.svg" alt="CI status" />
  </a>
  <br />
  <img src="https://img.shields.io/badge/Salesforce-Superbadge-00A1E0?style=for-the-badge" alt="Salesforce Superbadge" />
  <img src="https://img.shields.io/badge/Metadata-Salesforce%20DX-0176D3?style=for-the-badge" alt="Salesforce DX" />
  <img src="https://img.shields.io/badge/Focus-Formulas%20%26%20Reports-5E5E5E?style=for-the-badge" alt="Formulas and Reports" />
</p>

Projeto criado para documentar a implementação do **Superbadge: Formulas** no Salesforce, com foco em fórmulas, visibilidade por permission set, layout de Asset e ajustes de relatório.

## Autor

**Leandro da Silva Stampini**

## Status do CI

O projeto conta com uma pipeline no GitHub Actions para validar a instalação das dependências, verificar a formatação dos arquivos e conferir a presença dos principais metadados do superbadge. O indicador acima mostra automaticamente se a execução mais recente da esteira passou ou falhou.

## Destaques

- Implementação completa do desafio com metadados organizados em Salesforce DX
- Fórmulas reutilizáveis aplicadas em `Lead`, `Asset` e `Case`
- Controle de acesso seguindo o princípio do menor privilégio
- Atualização de relatório com lógica de negócio para análise operacional
- Projeto estruturado para estudo, portfólio e reaproveitamento em outras orgs

## Objetivo

Este repositório reúne a estrutura do projeto Salesforce DX e os metadados utilizados para atender aos requisitos do superbadge, incluindo:

- pontuação automática de leads
- classificação visual por estrelas
- exibição do valor da oportunidade no Asset sem acesso direto ao objeto `Opportunity`
- conversão da severidade do Case para valor numérico reutilizável
- atualização do relatório de severidade com lógica de negócio

## O que foi implementado

| Área | Implementação |
| --- | --- |
| Lead | `Lead_Score__c` e `Lead_Rating__c` |
| Asset | `Opportunity_Value__c` no layout do ativo |
| Case | `Severity_Number__c` para cálculos e média |
| Segurança | visibilidade por `Sales_Representative` e `Service_Agent` |
| Relatórios | atualização do relatório com `Close Month` e média de severidade |

### Lead

- Campo de fórmula `Lead_Score__c`
  - Calcula a pontuação do lead com base em:
    - `Status`
    - `DoNotCall`
    - `Email`
    - `LeadSource`
- Campo de fórmula `Lead_Rating__c`
  - Exibe classificação visual por estrelas com `IMAGE()`
  - Inclui texto alternativo para acessibilidade

### Asset

- Campo de fórmula `Opportunity_Value__c`
  - Recupera o valor da oportunidade relacionada por meio de `Opportunity__r.Amount`
- Inclusão do campo no layout `Asset-Asset Layout`

### Case

- Campo de fórmula `Severity_Number__c`
  - Converte o picklist `Severity__c` em número para uso em relatórios e médias

### Segurança e acesso

- Permission set `Sales_Representative`
  - acesso aos campos:
    - `Lead.Lead_Score__c`
    - `Lead.Lead_Rating__c`
- Permission set `Service_Agent`
  - acesso aos campos:
    - `Asset.Opportunity_Value__c`
    - `Case.Severity_Number__c`

### Relatório

Relatório atualizado:

- `Case Severity By Month Last Year`

Ajustes aplicados:

- uso de `Severity_Number__c`
- agregação por média
- fórmula de linha `Close Month`
- agrupamento por:
  - `Close Month`
  - `Account Name`

## Resultado

Este projeto demonstra, na prática, habilidades importantes de administração e configuração no Salesforce:

- criação de fórmulas com lógica condicional
- aplicação de boas práticas de descrição e help text
- configuração de Field-Level Security com permission sets
- uso de cross-object formula fields
- adaptação de relatórios para atender requisitos de negócio e compliance

Ele pode ser usado tanto como material de estudo quanto como peça de portfólio para demonstrar domínio em automação declarativa no ecossistema Salesforce.

## Estrutura principal

```text
force-app/main/default/
  layouts/
  objects/
    Asset/fields/
    Case/fields/
    Lead/fields/
  permissionsets/
  reports/
```

## Arquivos relevantes

- [`Lead_Score.field-meta.xml`](./force-app/main/default/objects/Lead/fields/Lead_Score.field-meta.xml)
- [`Lead_Rating.field-meta.xml`](./force-app/main/default/objects/Lead/fields/Lead_Rating.field-meta.xml)
- [`Opportunity_Value.field-meta.xml`](./force-app/main/default/objects/Asset/fields/Opportunity_Value.field-meta.xml)
- [`Severity_Number.field-meta.xml`](./force-app/main/default/objects/Case/fields/Severity_Number.field-meta.xml)
- [`Asset-Asset Layout.layout-meta.xml`](./force-app/main/default/layouts/Asset-Asset%20Layout.layout-meta.xml)
- [`Sales_Representative.permissionset-meta.xml`](./force-app/main/default/permissionsets/Sales_Representative.permissionset-meta.xml)
- [`Service_Agent.permissionset-meta.xml`](./force-app/main/default/permissionsets/Service_Agent.permissionset-meta.xml)
- [`Case_Severity_By_Month_Last_Year_ReM.report-meta.xml`](./force-app/main/default/reports/ComplianceReports/Case_Severity_By_Month_Last_Year_ReM.report-meta.xml)

## Tecnologias e recursos

- Salesforce CLI / SFDX
- Salesforce Metadata API
- Salesforce DX source format
- Permission Sets
- Formula Fields
- Report metadata
- GitHub Actions

## Como usar

### Pré-requisitos

- Salesforce CLI
- uma org Salesforce autenticada

### Deploy

Exemplo de deploy por source:

```bash
sfdx force:source:deploy -p force-app/main/default -u <alias-ou-username-da-org>
```

Exemplo de deploy via Metadata API:

```bash
sfdx force:mdapi:deploy -d mdapiout -u <alias-ou-username-da-org> -w 15
```
