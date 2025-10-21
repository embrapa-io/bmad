# Generate .embrapa/settings.json - Instruções

<critical>The workflow execution engine is governed by: {project-root}/bmad/core/tasks/workflow.xml</critical>
<critical>You MUST have already loaded and processed: {project-root}/bmad/embrapa-io/workflows/setup/generate-settings-json/workflow.yaml</critical>
<critical>Communicate in {communication_language} throughout the workflow</critical>

<workflow>

<step n="1" goal="Verificar dependências e identificar stack">
<action>Verificar se .env.io existe (necessário para variáveis IO_*)</action>

<check if=".env.io exists">
<action>Ler variáveis: IO_PROJECT, IO_APP, IO_VERSION</action>
</check>

<action>Detectar stack automaticamente através de arquivos:</action>
- package.json → Node.js ou Frontend
- requirements.txt → Python
- composer.json → PHP
- *.csproj → .NET

<action>Exibir detecção para {user_name}</action>
</step>

<step n="2" goal="Coletar informações do projeto">
<ask>Qual o nome/label da aplicação? (ex: "API de Catálogo de Produtos")</ask>
<action>Armazenar como {{app_label}}</action>

<ask>Breve descrição da aplicação (1 linha):</ask>
<action>Armazenar como {{app_description}}</action>

<ask>Nome do mantenedor principal:</ask>
<action>Armazenar como {{maintainer_name}}</action>

<ask>Email do mantenedor:</ask>
<action>Validar formato de email</action>
<action>Armazenar como {{maintainer_email}}</action>

<ask optional="true">Telefone do mantenedor (opcional):</ask>
<action>Armazenar como {{maintainer_phone}}</action>
</step>

<step n="3" goal="Definir variáveis de ambiente">
<action>Carregar template base conforme stack detectada</action>

<action>Apresentar variáveis padrão da stack:</action>
- ENVIRONMENT (default: test, release: production)
- PORT
- BASE_URL
- SECRET
- Outras específicas da stack

<ask>Deseja adicionar variáveis personalizadas além das padrão? (s/n)</ask>

<check if="resposta == 's'">
<action>Coletar variáveis adicionais:</action>
<ask>Nome da variável:</ask>
<ask>Tipo (TEXT, PORT, SECRET, PASSWORD, VOLUME, EMPTY):</ask>
<ask>Valor padrão (deixe vazio se não aplicável):</ask>
<ask>Adicionar mais variáveis? (s/n)</ask>
</check>
</step>

<step n="4" goal="Gerar settings.json">
<action>Construir objeto JSON com estrutura Embrapa I/O:</action>

```json
{
  "boilerplate": "_",
  "platform": "{{platform}}",
  "label": "{{app_label}}",
  "description": "{{app_description}}",
  "references": [{{references}}],
  "maintainers": [
    {
      "name": "{{maintainer_name}}",
      "email": "{{maintainer_email}}",
      "phone": "{{maintainer_phone}}"
    }
  ],
  "variables": {
    "default": [{{default_variables}}],
    "alpha": [],
    "beta": [],
    "release": [{{release_variables}}]
  },
  "orchestrators": ["DockerCompose"]
}
```

<template-output>settings_json_content</template-output>
</step>

<step n="5" goal="Validar e salvar arquivo">
<action>Validar JSON gerado (sintaxe válida)</action>
<action>Criar diretório .embrapa/ se não existir</action>
<action>Salvar em {project-root}/.embrapa/settings.json</action>

<action>Exibir resumo para {user_name}:</action>

**Arquivo .embrapa/settings.json criado!**

**Localização**: {project-root}/.embrapa/settings.json
**Platform**: {{platform}}
**Label**: {{app_label}}
**Variáveis padrão**: {{variable_count}}
**Mantenedores**: 1

**Próximos passos**:
1. Revisar arquivo gerado
2. Ajustar variáveis se necessário
3. Committar no repositório
</step>

</workflow>
