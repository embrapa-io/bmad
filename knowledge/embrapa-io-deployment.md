# Deployment Embrapa I/O

## 🚀 Pipeline de 4 Stages

### Stage 1: Development
- **Auto-deploy**: ✅ Sim
- **Aprovação**: ❌ Não requerida
- **Uso**: Desenvolvimento ativo
- **Profile**: `COMPOSE_PROFILES=development`

```bash
env $(cat .env.io) COMPOSE_PROFILES=development docker compose up
```

### Stage 2: Alpha
- **Auto-deploy**: ❌ Não
- **Aprovação**: ✅ Requerida (Tech Lead)
- **Uso**: Testes internos
- **Profile**: `COMPOSE_PROFILES=alpha`

```bash
env $(cat .env.io) COMPOSE_PROFILES=alpha docker compose up
```

### Stage 3: Beta
- **Auto-deploy**: ❌ Não
- **Aprovação**: ✅ Requerida (Product Owner)
- **Uso**: Testes com usuários
- **Profile**: `COMPOSE_PROFILES=beta`

```bash
env $(cat .env.io) COMPOSE_PROFILES=beta docker compose up
```

### Stage 4: Release
- **Auto-deploy**: ❌ Não
- **Aprovação**: ✅ Requerida (Múltiplas aprovações)
- **Uso**: Produção
- **Profile**: `COMPOSE_PROFILES=release`

```bash
env $(cat .env.io) COMPOSE_PROFILES=release docker compose up
```

## 🔄 Fluxo de Deployment

1. **Commit** → development (auto)
2. **Validação** → compliance check
3. **Aprovação Tech Lead** → alpha
4. **Testes internos** → feedback
5. **Aprovação PO** → beta
6. **Testes com usuários** → validação
7. **Aprovações finais** → release
8. **Produção** → monitoramento

## 📦 Comandos Essenciais

### Setup Inicial

```bash
# Criar network externa
docker network create ${IO_PROJECT}_${IO_APP}_${IO_STAGE}

# Criar volumes externos
docker volume create ${IO_PROJECT}_${IO_APP}_${IO_STAGE}_postgres
docker volume create ${IO_PROJECT}_${IO_APP}_${IO_STAGE}_pgadmin

# Criar volume de backup (bind mount)
docker volume create \
  --driver local \
  --opt type=none \
  --opt device=$(pwd)/backup \
  --opt o=bind \
  ${IO_PROJECT}_${IO_APP}_${IO_STAGE}_backup
```

### Operações

```bash
# Iniciar aplicação
env $(cat .env.io) docker compose up --force-recreate --build --remove-orphans --wait

# Parar aplicação
env $(cat .env.io) docker compose down

# Ver logs
env $(cat .env.io) docker compose logs -f

# Backup
env $(cat .env.io) docker compose run --rm --no-deps backup

# Restore
env $(cat .env.io) BACKUP_FILE_TO_RESTORE=backup.tar.gz docker compose run --rm --no-deps restore

# Sanitize
env $(cat .env.io) docker compose run --rm --no-deps sanitize
```

## 🔌 Integrações de Deployment

### Sentry Release Tracking

```javascript
// Backend (Node.js)
const Sentry = require('@sentry/node');

Sentry.init({
  dsn: process.env.SENTRY_DSN,
  release: process.env.IO_VERSION.split('-')[0],
  environment: process.env.IO_STAGE,
  tracesSampleRate: 1.0
});
```

### Matomo Custom Dimensions

```javascript
// Frontend
_paq.push(['setCustomDimension', 1, process.env.IO_STAGE]); // Stage
_paq.push(['setCustomDimension', 2, process.env.IO_VERSION]); // Version
_paq.push(['trackPageView']);
```

### Grafana Loki Logging

```yaml
# docker-compose.yaml
services:
  app:
    logging:
      driver: loki
      options:
        loki-url: "https://loki.embrapa.io/loki/api/v1/push"
        loki-external-labels: "project=${IO_PROJECT},app=${IO_APP},stage=${IO_STAGE}"
```

## 🛡️ Health Checks

### Exemplo PostgreSQL

```yaml
healthcheck:
  test: ["CMD-SHELL", "pg_isready -U ${DB_USER} -d ${DB_NAME}"]
  interval: 10s
  timeout: 5s
  retries: 5
```

### Exemplo Node.js API

```yaml
healthcheck:
  test: ["CMD", "wget", "--spider", "--quiet", "http://localhost:${NODEJS_PORT}/health"]
  interval: 30s
  timeout: 10s
  retries: 3
  start_period: 40s
```

## 📊 Monitoramento

### Métricas Essenciais

- **Uptime**: >99.5% por stage
- **Response Time**: <500ms (p95)
- **Error Rate**: <1%
- **CPU Usage**: <70% média
- **Memory Usage**: <80% média

### Alertas Recomendados

- Error rate >2% por 5 minutos
- Response time >1s (p95) por 10 minutos
- Uptime <99% em 24h
- Disk usage >85%

## 🔐 Segurança

### Variáveis Sensíveis

```bash
# NUNCA commitar
.env
.env.io

# Sempre usar em .gitignore
echo ".env" >> .gitignore
echo ".env.io" >> .gitignore
```

### Secrets Management

- Usar variáveis de ambiente
- Criptografar em Base64 se necessário
- Rotacionar secrets regularmente
- Auditar acessos

## 📋 Checklist de Deploy

- [ ] Compliance score = HIGH
- [ ] Testes passando
- [ ] Documentação atualizada
- [ ] Backup realizado
- [ ] Variáveis de ambiente corretas
- [ ] Network e volumes criados
- [ ] Healthchecks funcionando
- [ ] Integrações configuradas
- [ ] Logs centralizados ativos
- [ ] Monitoramento ativo

---

**Versão**: 1.0
**Última atualização**: 2025-10-06
**Autor**: Módulo Embrapa I/O BMAD
