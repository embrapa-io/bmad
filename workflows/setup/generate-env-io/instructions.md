# Generate .env.io - Instruções de Geração Interativa

<critical>The workflow execution engine is governed by: {project-root}/bmad/core/tasks/workflow.xml</critical>
<critical>You MUST have already loaded and processed: {project-root}/bmad/embrapa-io/workflows/setup/generate-env-io/workflow.yaml</critical>
<critical>This is an INTERACTIVE workflow - requires user input</critical>
<critical>Communicate in {communication_language} throughout execution</critical>

<workflow>

<step n="1" goal="Coletar informações do projeto">
<ask>Qual o nome unix do projeto? (ex: mecaniza, agro-tools, meu-projeto)</ask>
<action>Validar que o nome está em lowercase e usa apenas letras, números e hífens</action>
<action>Armazenar resposta como {{io_project}}</action>

<check if="nome inválido (uppercase ou caracteres especiais)">
<action>Solicitar novo nome explicando: deve ser lowercase, apenas letras, números e hífens</action>
<action>Repetir até receber nome válido</action>
</check>

<template-output>io_project</template-output>
</step>

<step n="2" goal="Coletar informações da aplicação">
<ask>Qual o nome unix da aplicação? (ex: api, frontend, web)</ask>
<action>Validar que o nome está em lowercase e usa apenas letras, números e hífens</action>
<action>Armazenar resposta como {{io_app}}</action>

<check if="nome inválido">
<action>Solicitar novo nome com mesmas regras de validação do Step 1</action>
</check>

<template-output>io_app</template-output>
</step>

<step n="3" goal="Coletar email do desenvolvedor">
<ask>Qual o email do desenvolvedor? (formato obrigatório: name.surname@embrapa.br)</ask>
<action>Validar formato de email @embrapa.br</action>
<action>Armazenar resposta como {{io_deployer}}</action>

<check if="email inválido (não termina com @embrapa.br)">
<action>Solicitar email novamente explicando formato obrigatório</action>
<action>Repetir até receber email válido @embrapa.br</action>
</check>

<template-output>io_deployer</template-output>
</step>

<step n="4" goal="Gerar valores calculados automaticamente">
<action>Obter data atual do sistema</action>
<action>Calcular {{current_year}} no formato YY (2 dígitos: ano atual - 2000)</action>
<action>Calcular {{current_month}} no formato M (mês sem zero à esquerda: 1-12)</action>
<action>Calcular COMPOSE_PROJECT_NAME como: {{io_project}}_{{io_app}}_development</action>
<action>Calcular IO_VERSION como: 0.{{current_year}}.{{current_month}}-dev.1</action>

**Importante sobre IO_VERSION**:
- YY = ano com 2 dígitos (ex: 25 para 2025, 26 para 2026)
- M = mês SEM zero à esquerda (1, 2, 3... 10, 11, 12)

**Exemplos**:
- Outubro/2025: IO_VERSION = 0.25.10-dev.1 (current_year=25, current_month=10)
- Julho/2025: IO_VERSION = 0.25.7-dev.1 (current_year=25, current_month=7)
- Janeiro/2026: IO_VERSION = 0.26.1-dev.1 (current_year=26, current_month=1)

<template-output>current_year</template-output>
<template-output>current_month</template-output>
<template-output>compose_project_name</template-output>
<template-output>io_version</template-output>
</step>

<step n="5" goal="Gerar arquivo .env.io">
<action>Carregar template de: {installed_path}/template.env.io</action>
<action>Substituir todas as variáveis:</action>
<action>- {{io_project}} pelo valor coletado no Step 1</action>
<action>- {{io_app}} pelo valor coletado no Step 2</action>
<action>- {{io_deployer}} pelo valor coletado no Step 3</action>
<action>- {{compose_project_name}} pelo valor calculado no Step 4</action>
<action>- {{io_version}} pelo valor calculado no Step 4</action>
<action>Salvar arquivo completo em {default_output_file} (resolve para {project-root}/.env.io)</action>

<check if="arquivo gerado com sucesso">
<action>Mostrar conteúdo completo para {user_name} em {communication_language}:</action>

**Arquivo .env.io gerado**:
```
[mostrar conteúdo completo do arquivo]
```
</check>

<check if="erro ao gerar">
<action>Reportar erro em {communication_language}</action>
<action>Informar caminho tentado e motivo da falha</action>
</check>

<template-output>env_io_content</template-output>
</step>

<step n="6" goal="Gerar arquivo .env.io.example">
<action>Copiar conteúdo gerado no Step 5</action>
<action>Substituir valor sensível: {{io_deployer}} → your.email@embrapa.br</action>
<action>Manter todos os outros valores como referência para desenvolvedores</action>
<action>Salvar em {example_output_file} (resolve para {project-root}/.env.io.example)</action>

**Importante**: .env.io.example serve como referência versionada, contendo valores de exemplo mas mantendo a estrutura real.

<check if="arquivo gerado com sucesso">
<action>Mostrar conteúdo para {user_name}:</action>

**Arquivo .env.io.example gerado**:
```
[mostrar conteúdo]
```
</check>

<template-output>env_io_example_content</template-output>
</step>

<step n="7" goal="Orientar usuário sobre próximos passos e boas práticas">
<action>Informar {user_name} em {communication_language}:</action>

**✅ Arquivos gerados com sucesso!**

**📋 Próximos passos obrigatórios**:

1. **Obter credenciais reais**:
   - Acesse https://dashboard.embrapa.io
   - Obtenha o SENTRY_DSN específico do seu projeto
   - Obtenha o MATOMO_ID (se diferente de 522)
   - Atualize estes valores no arquivo .env.io local

2. **Configurar Git corretamente**:
   - ❌ .env.io NÃO deve ser commitado (adicionar ao .gitignore)
   - ✅ .env.io.example DEVE ser commitado como referência

3. **Entender ambientes**:
   - Arquivo .env.io é apenas para desenvolvimento local
   - Em ambientes remotos (alpha, beta, release) a plataforma Embrapa I/O injeta estas variáveis automaticamente
   - Valores locais servem para simular comportamento da plataforma

**📌 Regras da Plataforma Embrapa I/O**:
- COMPOSE_PROJECT_NAME: SEMPRE concatenação `${IO_PROJECT}_${IO_APP}_development`
- IO_STAGE: SEMPRE `development` no ambiente local
- IO_VERSION: Formato `0.YY.M-dev.1` (YY=ano 2 dígitos, M=mês sem zero)
- COMPOSE_PROFILES: SEMPRE `development` no ambiente local
- IO_SERVER: SEMPRE `localhost` no ambiente local

<action>Perguntar se {user_name} deseja ajuda adicional com configuração do .gitignore</action>
</step>

</workflow>

## 📋 Variáveis da Plataforma

O arquivo `.env.io` contém SEMPRE as mesmas variáveis, que são injetadas pela plataforma DevOps nos ambientes remotos:

- **COMPOSE_PROJECT_NAME**: Concatenação `${IO_PROJECT}_${IO_APP}_development`
- **COMPOSE_PROFILES**: Sempre `development` no ambiente local
- **IO_SERVER**: Sempre `localhost` no ambiente local
- **IO_PROJECT**: Nome unix do projeto (lowercase, hífens permitidos)
- **IO_APP**: Nome unix da aplicação (lowercase, hífens permitidos)
- **IO_STAGE**: Sempre `development` no ambiente local
- **IO_VERSION**: Formato `0.YY.M-dev.1` (YY=ano 2 dígitos, M=mês sem zero à esquerda)
- **IO_DEPLOYER**: Email do desenvolvedor no formato `name.surname@embrapa.br`
- **SENTRY_DSN**: Placeholder `GET_IN_DASHBOARD` (obtido em https://dashboard.embrapa.io)
- **MATOMO_ID**: Valor padrão `522` (pode ser ajustado conforme dashboard)
- **MATOMO_TOKEN**: Sempre vazio inicialmente

## 🎯 Regras de Validação

1. **Formato de nomes unix**: lowercase, apenas letras, números e hífens
2. **IO_VERSION**: SEMPRE no formato `0.YY.M-dev.1` onde:
   - YY = ano com 2 dígitos (ex: 25 para 2025)
   - M = mês SEM zero à esquerda (1 a 12, não 01 a 12)
3. **COMPOSE_PROJECT_NAME**: SEMPRE concatenação exata de `${IO_PROJECT}_${IO_APP}_development`
4. **IO_STAGE**: SEMPRE `development` no ambiente local
5. **SENTRY_DSN**: Sempre placeholder `GET_IN_DASHBOARD` inicialmente
6. **MATOMO_TOKEN**: Sempre vazio inicialmente
7. **Validação de email**: Deve terminar com @embrapa.br

## 🔧 Uso por Agentes

Este workflow deve ser invocado durante setup inicial de projetos Embrapa I/O:

```xml
<step n="X" goal="Gerar variáveis de ambiente Embrapa I/O">
  <invoke-workflow>
    <path>{project-root}/bmad/embrapa-io/workflows/setup/generate-env-io/workflow.yaml</path>
    <description>Cria .env.io e .env.io.example com variáveis da plataforma (workflow interativo)</description>
  </invoke-workflow>
</step>
```

**Características**:
- Workflow interativo (requer input do usuário)
- Valida nomes unix e emails
- Calcula versão automaticamente
- Gera arquivo de exemplo para versionamento
- Orienta sobre boas práticas Git e dashboard
