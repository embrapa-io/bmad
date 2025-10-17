# Roadmap de Componentes - Módulo Embrapa I/O

**Versão do Módulo**: 1.0.0
**Gerado em**: 2025-10-06
**Status Geral**: 🟡 Em Desenvolvimento (25% completo)

## 📊 Visão Geral do Progresso

| Categoria | Total | Completos | Pendentes | % Completo |
|-----------|-------|-----------|-----------|------------|
| **Conhecimento** | 6 | 6 | 0 | 100% ✅ |
| **Templates** | 20 | 4 | 16 | 20% 🟡 |
| **Workflows** | 18 | 1 | 17 | 6% 🔴 |
| **Tasks** | 8 | 0 | 8 | 0% 🔴 |
| **Config & Docs** | 3 | 3 | 0 | 100% ✅ |
| **TOTAL** | 55 | 14 | 41 | 25% 🟡 |

---

## 🎯 Fase 1: Core Validation (Prioridade ALTA)

**Objetivo**: Sistema de validação completo e funcional

**Prazo Estimado**: 4 semanas

**Componentes**: 15 itens

### ✅ Completos (6/15)

- [x] knowledge/embrapa-io-fundamentals.md
- [x] knowledge/embrapa-io-validation.md
- [x] knowledge/embrapa-io-workflows.md
- [x] workflows/validate/validate-compliance/workflow.yaml
- [x] workflows/validate/validate-compliance/instructions.md
- [x] config.yaml

### 🔨 Em Desenvolvimento (9/15)

#### Workflows VALIDATE (5 workflows)

- [ ] **workflows/validate/validate-docker-compose/**
  - [ ] workflow.yaml
  - [ ] instructions.md
  - **Prioridade**: ALTA
  - **Depende de**: Task validate-docker-compose.xml
  - **Tempo estimado**: 3 dias

- [ ] **workflows/validate/validate-env-files/**
  - [ ] workflow.yaml
  - [ ] instructions.md
  - **Prioridade**: ALTA
  - **Depende de**: Task validate-env.xml
  - **Tempo estimado**: 2 dias

- [ ] **workflows/validate/validate-settings/**
  - [ ] workflow.yaml
  - [ ] instructions.md
  - **Prioridade**: ALTA
  - **Depende de**: Task validate-settings.xml
  - **Tempo estimado**: 2 dias

- [ ] **workflows/validate/validate-integrations/**
  - [ ] workflow.yaml
  - [ ] instructions.md
  - **Prioridade**: ALTA
  - **Depende de**: Análise de código fonte
  - **Tempo estimado**: 3 dias

- [ ] **workflows/validate/generate-compliance-report/**
  - [ ] workflow.yaml
  - [ ] instructions.md
  - **Prioridade**: ALTA
  - **Depende de**: Task generate-compliance-score.xml
  - **Tempo estimado**: 2 dias

#### Tasks VALIDATION (4 tasks)

- [ ] **tasks/validation/validate-docker-compose.xml**
  - Implementar 14 validações
  - Retornar erros estruturados
  - **Prioridade**: ALTA
  - **Tempo estimado**: 4 dias

- [ ] **tasks/validation/validate-env.xml**
  - Implementar 8 validações
  - Detectar duplicação de variáveis
  - **Prioridade**: ALTA
  - **Tempo estimado**: 3 dias

- [ ] **tasks/validation/validate-settings.xml**
  - Implementar 8 validações
  - Validar JSON schema
  - **Prioridade**: ALTA
  - **Tempo estimado**: 3 dias

- [ ] **tasks/validation/generate-compliance-score.xml**
  - Calcular score baseado em severidades
  - Gerar relatórios JSON e Markdown
  - **Prioridade**: ALTA
  - **Tempo estimado**: 2 dias

**Total Fase 1**: 16 dias úteis (~3 semanas)

---

## 🚀 Fase 2: Scaffolding Node.js (Prioridade ALTA)

**Objetivo**: Criar projetos Node.js API conformes do zero

**Prazo Estimado**: 3 semanas

**Componentes**: 8 itens

### 🔨 Pendentes (8/8)

#### Templates (2)

- [ ] **templates/docker-compose/nodejs-api.yaml**
  - Services: api, db (MongoDB/PostgreSQL), backup, restore, sanitize
  - Healthchecks completos
  - **Prioridade**: ALTA
  - **Tempo estimado**: 2 dias

- [ ] **templates/env/.env.nodejs.example**
  - Variáveis específicas Node.js + Express
  - Variáveis de banco de dados
  - **Prioridade**: ALTA
  - **Tempo estimado**: 1 dia

#### Workflows CREATE (1)

- [ ] **workflows/create/create-nodejs-api/**
  - [ ] workflow.yaml
  - [ ] instructions.md
  - [ ] templates/ (código base)
    - [ ] src/index.js
    - [ ] src/routes/api.routes.js
    - [ ] src/config/sentry.js
    - [ ] src/config/matomo.js
    - [ ] package.json
    - [ ] Dockerfile
  - **Prioridade**: ALTA
  - **Depende de**: Tasks scaffolding
  - **Tempo estimado**: 5 dias

#### Tasks SCAFFOLDING (3)

- [ ] **tasks/scaffolding/scaffold-project-structure.xml**
  - Criar estrutura de diretórios
  - Copiar templates base
  - **Prioridade**: ALTA
  - **Tempo estimado**: 2 dias

- [ ] **tasks/scaffolding/apply-template.xml**
  - Substituir variáveis em templates
  - Aplicar configurações específicas
  - **Prioridade**: ALTA
  - **Tempo estimado**: 2 dias

- [ ] **tasks/scaffolding/configure-integrations.xml**
  - Instalar SDKs (Sentry, Matomo)
  - Gerar código de configuração
  - **Prioridade**: ALTA
  - **Tempo estimado**: 3 dias

**Total Fase 2**: 15 dias úteis (~3 semanas)

---

## 🎨 Fase 3: Frontend Scaffolding (Prioridade MÉDIA)

**Objetivo**: Criar projetos Vue.js e React conformes

**Prazo Estimado**: 3 semanas

**Componentes**: 8 itens

### 🔨 Pendentes (8/8)

#### Templates (4)

- [ ] **templates/docker-compose/vuejs-frontend.yaml**
- [ ] **templates/env/.env.vuejs.example**
- [ ] **templates/docker-compose/react-frontend.yaml**
- [ ] **templates/env/.env.react.example**
- **Prioridade**: MÉDIA
- **Tempo estimado**: 2 dias cada (8 dias total)

#### Workflows CREATE (2)

- [ ] **workflows/create/create-vuejs-frontend/**
  - Código base Vue.js 3 + Vuetify
  - Tema verde Embrapa
  - Logo integrada
  - **Prioridade**: MÉDIA
  - **Tempo estimado**: 4 dias

- [ ] **workflows/create/create-react-frontend/**
  - Código base React 18
  - UI library configurada
  - **Prioridade**: MÉDIA
  - **Tempo estimado**: 3 dias

#### Templates de Integração (2)

- [ ] **templates/integrations/sentry-config.js**
  - Configuração para Node.js, Vue.js, React
  - **Prioridade**: MÉDIA
  - **Tempo estimado**: 1 dia

- [ ] **templates/integrations/matomo-tracking.js**
  - Tracking code para frontends
  - Custom dimensions
  - **Prioridade**: MÉDIA
  - **Tempo estimado**: 1 dia

**Total Fase 3**: 15 dias úteis (~3 semanas)

---

## ➕ Fase 4: Add Compliance Workflows (Prioridade MÉDIA)

**Objetivo**: Adicionar conformidade a projetos existentes

**Prazo Estimado**: 3 semanas

**Componentes**: 9 itens

### 🔨 Pendentes (9/9)

#### Workflows ADD (5)

- [ ] **workflows/add/add-embrapa-compliance/**
  - Analisar projeto existente
  - Gerar docker-compose conforme
  - Criar .env files sem conflitos
  - **Prioridade**: MÉDIA
  - **Tempo estimado**: 5 dias

- [ ] **workflows/add/add-sentry-integration/**
  - Detectar stack
  - Instalar SDK apropriado
  - Configurar código
  - **Prioridade**: MÉDIA
  - **Tempo estimado**: 3 dias

- [ ] **workflows/add/add-matomo-integration/**
  - Frontend ou backend
  - Implementar tracking
  - **Prioridade**: MÉDIA
  - **Tempo estimado**: 2 dias

- [ ] **workflows/add/add-sonarqube-integration/**
  - Criar sonar-project.properties
  - Configurar CI/CD
  - **Prioridade**: BAIXA
  - **Tempo estimado**: 2 dias

- [ ] **workflows/add/add-loki-integration/**
  - Configurar logging driver
  - Definir labels
  - **Prioridade**: BAIXA
  - **Tempo estimado**: 2 dias

#### Tasks MIGRATION (3)

- [ ] **tasks/migration/analyze-existing-project.xml**
  - Detectar stack, banco, estrutura
  - **Prioridade**: MÉDIA
  - **Tempo estimado**: 3 dias

- [ ] **tasks/migration/generate-migration-plan.xml**
  - Plano de correções priorizadas
  - **Prioridade**: MÉDIA
  - **Tempo estimado**: 2 dias

- [ ] **tasks/migration/apply-compliance-changes.xml**
  - Aplicar mudanças com backup
  - **Prioridade**: MÉDIA
  - **Tempo estimado**: 3 dias

#### Templates de Integração (1)

- [ ] **templates/integrations/sonarqube-properties**
  - Template para diferentes stacks
  - **Prioridade**: BAIXA
  - **Tempo estimado**: 1 dia

**Total Fase 4**: 23 dias úteis (~4.5 semanas)

---

## 🔄 Fase 5: Migration Workflows (Prioridade MÉDIA)

**Objetivo**: Migrar projetos entre estados

**Prazo Estimado**: 2 semanas

**Componentes**: 2 itens

### 🔨 Pendentes (2/2)

#### Workflows MIGRATE (2)

- [ ] **workflows/migrate/migrate-to-compliance/**
  - Usar análise + plano + apply
  - Validação pós-migração
  - **Prioridade**: MÉDIA
  - **Tempo estimado**: 3 dias

- [ ] **workflows/migrate/migrate-stage/**
  - Atualizar variáveis de ambiente
  - Criar tag git
  - Checklist de deploy
  - **Prioridade**: MÉDIA
  - **Tempo estimado**: 2 dias

**Total Fase 5**: 5 dias úteis (~1 semana)

---

## 🌟 Fase 6: Stacks Adicionais (Prioridade BAIXA)

**Objetivo**: Suporte para React Native, .NET, PHP

**Prazo Estimado**: 4 semanas

**Componentes**: 12 itens

### 🔨 Pendentes (12/12)

#### Templates (6)

- [ ] templates/docker-compose/reactnative-mobile.yaml (2 dias)
- [ ] templates/env/.env.react.example (1 dia)
- [ ] templates/docker-compose/dotnet-app.yaml (2 dias)
- [ ] templates/env/.env.dotnet.example (1 dia)
- [ ] templates/docker-compose/php-app.yaml (2 dias)
- [ ] templates/env/.env.php.example (1 dia)

#### Workflows CREATE (3)

- [ ] workflows/create/create-reactnative-mobile/ (4 dias)
- [ ] workflows/create/create-dotnet-app/ (4 dias)
- [ ] workflows/create/create-php-app/ (3 dias)

#### Templates Settings (2)

- [ ] templates/settings/settings-nodejs.json (1 dia)
- [ ] templates/settings/settings-frontend.json (1 dia)

#### Templates de Integração (1)

- [ ] templates/integrations/loki-config.yaml (1 dia)

**Total Fase 6**: 20 dias úteis (~4 semanas)

---

## 🛠️ Fase 7: Tooling & Automation (Prioridade MÉDIA)

**Objetivo**: Ferramentas de instalação e automação

**Prazo Estimado**: 1 semana

**Componentes**: 4 itens

### 🔨 Pendentes (4/4)

- [ ] **install.sh**
  - Copiar conhecimento para agentes
  - Adicionar menu items
  - Registrar workflows
  - **Tempo estimado**: 2 dias

- [ ] **uninstall.sh**
  - Remover conhecimento
  - Remover menu items
  - Opção de manter templates
  - **Tempo estimado**: 1 dia

- [ ] **Testes de validação**
  - Suite de testes para 38 regras
  - **Tempo estimado**: 3 dias

- [ ] **Tutorial interativo**
  - Modo tutorial para Dev Júnior
  - **Tempo estimado**: 2 dias

**Total Fase 7**: 8 dias úteis (~1.5 semanas)

---

## 📅 Cronograma Geral

| Fase | Prazo | Dependências | Status |
|------|-------|--------------|--------|
| **Fase 1**: Core Validation | 3 semanas | Nenhuma | 🟡 Em Progresso (40%) |
| **Fase 2**: Scaffolding Node.js | 3 semanas | Fase 1 | 🔴 Pendente |
| **Fase 3**: Frontend Scaffolding | 3 semanas | Fase 2 | 🔴 Pendente |
| **Fase 4**: Add Compliance | 4.5 semanas | Fase 1 | 🔴 Pendente |
| **Fase 5**: Migration | 1 semana | Fase 4 | 🔴 Pendente |
| **Fase 6**: Stacks Adicionais | 4 semanas | Fase 2, 3 | 🔴 Pendente |
| **Fase 7**: Tooling | 1.5 semanas | Fase 1-5 | 🔴 Pendente |

**Tempo Total Estimado**: 20 semanas (~5 meses)

**Data de Conclusão Prevista (v1.0 completo)**: Março 2026

---

## 🎯 Próximas Ações Imediatas

### Esta Semana (Prioridade CRÍTICA)

1. ✅ Finalizar workflows/validate/validate-compliance
2. ⏳ Implementar tasks/validation/validate-docker-compose.xml
3. ⏳ Implementar tasks/validation/validate-env.xml
4. ⏳ Implementar tasks/validation/validate-settings.xml

### Próxima Semana

1. Implementar tasks/validation/generate-compliance-score.xml
2. Criar workflows/validate/validate-docker-compose/
3. Criar workflows/validate/validate-env-files/
4. Testar validação completa em projeto real

### Próximas 2 Semanas

1. Finalizar todos os workflows VALIDATE
2. Começar templates Node.js (docker-compose + env)
3. Iniciar workflow create-nodejs-api

---

## 📊 Métricas de Sucesso

### Versão 1.0 (Completa)

- [x] 6 arquivos de conhecimento ✅
- [ ] 18 workflows funcionais (1/18) 🔴
- [ ] 20 templates completos (4/20) 🟡
- [ ] 8 tasks implementadas (0/8) 🔴
- [ ] 100% das 38 regras validáveis 🔴
- [ ] Suporte para 7 stacks 🔴
- [ ] 4 integrações MCP funcionais 🔴
- [ ] Instalador/Desinstalador funcionais 🔴
- [ ] Documentação completa (3/3) ✅

### Versão 1.1 (Melhorias)

- [ ] Auto-fix avançado (além das 6 correções básicas)
- [ ] Modo tutorial interativo
- [ ] Dashboard consolidado
- [ ] Testes automatizados (cobertura >80%)

---

**Última atualização**: 2025-10-06
**Responsável**: BMAD Team
**Status Geral**: 🟡 25% Completo
