---
last-redoc-date: 2025-10-28
---

# Módulo Embrapa I/O

**Purpose**: Módulo de conhecimento BMAD que garante conformidade de aplicações com a plataforma Embrapa I/O através de workflows de setup, templates adaptáveis, e validação rigorosa contra 38 regras específicas.

**Overview**: Este módulo de conhecimento não cria aplicações diretamente - ele instrui outros agentes BMAD sobre como gerar projetos conformes com a plataforma Embrapa I/O independente de linguagem ou framework. Fornece workflows reutilizáveis para geração de arquivos essenciais (`.env.io`, `docker-compose.yaml`, `.embrapa/settings.json`, `LICENSE`), templates especializados por stack tecnológica (Node.js, Vue.js, React, PHP Laravel, .NET), e sistema completo de validação baseado nas 4 Verdades Fundamentais da plataforma.

O diferencial está na abordagem agnóstica de stack: detecta automaticamente a tecnologia utilizada e adapta configurações apropriadas sem necessidade de módulos específicos por linguagem. Workflows podem ser invocados por qualquer agente BMAD durante criação ou adaptação de projetos, garantindo que todas as aplicações sigam padrões rigorosos de nomenclatura, estruturação Docker, variáveis de ambiente, e integrações de serviços (Sentry, Matomo, healthchecks).

## Workflows

Ver [workflows/](./workflows/) para documentação completa.

**Total**: 5 workflows operacionais organizados em 2 categorias.

### Setup (4 workflows)
Workflows para configuração inicial de projetos conformes:
- **[generate-env-io](./workflows/setup/generate-env-io/)**: Gera `.env.io` e `.env.io.example` com as 10 variáveis obrigatórias da plataforma
- **[generate-docker-compose](./workflows/setup/generate-docker-compose/)**: Cria `docker-compose.yaml` conforme com as 4 Verdades Fundamentais
- **[generate-settings-json](./workflows/setup/generate-settings-json/)**: Gera `.embrapa/settings.json` com metadados e configurações por ambiente
- **[generate-license](./workflows/setup/generate-license/)**: Cria arquivo `LICENSE` padrão da Embrapa com copyright atualizado

### Validate (1 workflow)
Workflow de validação abrangente:
- **[validate-compliance](./workflows/validate/validate-compliance/)**: Validação completa contra 38 regras (docker-compose, env vars, settings, integrations)

## Configuration

O arquivo `config.yaml` define configurações padrão do módulo:

```yaml
user_name: Camilo Carromeu
communication_language: Brazilian Portuguese
document_output_language: Brazilian Portuguese
output_folder: '{project-root}/docs'
```

Estas configurações são referenciadas por todos os workflows através de `{config_source}:variable_name`, garantindo comunicação consistente em português brasileiro e outputs centralizados no diretório `docs/`.

## Usage

### Setup Completo de Novo Projeto

Executar workflows setup em sequência para criar estrutura completa:

```xml
<!-- 1. Gerar variáveis de ambiente (obrigatório primeiro) -->
<invoke-workflow>
  <path>{project-root}/bmad/embrapa-io/workflows/setup/generate-env-io/workflow.yaml</path>
</invoke-workflow>

<!-- 2. Gerar licença -->
<invoke-workflow>
  <path>{project-root}/bmad/embrapa-io/workflows/setup/generate-license/workflow.yaml</path>
</invoke-workflow>

<!-- 3. Gerar settings (requer .env.io) -->
<invoke-workflow>
  <path>{project-root}/bmad/embrapa-io/workflows/setup/generate-settings-json/workflow.yaml</path>
</invoke-workflow>

<!-- 4. Gerar docker-compose (requer .env.io) -->
<invoke-workflow>
  <path>{project-root}/bmad/embrapa-io/workflows/setup/generate-docker-compose/workflow.yaml</path>
</invoke-workflow>
```

### Validar Projeto Existente

Executar validação completa para verificar conformidade:

```xml
<invoke-workflow>
  <path>{project-root}/bmad/embrapa-io/workflows/validate/validate-compliance/workflow.yaml</path>
</invoke-workflow>
```

### Pré-requisitos

- Projeto com estrutura básica (código-fonte presente)
- Para workflows setup: nenhum arquivo Embrapa I/O necessário
- Para workflow validate: arquivos `.env.io`, `docker-compose.yaml`, `.embrapa/settings.json` devem existir

## Knowledge Base

O diretório `knowledge/` contém 7 arquivos de documentação técnica carregados por workflows para validação e orientação:

- `embrapa-io-fundamentals.md`: 4 Verdades Fundamentais da plataforma
- `embrapa-io-validation.md`: 38 regras de validação organizadas por categoria
- `embrapa-io-workflows.md`: Padrões de adaptação por tipo de projeto
- `embrapa-io-deployment.md`: Processos de deployment e ambientes
- `embrapa-io-stacks.md`: Configurações específicas por stack tecnológica
- `embrapa-io-integrations.md`: Integrações Sentry, Matomo, healthchecks
- `embrapa-io-integration-guide.md`: Guia detalhado de integrações

## Templates

O diretório `templates/` fornece templates base reutilizáveis:

- **docker-compose/**: `base.yaml` - Template base para docker-compose.yaml
- **settings/**: 3 templates JSON especializados por stack (nodejs, frontend, base)

Workflows carregam estes templates automaticamente baseando-se em detecção de stack, adaptando configurações específicas sem intervenção manual.

## Project Structure

```
bmad/embrapa-io/
├── config.yaml               # Configurações do módulo
├── README.md                 # Esta documentação
├── ROADMAP.md                # Planejamento de features futuras
├── knowledge/                # Base de conhecimento (7 arquivos)
├── templates/                # Templates reutilizáveis
│   ├── docker-compose/
│   └── settings/
└── workflows/                # Workflows organizados por categoria
    ├── setup/                # 4 workflows de configuração
    └── validate/             # 1 workflow de validação
```

## Compliance & Quality

**BMAD v6 Compliance**: 100% (auditado em 2025-10-28)
- Zero issues críticas
- Zero issues importantes
- Zero bloat
- Todas as 5 workflows em conformidade perfeita
