# Checklist de Validação - Generate docker-compose.yaml

## ✅ Conformidade com as 4 Verdades Fundamentais

### Verdade 1: Sem Campo 'version'
- [ ] Campo `version` está AUSENTE no arquivo
- [ ] Docker Compose v2 não requer campo version
- [ ] Arquivo inicia diretamente com `services:`

### Verdade 2: Network 'stack' Externa
- [ ] Network `stack` está configurada
- [ ] Propriedade `external: true` presente
- [ ] Nome da network usa padrão: `${IO_PROJECT}_${IO_APP}_${IO_STAGE}`
- [ ] Todos os serviços conectados à network `stack`

### Verdade 3: Volumes Externos
- [ ] Todos os volumes possuem `external: true`
- [ ] Nomes dos volumes seguem padrão: `${IO_PROJECT}_${IO_APP}_${IO_STAGE}_{nome}`
- [ ] Volumes não são criados automaticamente pelo Compose
- [ ] Documentação sobre criar volumes manualmente fornecida

### Verdade 4: Sem container_name
- [ ] NENHUM serviço possui campo `container_name`
- [ ] Docker Compose gerencia nomes automaticamente
- [ ] Permite múltiplas instâncias sem conflitos

## ✅ Estrutura do Arquivo

### Serviço Principal (app)
- [ ] Serviço `app` está presente
- [ ] Build context configurado: `build: .`
- [ ] Restart policy: `restart: unless-stopped`
- [ ] Porta exposta via variável: `${APP_PORT}:XXXX`
- [ ] Variáveis de ambiente referenciando .env files
- [ ] Network `stack` configurada
- [ ] Healthcheck implementado com endpoint correto
- [ ] Volumes configurados (se necessário)

### Healthcheck
- [ ] Healthcheck presente no serviço principal
- [ ] Comando test configurado (wget, curl, ou específico da stack)
- [ ] Interval configurado (recomendado: 30s)
- [ ] Timeout configurado (recomendado: 10s)
- [ ] Retries configurado (recomendado: 3)
- [ ] Start period configurado (recomendado: 40s)

### Serviços CLI (Opcional)
- [ ] Serviços CLI usam `profiles: [cli]`
- [ ] Restart policy: `restart: "no"`
- [ ] Backup service configurado (se incluído)
- [ ] Restore service configurado (se incluído)
- [ ] Sanitize service configurado (se incluído)
- [ ] Volumes de backup externos

### Banco de Dados (Se Aplicável)
- [ ] Serviço de banco configurado
- [ ] Volumes para persistência externos
- [ ] Healthcheck implementado
- [ ] Variáveis de ambiente via .env
- [ ] Network `stack` configurada
- [ ] Restart policy: `unless-stopped`

## ✅ Boas Práticas

### Portas
- [ ] Portas NÃO hardcoded (usar variáveis)
- [ ] Formato: `${VAR_PORT}:porta_interna`
- [ ] Porta interna consistente com a stack

### Environment Variables
- [ ] Variáveis sensíveis não hardcoded
- [ ] Referências a .env e .env.io
- [ ] Variável `IO_STAGE` usada para ambiente
- [ ] Nomes de variáveis claros e descritivos

### Imagens
- [ ] Imagens oficiais e versionadas
- [ ] Tags específicas (não `:latest`)
- [ ] Preferência por imagens alpine (menor)
- [ ] Build multi-stage para frontend (se aplicável)

### Networks
- [ ] Apenas network `stack` usada
- [ ] Network configurada no final do arquivo
- [ ] Serviços isolados em rede interna

### Volumes
- [ ] Volumes configurados no final do arquivo
- [ ] Nomes descritivos e consistentes
- [ ] Volumes de dados separados de volumes de código
- [ ] Documentação sobre criação manual

## ✅ Validação por Stack

### Node.js API
- [ ] Imagem base: `node:20-alpine` ou similar
- [ ] Comando: `npm start` ou equivalente
- [ ] Porta padrão: 3000 ou configurável
- [ ] node_modules em volume (se dev)
- [ ] Package.json e package-lock copiados

### Vue.js Frontend
- [ ] Multi-stage build (node + nginx)
- [ ] Stage 1: Build com Node
- [ ] Stage 2: Servir com Nginx
- [ ] Porta: 80
- [ ] Nginx.conf configurado
- [ ] Dist folder copiado para nginx

### React Frontend
- [ ] Multi-stage build (node + nginx)
- [ ] Stage 1: Build com Node
- [ ] Stage 2: Servir com Nginx
- [ ] Porta: 80
- [ ] Build folder copiado para nginx

### PHP Laravel
- [ ] PHP-FPM configurado
- [ ] Nginx para servir aplicação
- [ ] Composer dependencies instaladas
- [ ] Storage e cache em volumes
- [ ] Porta: 80

### .NET Blazor
- [ ] Imagem: mcr.microsoft.com/dotnet/aspnet:8.0
- [ ] Porta: 5000
- [ ] ASPNETCORE_ENVIRONMENT configurado
- [ ] Build artifacts copiados

## ✅ Integração com .env.io

- [ ] Arquivo .env.io existe no projeto
- [ ] Variáveis IO_* referenciadas corretamente
- [ ] COMPOSE_PROJECT_NAME calculado
- [ ] IO_STAGE usado para ambiente
- [ ] IO_VERSION disponível

## ✅ Documentação e Outputs

- [ ] Arquivo salvo em `{project-root}/docker-compose.yaml`
- [ ] Resumo de configuração exibido ao usuário
- [ ] Próximos passos comunicados
- [ ] Comandos para criar network/volumes fornecidos
- [ ] Script helper gerado (se solicitado)

## ✅ Comandos Helper (Opcional)

- [ ] Arquivo scripts/docker-helpers.sh criado
- [ ] Permissões de execução configuradas
- [ ] Comando create-network presente
- [ ] Comando create-volumes presente
- [ ] Comandos para serviços CLI (backup, restore, sanitize)
- [ ] Documentação inline nos scripts

## 🎯 Critérios de Sucesso

**O workflow é considerado bem-sucedido quando:**

1. ✅ Arquivo docker-compose.yaml criado em `{project-root}/`
2. ✅ Todas as 4 Verdades Fundamentais validadas
3. ✅ Stack corretamente identificada ou selecionada
4. ✅ Healthcheck implementado
5. ✅ Network externa configurada
6. ✅ Volumes externos configurados
7. ✅ Portas via variáveis
8. ✅ Serviços CLI com profiles (se incluídos)
9. ✅ Banco de dados configurado (se aplicável)
10. ✅ Usuário informado sobre próximos passos

## ⚠️ Casos de Falha Comum

### Campo 'version' Presente
- ❌ `version: '3.8'` no topo do arquivo
- ✅ Arquivo inicia com `services:`

### Container Name Hardcoded
- ❌ `container_name: meu-app`
- ✅ Sem campo `container_name`

### Network Não Externa
- ❌ `networks: stack: {}` (cria automaticamente)
- ✅ `networks: stack: external: true`

### Volumes Não Externos
- ❌ `volumes: data: {}` (cria automaticamente)
- ✅ `volumes: data: external: true`

### Portas Hardcoded
- ❌ `ports: - "3000:3000"`
- ✅ `ports: - "${APP_PORT}:3000"`

### Healthcheck Ausente
- ❌ Serviço sem healthcheck
- ✅ Healthcheck configurado com test, interval, timeout, retries

### Serviços CLI Sem Profile
- ❌ Serviço backup sem `profiles: [cli]`
- ✅ `profiles: [cli]` presente em serviços CLI

## 📝 Exemplo de docker-compose.yaml Válido (Mínimo)

```yaml
services:
  app:
    build: .
    restart: unless-stopped
    ports:
      - "${APP_PORT}:3000"
    environment:
      NODE_ENV: ${IO_STAGE}
    networks:
      - stack
    healthcheck:
      test: ["CMD", "wget", "--spider", "http://localhost:3000/health"]
      interval: 30s
      timeout: 10s
      retries: 3

networks:
  stack:
    name: ${IO_PROJECT}_${IO_APP}_${IO_STAGE}
    external: true
```

## 🔄 Pós-Geração

### Comandos Necessários
```bash
# Criar network externa
docker network create ${IO_PROJECT}_${IO_APP}_${IO_STAGE}

# Criar volumes (se necessário)
docker volume create ${IO_PROJECT}_${IO_APP}_${IO_STAGE}_data

# Subir aplicação
docker compose up -d

# Ver logs
docker compose logs -f

# Executar CLI services
docker compose --profile cli run --rm backup
```

### Validação Manual
- [ ] Executar `docker compose config` para validar sintaxe
- [ ] Verificar se network existe antes de subir
- [ ] Criar volumes manualmente se necessário
- [ ] Testar healthcheck: `docker compose ps`

---

**Checklist Version**: 1.0.0
**Última atualização**: 2025-10-21
**Módulo**: embrapa-io/setup/generate-docker-compose
**Conformidade**: 4 Verdades Fundamentais Embrapa I/O
