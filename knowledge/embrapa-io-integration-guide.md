# Guia de Integração - Módulo Embrapa I/O

**Versão**: 1.0.0
**Última atualização**: 2025-10-10
**Propósito**: Instruir agentes BMAD sobre como criar aplicações aderentes ao Embrapa I/O

---

## 🎯 O que é o Módulo Embrapa I/O?

O módulo **Embrapa I/O** é um **MÓDULO DE CONHECIMENTO** que fornece:

- ✅ **Conhecimento técnico** sobre padrões da plataforma Embrapa I/O
- ✅ **Templates** para Docker Compose, .env files, settings.json
- ✅ **Workflows de validação** para garantir conformidade
- ✅ **Workflows de setup** para gerar arquivos obrigatórios
- ✅ **Documentação** completa das 4 Verdades Fundamentais

**O módulo NÃO cria aplicações**. Ele **instrui outros agentes** sobre como criar aplicações conformes.

---

## 🏗️ Como Agentes Devem Usar Este Módulo

### 1. Carregamento de Conhecimento

Qualquer agente que precise criar ou modificar aplicações para Embrapa I/O deve **carregar o conhecimento**:

```yaml
knowledge_base:
  embrapa_io_fundamentals: "{project-root}/bmad/embrapa-io/knowledge/embrapa-io-fundamentals.md"
  embrapa_io_stacks: "{project-root}/bmad/embrapa-io/knowledge/embrapa-io-stacks.md"
  embrapa_io_integrations: "{project-root}/bmad/embrapa-io/knowledge/embrapa-io-integrations.md"
  embrapa_io_validation: "{project-root}/bmad/embrapa-io/knowledge/embrapa-io-validation.md"
```

### 2. Invocação Automática de Workflows

Durante a criação de uma aplicação, o agente deve **invocar automaticamente** os workflows de setup:

#### A. Geração de .env.io (OBRIGATÓRIO)

```xml
<step n="X" goal="Gerar arquivos de variáveis da plataforma">
  <invoke-workflow>
    <path>{project-root}/bmad/embrapa-io/workflows/setup/generate-env-io/workflow.yaml</path>
    <description>Gera .env.io e .env.io.example com variáveis da plataforma Embrapa I/O</description>
  </invoke-workflow>
</step>
```

Este workflow é **executado automaticamente** e solicita ao usuário:
- Nome unix do projeto (IO_PROJECT)
- Nome unix da aplicação (IO_APP)
- Email do desenvolvedor (IO_DEPLOYER)

Os arquivos `.env.io` e `.env.io.example` são criados automaticamente com:
- ✅ COMPOSE_PROJECT_NAME calculado
- ✅ IO_VERSION no formato correto (0.YY.M-dev.1)
- ✅ Todas as variáveis obrigatórias da plataforma

#### B. Geração de LICENSE (OBRIGATÓRIO)

```xml
<step n="Y" goal="Gerar arquivo LICENSE da Embrapa">
  <invoke-workflow>
    <path>{project-root}/bmad/embrapa-io/workflows/setup/generate-license/workflow.yaml</path>
    <description>Cria LICENSE com copyright da Embrapa</description>
  </invoke-workflow>
</step>
```

Este workflow é **executado silenciosamente** (sem interação com usuário) e:
- ✅ Calcula o ano atual automaticamente
- ✅ Cria arquivo `LICENSE` na raiz do projeto
- ✅ Conteúdo: `Copyright ⓒ YYYY Brazilian Agricultural Research Corporation (Embrapa). All rights reserved.`

#### C. Outros Workflows de Setup (Futuros)

```xml
<!-- Quando implementados -->
<invoke-workflow path="{...}/setup/generate-docker-compose/workflow.yaml" />
<invoke-workflow path="{...}/setup/generate-settings-json/workflow.yaml" />
```

### 3. Uso de Templates

O módulo fornece templates para estruturas obrigatórias:

```yaml
templates:
  docker_compose_base: "{project-root}/bmad/embrapa-io/templates/docker-compose/base.yaml"
  settings_base: "{project-root}/bmad/embrapa-io/templates/settings/settings-base.json"
  settings_nodejs: "{project-root}/bmad/embrapa-io/templates/settings/settings-nodejs.json"
  settings_frontend: "{project-root}/bmad/embrapa-io/templates/settings/settings-frontend.json"
```

**Como usar os templates:**

```xml
<step n="Y" goal="Criar docker-compose.yaml conforme">
  <action>Carregar template: {docker_compose_base}</action>
  <action>Adaptar para stack específica (Node.js, Python, React, etc.)</action>
  <action>Garantir que segue as 4 Verdades Fundamentais:</action>
  <action>- Network externa: ${IO_PROJECT}_${IO_APP}_${IO_STAGE}</action>
  <action>- Volumes externos com mesma convenção</action>
  <action>- Serviços CLI: backup, restore, sanitize</action>
  <action>- Healthchecks em serviços de longa duração</action>
  <action>Salvar em {project-root}/docker-compose.yaml</action>
</step>
```

### 4. Validação de Conformidade

Após criar a aplicação, o agente deve **validar conformidade**:

```xml
<step n="Z" goal="Validar conformidade com Embrapa I/O">
  <invoke-workflow>
    <path>{project-root}/bmad/embrapa-io/workflows/validate/validate-compliance/workflow.yaml</path>
    <description>Valida se a aplicação está conforme com todas as regras do Embrapa I/O</description>
  </invoke-workflow>
</step>
```

---

## 📋 Checklist para Agentes de Criação

Ao criar uma aplicação, o agente deve assegurar:

### Arquivos Obrigatórios
- [ ] `LICENSE` (via workflow generate-license - executado silenciosamente)
- [ ] `.env.io` e `.env.io.example` (via workflow generate-env-io)
- [ ] `.env` e `.env.example` (criado pelo agente, sem duplicar variáveis)
- [ ] `docker-compose.yaml` (seguindo as 4 Verdades Fundamentais)
- [ ] `.embrapa/settings.json` (metadados da aplicação)

### Estrutura Docker Compose
- [ ] Network externa: `${IO_PROJECT}_${IO_APP}_${IO_STAGE}`
- [ ] Volumes externos com convenção correta
- [ ] Serviços de longa duração com `restart: unless-stopped` + `healthcheck`
- [ ] Serviços CLI: backup, restore, sanitize com `profiles: ['cli']`
- [ ] Nenhum serviço usa `container_name`
- [ ] Portas mapeadas via variáveis do `.env`

### Variáveis de Ambiente
- [ ] `.env.io` contém APENAS variáveis da plataforma (geradas automaticamente)
- [ ] `.env` contém APENAS variáveis da aplicação
- [ ] Nenhuma variável duplicada entre os arquivos
- [ ] Volumes seguem convenção: `${IO_PROJECT}_${IO_APP}_${IO_STAGE}_[nome]`

### Integrações (Recomendadas)
- [ ] Sentry configurado usando `${SENTRY_DSN}` do .env.io
- [ ] Matomo configurado usando `${MATOMO_ID}` e `${MATOMO_TOKEN}` do .env.io
- [ ] Release tracking usando `${IO_VERSION}` do .env.io
- [ ] Environment tracking usando `${IO_STAGE}` do .env.io

---

## 🔧 Exemplo Completo: Agente Criando Node.js API

```xml
<workflow>
  <step n="1" goal="Coletar informações do projeto">
    <ask>Qual o nome do projeto Node.js API que deseja criar?</ask>
    <!-- Armazena resposta em {{project_name}} -->
  </step>

  <step n="2" goal="Gerar arquivo LICENSE">
    <invoke-workflow>
      <path>{project-root}/bmad/embrapa-io/workflows/setup/generate-license/workflow.yaml</path>
    </invoke-workflow>
    <!-- Workflow executado silenciosamente -->
    <!-- Cria LICENSE com copyright da Embrapa automaticamente -->
  </step>

  <step n="3" goal="Gerar variáveis da plataforma Embrapa I/O">
    <invoke-workflow>
      <path>{project-root}/bmad/embrapa-io/workflows/setup/generate-env-io/workflow.yaml</path>
    </invoke-workflow>
    <!-- Workflow solicita IO_PROJECT, IO_APP, IO_DEPLOYER -->
    <!-- Cria .env.io e .env.io.example automaticamente -->
  </step>

  <step n="4" goal="Criar estrutura do projeto Node.js">
    <action>Criar diretórios: src/, src/config/, src/routes/</action>
    <action>Criar package.json com dependências</action>
    <action>Criar src/index.js com Express</action>
    <action>Criar Dockerfile otimizado</action>
  </step>

  <step n="5" goal="Configurar integrações Embrapa I/O">
    <action>Criar src/config/sentry.js usando process.env.SENTRY_DSN</action>
    <action>Criar src/config/matomo.js usando process.env.MATOMO_ID</action>
    <action>Injetar integrações em src/index.js</action>
  </step>

  <step n="6" goal="Criar docker-compose.yaml conforme">
    <action>Carregar template base do módulo Embrapa I/O</action>
    <action>Adicionar serviço API (Node.js)</action>
    <action>Adicionar serviço MongoDB com healthcheck</action>
    <action>Adicionar serviços CLI: backup, restore, sanitize</action>
    <action>Configurar network externa: ${IO_PROJECT}_${IO_APP}_${IO_STAGE}</action>
    <action>Configurar volumes externos: ${IO_PROJECT}_${IO_APP}_${IO_STAGE}_mongodb</action>
  </step>

  <step n="7" goal="Criar .embrapa/settings.json">
    <action>Carregar template settings-nodejs.json</action>
    <action>Preencher metadados: label, description, maintainers</action>
    <action>Definir variáveis: default, alpha, beta, release</action>
    <action>Salvar em .embrapa/settings.json</action>
  </step>

  <step n="8" goal="Criar .env e .env.example da aplicação">
    <action>Listar variáveis específicas da aplicação (não da plataforma)</action>
    <action>Criar .env com valores de desenvolvimento</action>
    <action>Criar .env.example com placeholders</action>
    <critical>NUNCA duplicar variáveis que já estão em .env.io</critical>
  </step>

  <step n="9" goal="Validar conformidade">
    <invoke-workflow>
      <path>{project-root}/bmad/embrapa-io/workflows/validate/validate-compliance/workflow.yaml</path>
    </invoke-workflow>
    <!-- Valida todas as regras e gera relatório -->
  </step>

  <step n="10" goal="Orientar próximos passos">
    <action>Informar que aplicação está pronta</action>
    <action>Explicar como obter SENTRY_DSN e MATOMO_ID no Dashboard</action>
    <action>Mostrar comando para iniciar: env $(cat .env.io) docker compose up</action>
  </step>
</workflow>
```

---

## 🌍 Aplicações em Outras Linguagens/Frameworks

O mesmo padrão se aplica para **qualquer linguagem ou framework**:

### Python/Django API
- Carrega conhecimento Embrapa I/O
- Invoca `generate-env-io` (mesmo workflow)
- Usa template base do docker-compose
- Adapta para Python/Django (pip, gunicorn, etc.)
- Configura Sentry/Matomo para Python
- Valida conformidade

### React/Vue.js Frontend
- Carrega conhecimento Embrapa I/O
- Invoca `generate-env-io` (mesmo workflow)
- Usa template base do docker-compose
- Adapta para frontend (nginx, build process)
- Configura Matomo tracking no frontend
- Valida conformidade

### PHP/Laravel
- Carrega conhecimento Embrapa I/O
- Invoca `generate-env-io` (mesmo workflow)
- Usa template base do docker-compose
- Adapta para PHP/Laravel (composer, php-fpm)
- Configura integrações
- Valida conformidade

**O padrão é sempre o mesmo**: Carregar conhecimento → Invocar workflows → Adaptar templates → Validar

---

## 📚 Referências do Módulo

| Documento | Propósito |
|-----------|-----------|
| `embrapa-io-fundamentals.md` | 4 Verdades Fundamentais, regras obrigatórias |
| `embrapa-io-stacks.md` | Stacks suportadas e suas particularidades |
| `embrapa-io-integrations.md` | Como integrar Sentry, Matomo, SonarQube, Loki |
| `embrapa-io-validation.md` | Regras de validação e compliance |
| `embrapa-io-workflows.md` | Workflows disponíveis e como usá-los |
| `embrapa-io-deployment.md` | Pipeline de 4 stages e deployment |

---

## ✅ Resumo

**O módulo Embrapa I/O:**
- ✅ É um **módulo de conhecimento**, não de criação
- ✅ Funciona com **qualquer linguagem/framework**
- ✅ Fornece **workflows** que são invocados automaticamente
- ✅ Fornece **templates** que são adaptados pelos agentes
- ✅ Fornece **validações** para garantir conformidade

**Os agentes do BMAD:**
- ✅ **Carregam o conhecimento** do módulo
- ✅ **Invocam os workflows** automaticamente durante criação
- ✅ **Adaptam os templates** para a stack específica
- ✅ **Validam conformidade** antes de finalizar

**Resultado:**
- 🎯 Aplicações em qualquer linguagem/framework aderentes ao Embrapa I/O
- 🎯 Processo automatizado e consistente
- 🎯 Validação garantida em todos os casos

---

**Versão**: 1.0.0
**Autor**: Módulo Embrapa I/O BMAD
**Mantido por**: Camilo Carromeu
