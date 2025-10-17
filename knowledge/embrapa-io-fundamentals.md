# Fundamentos da Plataforma Embrapa I/O

## 🎯 Visão Geral

A **Embrapa I/O** é uma plataforma de DevOps automatizada que orquestra aplicações containerizadas. Todas as aplicações devem ser totalmente containerizadas com Docker e configuradas via Docker Compose.

## 🔑 As 4 Verdades Fundamentais

### 1. Docker Compose com Networks e Volumes Externos

**OBRIGATÓRIO**: Todo projeto Embrapa I/O deve ter um `docker-compose.yaml` que:

- **NÃO** inclui campo `version` (obsoleto)
- Usa **network externa única** com padrão: `${IO_PROJECT}_${IO_APP}_${IO_STAGE}`
- Usa **volumes externos** com padrão: `${IO_PROJECT}_${IO_APP}_${IO_STAGE}_[nome]`
- Todos os serviços devem estar conectados à network `stack`

**Exemplo de estrutura obrigatória:**

```yaml
services:
  app:
    # configuração do serviço principal
    networks:
      - stack

  db:
    image: postgres:17-alpine
    restart: unless-stopped
    environment:
      POSTGRES_DB: ${DB_NAME}
      POSTGRES_USER: ${DB_USER}
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    volumes:
      - postgres_data:/var/lib/postgresql/data
    networks:
      - stack
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${DB_USER} -d ${DB_NAME}"]
      interval: 10s
      timeout: 5s
      retries: 5

networks:
  stack:
    name: ${IO_PROJECT}_${IO_APP}_${IO_STAGE}
    external: true

volumes:
  backup_data:
    name: ${IO_PROJECT}_${IO_APP}_${IO_STAGE}_backup
    external: true

  postgres_data:
    name: ${POSTGRES_VOLUME}
    external: true
```

**Regras para Serviços:**
- **Longa duração**: `restart: unless-stopped` + `healthcheck`
- **CLI**: `restart: "no"` + `profiles: ['cli']`
- **NUNCA** usar `container_name`
- **Todas as portas** mapeadas via variáveis do `.env`

### 2. Dual .env Files (Plataforma + Aplicação)

**OBRIGATÓRIO**: Dois arquivos de variáveis de ambiente:

#### `.env.io` (Variáveis da Plataforma)
Fornecido pela plataforma nos ambientes remotos, **NUNCA** alterar manualmente nesses ambientes.
No ambiente de desenvolvimento local, gerar usando o workflow do módulo Embrapa I/O.

**Estrutura OBRIGATÓRIA** (sempre as mesmas variáveis):
```ini
COMPOSE_PROJECT_NAME=meu-projeto_minha-app_development
COMPOSE_PROFILES=development
IO_SERVER=localhost
IO_PROJECT=meu-projeto
IO_APP=minha-app
IO_STAGE=development
IO_VERSION=0.25.10-dev.1
IO_DEPLOYER=first.surname@embrapa.br
SENTRY_DSN=GET_IN_DASHBOARD
MATOMO_ID=522
MATOMO_TOKEN=
```

**Regras de Preenchimento:**
- **COMPOSE_PROJECT_NAME**: Concatenação exata de `${IO_PROJECT}_${IO_APP}_development`
- **COMPOSE_PROFILES**: Sempre `development` no ambiente local
- **IO_SERVER**: Sempre `localhost` no ambiente local
- **IO_PROJECT**: Nome unix do projeto (lowercase, letras, números, hífens)
- **IO_APP**: Nome unix da aplicação (lowercase, letras, números, hífens)
- **IO_STAGE**: Sempre `development` no ambiente local de construção
- **IO_VERSION**: Formato obrigatório `0.YY.M-dev.1` onde:
  - YY = ano com 2 dígitos (ex: 25 para 2025)
  - M = mês SEM zero à esquerda (1-12, não 01-12)
  - Exemplo: Em outubro/2025 = `0.25.10-dev.1`
  - Exemplo: Em julho/2025 = `0.25.7-dev.1`
- **IO_DEPLOYER**: Email do desenvolvedor no formato `name.surname@embrapa.br`
- **SENTRY_DSN**: Obter no Dashboard Embrapa I/O (https://dashboard.embrapa.io)
- **MATOMO_ID**: Obter no Dashboard Embrapa I/O (valor padrão: 522)
- **MATOMO_TOKEN**: Deixar vazio inicialmente (preenchido conforme necessidade)

#### `.env` (Variáveis da Aplicação)
Criado pelo desenvolvedor com `.env.example`:
```ini
# NÃO repetir variáveis de .env.io
# Nomenclatura: ${IO_PROJECT}_${IO_APP}_${IO_STAGE}_[nome]

NODEJS_PORT=3000
SECRET=secret
DB_USER=postgres
DB_PASSWORD=secret
POSTGRES_VOLUME=meu-projeto_minha-app_development_postgres
```

**Regras Críticas:**
- ✅ **JAMAIS** colocar espaços ou aspas nos valores
- ✅ **JAMAIS** repetir variáveis de `.env.io`
- ✅ Se necessário espaços/aspas, criptografar em Base64
- ✅ Usar convenção: `${IO_PROJECT}_${IO_APP}_${IO_STAGE}_[nome]`

### 3. Arquivo .embrapa/settings.json (Metadados)

**OBRIGATÓRIO**: Arquivo de metadados do projeto na raiz:

```json
{
  "boilerplate": "_",
  "platform": "node",
  "label": "NodeJS com autenticação password-less (PIN)",
  "description": "NodeJS com Express, Mongoose, MongoDB e NodeMailer. Com a implementação de login do tipo password-less, ou seja, utilizando o envio de PIN por e-mail (com token JWT e utilizando o Passport).",
  "references": [
    { "label": "Site oficial do NodeJS", "url": "https://nodejs.dev/" },
    { "label": "JSON Web Tokens", "url": "https://jwt.io/" }
  ],
  "maintainers": [
    { "name": "Camilo Carromeu", "email": "camilo.carromeu@embrapa.br", "phone": "+55(67)9-8111-8060" }
  ],
  "variables": {
    "default": [
      { "name": "ENVIRONMENT", "type": "TEXT", "value": "test" },
      { "name": "PORT", "type": "PORT" },
      { "name": "BASE_URL", "type": "TEXT", "value": "https://this-app.com/api" },
      { "name": "FRONTEND_URL", "type": "TEXT", "value": "https://my-pwa.com" },
      { "name": "SECRET", "type": "SECRET" },
      { "name": "MAILHOG_PORT", "type": "EMPTY" },
      { "name": "MONGO_DATA", "type": "VOLUME", "value": "db" },
      { "name": "MONGO_ROOT_PASSWORD", "type": "PASSWORD" },
      { "name": "MONGO_NON_ROOT_USERNAME", "type": "TEXT", "value": "user" },
      { "name": "MONGO_NON_ROOT_PASSWORD", "type": "PASSWORD" },
      { "name": "EXPRESS_PORT", "type": "PORT" },
      { "name": "EXPRESS_USERNAME", "type": "TEXT", "value": "express" },
      { "name": "EXPRESS_PASSWORD", "type": "PASSWORD" },
      { "name": "SMTP_HOST", "type": "TEXT", "value": "smtp-relay.gmail.com" },
      { "name": "SMTP_PORT", "type": "TEXT", "value": "587" },
      { "name": "SMTP_SECURE", "type": "TEXT", "value": "yes" }
    ],
    "alpha": [],
    "beta": [],
    "release": [
      { "name": "ENVIRONMENT", "type": "TEXT", "value": "production" }
    ]
  },
  "orchestrators": [ "DockerCompose" ]
}
```

**CAMPOS OBRIGATÓRIOS**:

- **boilerplate**: Nome do boilerplate utilizado ou "_" para projetos greenfield
- **platform**: Uma dentre: android, apple, dart, dotnet, electron, elixir, flutter, go, java, javascript, kotlin, native, node, php, python, react-native, ruby, rust, unity, unreal
- **label**: Título descritivo curto da aplicação
- **description**: Descrição detalhada da aplicação e stack tecnológica
- **references**: Array de objetos com label e url de referências técnicas
- **maintainers**: Array de objetos com name, email e phone dos mantenedores
- **variables**: Objeto com arrays default, alpha, beta, release contendo variáveis de ambiente
- **orchestrators**: Array com orquestradores (sempre ["DockerCompose"])

**TIPOS DE VARIÁVEIS**:

- **TEXT**: String sem espaços (/^[^\s]*$/). Sem value = vazio
- **PASSWORD**: String sem espaços (/^[^\s]*$/). Sem value = random 16 chars
- **SECRET**: String sem espaços (/^[^\s]*$/). Sem value = random 256 chars
- **PORT**: Porta exposta publicamente, definida na build
- **VOLUME**: Referência a volumes Docker
- **EMPTY**: Força string vazia como valor

**REGRAS IMPORTANTES**:

- ✅ **JAMAIS** usar espaços ou aspas nos valores das variáveis
- ✅ Variáveis do array **default** devem mapear todas as variáveis do .env
- ✅ Arrays **alpha**, **beta**, **release** contêm apenas OVERRIDES de valores
- ✅ Telefone no formato: +55(XX)9-XXXX-XXXX (sem espaços)

### 4. Serviços CLI Obrigatórios (Backup, Restore, Sanitize)

**OBRIGATÓRIO**: Três serviços CLI com `profiles: ['cli']`:

#### Serviço de Backup
```yaml
backup:
  image: postgres:17-alpine  # ou tecnologia correspondente
  restart: "no"
  profiles:
    - cli
  environment:
    # variáveis necessárias
  volumes:
    - backup_data:/backup
    - postgres_data:/var/lib/postgresql/data
  networks:
    - stack
  command: >
    sh -c "
      set -ex &&
      BACKUP_DIR=${IO_PROJECT}_${IO_APP}_${IO_STAGE}_${IO_VERSION}_$$(date +'%Y-%m-%d_%H-%M-%S') &&
      mkdir -p /backup/$$BACKUP_DIR &&
      pg_dump -U ${DB_USER} -d ${DB_NAME} > /backup/$$BACKUP_DIR/database.sql &&
      tar -czf /backup/$$BACKUP_DIR.tar.gz -C /backup $$BACKUP_DIR &&
      rm -rf /backup/$$BACKUP_DIR
    "
```

#### Serviço de Restore
```yaml
restore:
  image: postgres:17-alpine
  restart: "no"
  profiles:
    - cli
  environment:
    # variáveis necessárias
  volumes:
    - backup_data:/backup
    - postgres_data:/var/lib/postgresql/data
  networks:
    - stack
  command: >
    sh -c "
      set -ex &&
      FILE_TO_RESTORE=${BACKUP_FILE_TO_RESTORE:-no_file_to_restore} &&
      test -f /backup/$$FILE_TO_RESTORE &&
      RESTORE_DIR=$$(mktemp -d) &&
      tar -xf /backup/$$FILE_TO_RESTORE -C $$RESTORE_DIR --strip-components=1 &&
      psql -U ${DB_USER} -d ${DB_NAME} < $$RESTORE_DIR/database.sql &&
      rm -rf $$RESTORE_DIR
    "
```

#### Serviço de Sanitize
```yaml
sanitize:
  image: postgres:17-alpine
  restart: "no"
  profiles:
    - cli
  environment:
    # variáveis necessárias
  volumes:
    - postgres_data:/var/lib/postgresql/data
  networks:
    - stack
  command: >
    sh -c '
      set -ex &&
      psql -U ${DB_USER} -d ${DB_NAME} -c "VACUUM ANALYZE;" &&
      echo "Database optimized successfully"
    '
```

## 🚀 Pipeline de 4 Stages

A plataforma Embrapa I/O opera com 4 ambientes distintos:

| Stage | Propósito | Auto-Deploy | Requer Aprovação |
|-------|-----------|-------------|------------------|
| **development** | Desenvolvimento ativo | ✅ Sim | ❌ Não |
| **alpha** | Testes internos | ❌ Não | ✅ Sim |
| **beta** | Testes com usuários | ❌ Não | ✅ Sim |
| **release** | Produção | ❌ Não | ✅ Sim |

**Diferenciação via COMPOSE_PROFILES**:
```bash
# Development
env $(cat .env.io) COMPOSE_PROFILES=development docker compose up

# Alpha
env $(cat .env.io) COMPOSE_PROFILES=alpha docker compose up

# Beta
env $(cat .env.io) COMPOSE_PROFILES=beta docker compose up

# Release
env $(cat .env.io) COMPOSE_PROFILES=release docker compose up
```

## 🔌 Integrações Recomendadas

### Sentry (Monitoramento de Erros)

**Configuração padrão** (Node.js):
```javascript
const Sentry = require('@sentry/node');

Sentry.init({
  dsn: process.env.SENTRY_DSN,
  release: process.env.IO_VERSION.split('-')[0],
  environment: process.env.IO_STAGE,
  tracesSampleRate: 1.0
});
```

### Matomo (Analytics)

**Configuração padrão**:
- **Host**: `https://hit.embrapa.io`
- **Site ID**: `${MATOMO_ID}` (do .env.io)
- **Token**: `${MATOMO_TOKEN}` (para backends)
- **Custom dimensions**: stage e version

**Exemplo** (Frontend):
```javascript
_paq.push(['setCustomDimension', 1, process.env.IO_STAGE]);
_paq.push(['setCustomDimension', 2, process.env.IO_VERSION]);
_paq.push(['trackPageView']);
```

### SonarQube (Qualidade de Código)

**Arquivo** `sonar-project.properties`:
```properties
sonar.projectKey=${IO_PROJECT}_${IO_APP}
sonar.projectName=${IO_PROJECT} ${IO_APP}
sonar.projectVersion=${IO_VERSION}
sonar.sources=src
sonar.exclusions=node_modules/**,dist/**,build/**
```

### Grafana Loki (Logs Centralizados)

**Configuração** (Docker Compose):
```yaml
logging:
  driver: loki
  options:
    loki-url: "https://loki.embrapa.io/loki/api/v1/push"
    loki-external-labels: "project=${IO_PROJECT},app=${IO_APP},stage=${IO_STAGE}"
```

## 📦 Comandos Padrão

Toda aplicação Embrapa I/O deve funcionar com:

```bash
# Iniciar aplicação
env $(cat .env.io) docker compose up --force-recreate --build --remove-orphans --wait

# Executar CLI (backup)
env $(cat .env.io) docker compose run --rm --no-deps backup

# Executar CLI (restore)
env $(cat .env.io) BACKUP_FILE_TO_RESTORE=backup_file.tar.gz docker compose run --rm --no-deps restore

# Executar CLI (sanitize)
env $(cat .env.io) docker compose run --rm --no-deps sanitize
```

## ✅ Checklist de Conformidade

- [ ] `docker-compose.yaml` com network externa `${IO_PROJECT}_${IO_APP}_${IO_STAGE}`
- [ ] Todos os volumes são externos
- [ ] Nenhum serviço usa `container_name`
- [ ] Serviços de longa duração têm `restart: unless-stopped` + `healthcheck`
- [ ] `.env.io.example` e `.env.example` presentes
- [ ] Variáveis seguem convenção `${IO_PROJECT}_${IO_APP}_${IO_STAGE}_[nome]`
- [ ] `.embrapa/settings.json` presente com metadados corretos
- [ ] Serviços CLI (backup, restore, sanitize) implementados
- [ ] Integrações Sentry e Matomo configuradas
- [ ] Logo da Embrapa presente em interfaces visuais

## 🎨 Diretrizes de UI

Para aplicações com interface visual:

- **Tema**: Verde remetendo à agropecuária e sustentabilidade
- **Logo**: **SEMPRE** incluir a logo da Embrapa
- **Paleta sugerida**:
  - Primária: `#008542` (verde Embrapa)
  - Secundária: `#6FAC46` (verde claro)
  - Texto: `#333333`
  - Fundo: `#FFFFFF` / `#F5F5F5`

## 🔒 Segurança

- **NUNCA** hardcode senhas ou chaves
- **SEMPRE** use variáveis de ambiente para configurações sensíveis
- **SEMPRE** implemente healthchecks robustos
- **NUNCA** exponha portas desnecessárias
- **SEMPRE** valide inputs e sanitize outputs

## 📚 Referências

- Documento completo: `/Users/camilo/Projects/test/AGENTS.md`
- Repositório de templates: (a ser definido)
- Documentação da plataforma: (a ser definido)

---

**Versão**: 1.0
**Última atualização**: 2025-10-06
**Autor**: Módulo Embrapa I/O BMAD
