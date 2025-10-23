---
last-redoc-date: 2025-10-22
---

# Generate Docker Compose Workflow

Workflow interativo que gera arquivos `docker-compose.yaml` conformes com a plataforma Embrapa I/O, adaptando-se automaticamente à stack tecnológica do projeto.

Este workflow realiza detecção automática da tecnologia utilizada (Node.js, Vue.js, React, PHP Laravel, .NET, etc.) através da análise de arquivos indicadores como `package.json`, `composer.json` e `*.csproj`. Com base na stack identificada, o workflow carrega templates apropriados e configura serviços Docker específicos, incluindo banco de dados quando necessário.

O diferencial deste workflow está na validação rigorosa contra as 4 Verdades Fundamentais da plataforma Embrapa I/O: ausência do campo `version` no compose, network `stack` externa, volumes externos com nomenclatura padronizada, e ausência de `container_name` nos serviços. Além disso, oferece suporte opcional para serviços CLI (backup, restore, sanitize) através de Docker Compose profiles, permitindo operações de manutenção sem modificar o compose principal.

A dependência do arquivo `.env.io` (gerado pelo workflow `generate-env-io`) é obrigatória, pois as variáveis `IO_PROJECT`, `IO_APP`, `IO_STAGE` e `IO_VERSION` são essenciais para nomenclatura de recursos e configuração de rede. O workflow também suporta multi-stage builds para aplicações frontend, otimizando imagens Docker com nginx como servidor de produção.

## Usage

```xml
<invoke-workflow>
  <path>{project-root}/bmad/embrapa-io/workflows/setup/generate-docker-compose/workflow.yaml</path>
  <description>Gera docker-compose.yaml conforme com Embrapa I/O</description>
</invoke-workflow>
```

## Inputs

- **Stack Detection**: Automática através de arquivos de projeto (pode ser sobrescrita manualmente)
- **Application Port**: Porta principal da aplicação (validada entre 1024-65535)
- **Healthcheck Endpoint**: Caminho do endpoint de health (padrão: `/health`)
- **Database Type**: MongoDB, PostgreSQL, MySQL, SQL Server, ou nenhum
- **CLI Services**: Opcional - inclusão de serviços backup/restore/sanitize

## Outputs

- **File**: `{project-root}/docker-compose.yaml`
- **Validation**: Conformidade automática com as 4 Verdades Fundamentais
- **Scripts**: Opcionalmente gera `scripts/docker-helpers.sh` com comandos úteis
