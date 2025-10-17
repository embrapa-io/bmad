# Módulo Embrapa I/O DevOps - BMAD

**Versão**: 1.0.0
**Tipo**: Standard Module
**Autor**: BMAD Team
**Licença**: MIT

## 🎯 Visão Geral

O **Módulo Embrapa I/O** é um **MÓDULO DE CONHECIMENTO** BMAD que garante aderência à plataforma Embrapa I/O em aplicações de **qualquer linguagem ou framework**.

### O que é um Módulo de Conhecimento?

Um módulo de conhecimento **NÃO cria aplicações diretamente**. Em vez disso, ele **instrui outros agentes** sobre como criar aplicações conformes através de:

- 📚 **Conhecimento técnico** sobre padrões da plataforma
- 📋 **Templates** adaptáveis para qualquer stack
- 🔄 **Workflows reutilizáveis** invocados automaticamente
- ✅ **Validações** que garantem conformidade
- 🔌 **Integrações** com ferramentas do ecossistema

### Como Funciona?

1. **Agentes BMAD existentes** (scaffolding, criação, etc.) **carregam o conhecimento** deste módulo
2. Durante a criação de aplicações, eles **invocam automaticamente** os workflows do módulo
3. O módulo fornece **templates e validações** que funcionam com **qualquer linguagem/framework**
4. Resultado: **Aplicações conformes** sem necessidade de agentes específicos por stack

## 🏗️ Arquitetura do Módulo

```
┌─────────────────────────────────────────────────────────┐
│  AGENTES BMAD EXISTENTES (Qualquer Linguagem/Stack)    │
│                                                          │
│  • Scaffold Agent  • Creation Agents  • QA Agent       │
│  • DevOps Agent    • Migration Agents                   │
└────────────────────┬────────────────────────────────────┘
                     │ Carregam conhecimento
                     │ Invocam workflows
                     ▼
┌─────────────────────────────────────────────────────────┐
│  MÓDULO EMBRAPA I/O (Conhecimento)                     │
│                                                          │
│  📚 Knowledge Base  →  Padrões e regras                 │
│  🔄 Workflows       →  Geração automática de arquivos   │
│  📋 Templates       →  Adaptáveis para qualquer stack   │
│  ✅ Validations     →  Garantia de conformidade         │
└─────────────────────────────────────────────────────────┘
                     │
                     ▼
         Aplicações Conformes (Node.js, Python, React,
              Vue.js, PHP, .NET, Java, Go, etc.)
```

## 📦 Componentes

### 📚 Conhecimento (7 arquivos)

1. **embrapa-io-fundamentals.md** - As 4 verdades fundamentais da plataforma
2. **embrapa-io-integration-guide.md** - **🆕 Como agentes devem usar este módulo**
3. **embrapa-io-validation.md** - 38 regras de validação com severidades
4. **embrapa-io-workflows.md** - Workflows disponíveis e como invocá-los
5. **embrapa-io-deployment.md** - Pipeline de 4 stages e comandos essenciais
6. **embrapa-io-stacks.md** - Stacks e suas particularidades
7. **embrapa-io-integrations.md** - Integração com Sentry, Matomo, SonarQube, Loki

### 🔄 Workflows (Invocáveis por Agentes)

Os workflows deste módulo são **invocados automaticamente** por agentes durante a criação ou modificação de aplicações.

#### SETUP (Geração Automática de Arquivos)
- **`generate-license`** - **✅ IMPLEMENTADO** - Gera `LICENSE` com copyright da Embrapa
  - Executado silenciosamente (sem interação com usuário)
  - Calcula ano atual automaticamente
  - Conteúdo: `Copyright ⓒ YYYY Brazilian Agricultural Research Corporation (Embrapa). All rights reserved.`

- **`generate-env-io`** - **✅ IMPLEMENTADO** - Gera `.env.io` e `.env.io.example` com variáveis da plataforma
  - Invocado automaticamente durante criação de aplicações
  - Solicita: IO_PROJECT, IO_APP, IO_DEPLOYER
  - Calcula: IO_VERSION (formato 0.YY.M-dev.1)
  - Cria ambos os arquivos automaticamente

#### VALIDATE (Validação de Conformidade)
- **`validate-compliance`** - **✅ IMPLEMENTADO** - Validação completa de conformidade (38 regras)
- `validate-docker-compose` - **📋 PLANEJADO** - Validação específica de docker-compose.yaml
- `validate-env-files` - **📋 PLANEJADO** - Validação de .env.io e .env
- `validate-settings` - **📋 PLANEJADO** - Validação de .embrapa/settings.json
- `validate-integrations` - **📋 PLANEJADO** - Validação de integrações (Sentry, Matomo)
- `generate-compliance-report` - **📋 PLANEJADO** - Relatório de conformidade

#### ADD (Adicionar Funcionalidades)
- `add-embrapa-compliance` - **📋 PLANEJADO** - Adicionar conformidade a projeto existente
- `add-sentry-integration` - **📋 PLANEJADO** - Adicionar integração Sentry
- `add-matomo-integration` - **📋 PLANEJADO** - Adicionar integração Matomo
- `add-sonarqube-integration` - **📋 PLANEJADO** - Adicionar integração SonarQube
- `add-loki-integration` - **📋 PLANEJADO** - Adicionar integração Grafana Loki

#### MIGRATE (Migração de Projetos)
- `migrate-to-compliance` - **📋 PLANEJADO** - Migrar projeto não-conforme para Embrapa I/O
- `migrate-stage` - **📋 PLANEJADO** - Migrar entre stages (development → alpha → beta → release)

### 📋 Templates (20 templates)

#### docker-compose (7)
- base.yaml, nodejs-api.yaml, vuejs-frontend.yaml, react-frontend.yaml, reactnative-mobile.yaml, dotnet-app.yaml, php-app.yaml

#### env (7)
- .env.io.example, .env.example, .env.nodejs.example, .env.vuejs.example, .env.react.example, .env.dotnet.example, .env.php.example

#### settings (3)
- settings-base.json, settings-nodejs.json, settings-frontend.json

#### integrations (4)
- sentry-config.js, matomo-tracking.js, sonarqube-properties, loki-config.yaml

## 🚀 Instalação

### Pré-requisitos

- BMAD framework instalado
- Claude Code

### Instalar Módulo

```bash
# Navegar para diretório de módulos BMAD
cd /caminho/para/bmad

# Copiar módulo embrapa-io
cp -r /caminho/para/embrapa-io ./embrapa-io

# Executar installer (a ser implementado)
./embrapa-io/install.sh
```

### O que a instalação faz

1. ✅ Cria estrutura de diretórios do módulo
2. ✅ Copia arquivos de conhecimento para os agentes (QA, PM, DevOps)
3. ✅ Adiciona itens de menu nos agentes
4. ✅ Registra workflows no BMAD
5. ✅ Mostra tutorial de uso (opcional)

## 📖 Como Usar

### 1. Validar Projeto Existente

```bash
# Via QA Agent
bmad qa

# Menu: 🔍 Validar Conformidade Embrapa I/O
> Caminho do projeto: /caminho/para/meu-projeto
> Aplicar auto-fix: sim/não

# Resultado:
# - validation-report-TIMESTAMP.json
# - compliance-summary-TIMESTAMP.md
# - Compliance Score: HIGH|MEDIUM|LOW
```

### 2. Criar Novo Projeto Node.js API

```bash
# Via PM Agent
bmad pm

# Menu: 🚀 Criar projeto Node.js API
> Nome do projeto: meu-projeto
> Nome da app: api
> Tipo de banco: mongodb|postgresql
> Porta: 3000
> Integrações: sentry,matomo

# Resultado:
# - Estrutura completa do projeto
# - docker-compose.yaml conforme
# - .env files
# - Código base com exemplos
# - Compliance Score: HIGH
```

### 3. Adicionar Conformidade a Projeto Existente

```bash
# Via PM Agent
bmad pm

# Menu: ➕ Adicionar conformidade Embrapa I/O
> Caminho do projeto: /caminho/para/projeto-existente

# Resultado:
# - docker-compose.yaml ajustado/criado
# - .env files criados
# - .embrapa/settings.json criado
# - Relatório de mudanças
```

### 4. Adicionar Integração Sentry

```bash
# Via DevOps Agent
bmad devops

# Menu: 🚨 Adicionar integração Sentry
> Caminho do projeto: /caminho/para/projeto
> Stack detectada: nodejs

# Resultado:
# - Sentry SDK instalado
# - Configuração implementada
# - SENTRY_DSN em .env.io
# - Documentação de uso
```

## 📊 Sistema de Validação

### Compliance Score

| Score | Critério | Ação Recomendada |
|-------|----------|------------------|
| **HIGH** 🟢 | 0 CRITICAL, 0 HIGH | Pronto para produção |
| **MEDIUM** 🟡 | 0 CRITICAL, ≤3 HIGH | Corrigir erros HIGH |
| **LOW** 🔴 | ≥1 CRITICAL ou >3 HIGH | Bloqueado para produção |

### 38 Regras de Validação

#### CRITICAL (8 regras)
- Arquivo docker-compose.yaml existe
- Campo 'version' ausente
- Network 'stack' externa com nome correto
- Arquivos .env.io.example e .env.example existem
- Sem duplicação de variáveis entre .env files
- Arquivo .embrapa/settings.json existe e é válido

#### HIGH (14 regras)
- Serviços conectados à network 'stack'
- Volumes externos
- Sem container_name
- Healthchecks implementados
- Variáveis obrigatórias presentes
- Convenção de nomenclatura correta
- Integrações Sentry e Matomo configuradas

#### MEDIUM (11 regras)
- Portas via variáveis
- CLI services com profiles
- .gitignore correto
- Data de validação atualizada
- Logo Embrapa presente (UI)

#### LOW (5 regras)
- Serviços CLI recomendados
- SonarQube configurado
- Grafana Loki configurado

## 🔌 Integrações Suportadas

| Integração | Tipo | Obrigatório | MCP |
|------------|------|-------------|-----|
| **Sentry** | Error Monitoring | ✅ Sim | ✅ |
| **Matomo** | Analytics | ✅ Sim | ✅ |
| **SonarQube** | Code Quality | ❌ Não | ✅ |
| **Grafana Loki** | Logging | ❌ Não | ✅ |

## 🎨 Stacks Suportadas

| Stack | Versões | Banco Suportado | Difficulty |
|-------|---------|----------------|------------|
| Node.js + Express + MongoDB | 20.x, 4.x, 7.x | MongoDB | ⭐⭐ |
| Node.js + Express + PostgreSQL | 20.x, 4.x, 17.x | PostgreSQL | ⭐⭐ |
| Vue.js + Vuetify | 3.x, 3.x | N/A | ⭐⭐ |
| React | 18.x | N/A | ⭐⭐ |
| React Native | Latest | N/A | ⭐⭐⭐ |
| .NET Blazor | 8.x | SQL Server, PostgreSQL | ⭐⭐⭐ |
| PHP + Laravel | 8.2, 10.x | MySQL, PostgreSQL | ⭐⭐ |

## 🔄 Pipeline de 4 Stages

| Stage | Auto-Deploy | Aprovação | Uso |
|-------|-------------|-----------|-----|
| **development** | ✅ Sim | ❌ Não | Desenvolvimento ativo |
| **alpha** | ❌ Não | ✅ Tech Lead | Testes internos |
| **beta** | ❌ Não | ✅ PO | Testes com usuários |
| **release** | ❌ Não | ✅ Múltiplas | Produção |

## 📚 Arquivos de Conhecimento

Todos os agentes enriquecidos terão acesso a:

- `embrapa-io-fundamentals.md` - Fundamentos da plataforma
- `embrapa-io-validation.md` - Regras de validação
- `embrapa-io-workflows.md` - Workflows adaptativos
- `embrapa-io-deployment.md` - Deployment e comandos
- `embrapa-io-stacks.md` - Detalhes das stacks
- `embrapa-io-integrations.md` - Guias de integração

## 🛠️ Desenvolvimento

### Estrutura de Diretórios

```
embrapa-io/
├── config.yaml                      # Configuração do módulo
├── README.md                         # Este arquivo
├── knowledge/                        # Arquivos de conhecimento (6)
│   ├── embrapa-io-fundamentals.md
│   ├── embrapa-io-validation.md
│   ├── embrapa-io-workflows.md
│   ├── embrapa-io-deployment.md
│   ├── embrapa-io-stacks.md
│   └── embrapa-io-integrations.md
├── workflows/                        # Workflows organizados (18)
│   ├── validate/                     # 6 workflows
│   ├── create/                       # 6 workflows
│   ├── add/                          # 5 workflows
│   └── migrate/                      # 2 workflows
├── templates/                        # Templates (20)
│   ├── docker-compose/               # 7 templates
│   ├── env/                          # 7 templates
│   ├── settings/                     # 3 templates
│   └── integrations/                 # 4 templates
├── tasks/                            # Tasks reutilizáveis
│   ├── validation/
│   ├── scaffolding/
│   └── migration/
└── docs/                             # Documentação adicional
```

### Adicionar Novo Workflow

1. Criar diretório em `workflows/[categoria]/[workflow-name]/`
2. Criar `workflow.yaml` com configuração
3. Criar `instructions.md` com instruções detalhadas
4. Atualizar `config.yaml` na seção `workflows`
5. Adicionar menu item no agente apropriado

### Adicionar Nova Stack

1. Criar template em `templates/docker-compose/[stack].yaml`
2. Criar variáveis em `templates/env/.env.[stack].example`
3. Documentar em `knowledge/embrapa-io-stacks.md`
4. Criar workflow `create-[stack]-app` em `workflows/create/`
5. Atualizar `config.yaml` na seção `supported_stacks`

## 📋 Roadmap

### Fase 1: Core (Atual - v1.0)
- [x] Arquivos de conhecimento completos
- [x] Sistema de validação (38 regras)
- [x] Workflow validate-compliance
- [x] Templates base
- [x] Config.yaml completo
- [ ] Workflows CREATE (6)
- [ ] Workflows ADD (5)
- [ ] Workflows MIGRATE (2)
- [ ] Installer/Uninstaller
- [ ] Testes de validação

### Fase 2: Scaffolding (v1.1)
- [ ] create-nodejs-api completo
- [ ] create-vuejs-frontend completo
- [ ] create-react-frontend completo
- [ ] Modo tutorial para Dev Júnior
- [ ] Auto-fix avançado

### Fase 3: Integrações (v1.2)
- [ ] MCP Sentry completo
- [ ] MCP Matomo completo
- [ ] MCP SonarQube completo
- [ ] MCP Grafana Loki completo
- [ ] Dashboard consolidado para Tech Lead

### Fase 4: Advanced (v2.0)
- [ ] Versionamento de .embrapa/config.yaml
- [ ] Smart templates com lógica condicional
- [ ] CI/CD nativo
- [ ] Rollback automático
- [ ] Multi-project dashboard

## 🤝 Contribuindo

Este módulo segue os padrões BMAD. Para contribuir:

1. Fork o repositório
2. Crie uma branch (`feature/nova-funcionalidade`)
3. Commit suas mudanças
4. Push para a branch
5. Abra um Pull Request

## 📄 Licença

MIT License - veja LICENSE para detalhes

## 🆘 Suporte

- **Documentação**: `knowledge/` folder
- **Issues**: GitHub Issues
- **Discussões**: GitHub Discussions

---

**Desenvolvido com** ❤️ **usando BMAD Method**

**Versão**: 1.0.0
**Última atualização**: 2025-10-06
