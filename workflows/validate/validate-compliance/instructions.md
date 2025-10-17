# Validate Compliance - Instructions

<critical>Communicate with {user_name} in {communication_language}</critical>
<critical>Read knowledge files before starting validation</critical>

## Objetivo

Executar validação completa de conformidade de um projeto com a plataforma Embrapa I/O, gerando relatório detalhado com score e sugestões de correção.

## Conhecimento Necessário

Antes de iniciar, carregar e compreender:
- `{project-root}/bmad/embrapa-io/knowledge/embrapa-io-fundamentals.md` - As 4 verdades fundamentais
- `{project-root}/bmad/embrapa-io/knowledge/embrapa-io-validation.md` - Todas as 38 regras de validação
- `{project-root}/bmad/embrapa-io/knowledge/embrapa-io-workflows.md` - Adaptação por tipo de projeto

## Steps

<step n="1" goal="Coletar parâmetros de validação">
  <ask>Qual o caminho absoluto do projeto a ser validado?</ask>
  <action>Armazenar como project_path</action>

  <ask>Quais níveis de severidade validar? (CRITICAL, HIGH, MEDIUM, LOW ou ALL para todos)</ask>
  <action>Armazenar como severity_levels (array)</action>
  <action>Se "ALL", usar: ["CRITICAL", "HIGH", "MEDIUM", "LOW"]</action>

  <ask>Aplicar correções automáticas quando possível? (y/n)</ask>
  <action>Armazenar como auto_fix (boolean)</action>

  <action>Extrair nome do projeto do caminho para usar em outputs</action>
  <action>Armazenar como project_name</action>
</step>

<step n="2" goal="Detectar tipo de projeto">
  <action>Verificar existência dos arquivos no project_path:</action>
  <action>- docker-compose.yaml → hasDockerCompose</action>
  <action>- .embrapa/settings.json → hasSettings</action>
  <action>- .env.io.example → hasEnvIo</action>

  <action>Determinar tipo do projeto:</action>
  <check if="hasSettings AND hasEnvIo">
    <action>project_type = "ALREADY_COMPLIANT"</action>
  </check>
  <check if="hasDockerCompose AND NOT (hasSettings AND hasEnvIo)">
    <action>project_type = "EXISTING"</action>
  </check>
  <check if="NOT hasDockerCompose">
    <action>project_type = "NEW"</action>
  </check>

  <check if="project_type == NEW">
    <action>Informar {user_name}: Projeto novo detectado - validação não aplicável</action>
    <action>Sugerir usar workflow create-* apropriado</action>
    <action>Encerrar workflow</action>
  </check>

  <action>Informar {user_name}: Tipo de projeto detectado: {{project_type}}</action>
  <action>Listar arquivos detectados</action>
</step>

<step n="3" goal="Validar docker-compose.yaml">
  <action>Ler arquivo {project_path}/docker-compose.yaml completo</action>
  <action>Executar 14 validações conforme embrapa-io-validation.md seção "Validação 1":</action>

  <action>CRITICAL (ID 1.1-1.5):</action>
  <action>- 1.1: Arquivo existe</action>
  <action>- 1.2: Campo 'version' ausente (Compose v2+)</action>
  <action>- 1.3: Network 'stack' existe</action>
  <action>- 1.4: Network 'stack' é externa (external: true)</action>
  <action>- 1.5: Nome correto: ${IO_PROJECT}_${IO_APP}_stack</action>

  <action>HIGH (ID 1.6-1.10):</action>
  <action>- 1.6: Todos serviços conectados à network 'stack'</action>
  <action>- 1.7: Nenhum 'container_name' definido</action>
  <action>- 1.8: Volumes com 'external: true'</action>
  <action>- 1.9: Serviços com 'restart: unless-stopped'</action>
  <action>- 1.10: Serviços com 'healthcheck' configurado</action>

  <action>MEDIUM (ID 1.11-1.13):</action>
  <action>- 1.11: Portas via variáveis de ambiente</action>
  <action>- 1.12: Serviços CLI com profiles corretos</action>
  <action>- 1.13: Volume backup existe</action>

  <action>LOW (ID 1.14):</action>
  <action>- 1.14: Serviços CLI presentes (backup, restore, sanitize)</action>

  <action>Para cada erro encontrado, criar objeto:</action>
  <example>
  {
    "id": "1.6",
    "severity": "HIGH",
    "category": "docker-compose",
    "message": "Serviço 'api' não está conectado à network 'stack'",
    "location": "docker-compose.yaml:15",
    "solution": "Adicionar 'stack' em 'networks:' do serviço",
    "auto_fixable": true
  }
  </example>

  <action>Armazenar todos os erros em docker_compose_errors (array)</action>
  <action>Calcular status: compliant (0 erros) | partial (só MEDIUM/LOW) | non-compliant (CRITICAL/HIGH)</action>
</step>

<step n="4" goal="Validar arquivos .env">
  <action>Executar 8 validações conforme embrapa-io-validation.md seção "Validação 2":</action>

  <action>CRITICAL (ID 2.1-2.3):</action>
  <action>- 2.1: .env.io.example existe</action>
  <action>- 2.2: .env.example existe</action>
  <action>- 2.3: Sem duplicação de variáveis entre arquivos</action>

  <action>HIGH (ID 2.4-2.6):</action>
  <action>- 2.4: Variáveis obrigatórias presentes (IO_PROJECT, IO_APP, IO_STAGE, IO_VERSION, IO_DEPLOYER)</action>
  <action>- 2.5: Valores sem espaços ou aspas desnecessárias</action>
  <action>- 2.6: Convenção de nomenclatura (UPPER_SNAKE_CASE)</action>

  <action>MEDIUM (ID 2.7-2.8):</action>
  <action>- 2.7: .gitignore ignora arquivos .env (não .env.*.example)</action>
  <action>- 2.8: Variáveis sensíveis com comentário de documentação</action>

  <action>Armazenar erros em env_files_errors (array)</action>
  <action>Calcular status da categoria</action>
</step>

<step n="5" goal="Validar .embrapa/settings.json">
  <action>Ler arquivo {project_path}/.embrapa/settings.json</action>
  <action>Executar 8 validações conforme embrapa-io-validation.md seção "Validação 3":</action>

  <action>CRITICAL (ID 3.1-3.3):</action>
  <action>- 3.1: Arquivo existe</action>
  <action>- 3.2: JSON válido (parse sem erros)</action>
  <action>- 3.3: Campos obrigatórios: boilerplate, platform, label, description, references, maintainers, variables, orchestrators</action>

  <action>HIGH (ID 3.4-3.6):</action>
  <action>- 3.4: platform válida (nodejs|python|php|java|golang|ruby|dotnet)</action>
  <action>- 3.5: variables.default presente e não-vazio</action>
  <action>- 3.6: Tipos válidos (TEXT, PASSWORD, SECRET, PORT, VOLUME, EMPTY)</action>

  <action>MEDIUM (ID 3.7-3.8):</action>
  <action>- 3.7: Valores sem espaços ou aspas nos defaults</action>
  <action>- 3.8: references array não-vazio</action>

  <action>Armazenar erros em settings_errors (array)</action>
  <action>Calcular status da categoria</action>
</step>

<step n="6" goal="Validar integrações">
  <action>Executar 5 validações conforme embrapa-io-validation.md seção "Validação 4":</action>

  <action>HIGH (ID 4.1-4.2):</action>
  <action>- 4.1: Sentry configurado (DSN em .env, código de inicialização presente)</action>
  <action>- 4.2: Matomo tracking implementado (ID e código presentes)</action>

  <action>MEDIUM (ID 4.3-4.4):</action>
  <action>- 4.3: Logo Embrapa presente em assets/public</action>
  <action>- 4.4: SonarQube configurado se projeto backend</action>

  <action>LOW (ID 4.5):</action>
  <action>- 4.5: Grafana Loki configurado (opcional mas recomendado)</action>

  <action>Armazenar erros em integrations_errors (array)</action>
  <action>Calcular status da categoria</action>
</step>

<step n="7" goal="Calcular compliance score">
  <action>Consolidar todos os erros: all_errors = docker_compose_errors + env_files_errors + settings_errors + integrations_errors</action>

  <action>Contar por severidade:</action>
  <action>- critical_count = erros com severity "CRITICAL"</action>
  <action>- high_count = erros com severity "HIGH"</action>
  <action>- medium_count = erros com severity "MEDIUM"</action>
  <action>- low_count = erros com severity "LOW"</action>
  <action>- total_errors = soma de todos</action>

  <action>Calcular compliance_score:</action>
  <check if="critical_count > 0 OR high_count > 3">
    <action>compliance_score = "LOW"</action>
  </check>
  <check if="high_count > 0 AND high_count <= 3">
    <action>compliance_score = "MEDIUM"</action>
  </check>
  <check if="high_count == 0 AND critical_count == 0">
    <action>compliance_score = "HIGH"</action>
  </check>

  <action>Informar {user_name}: Score calculado: {{compliance_score}}</action>
</step>

<step n="8" goal="Gerar relatório JSON">
  <action>Criar estrutura do relatório JSON:</action>
  <example>
  {
    "project": {
      "name": "{{project_name}}",
      "path": "{{project_path}}",
      "type": "{{project_type}}"
    },
    "validation": {
      "timestamp": "{{date}}",
      "score": "{{compliance_score}}",
      "summary": {
        "critical": {{critical_count}},
        "high": {{high_count}},
        "medium": {{medium_count}},
        "low": {{low_count}},
        "total": {{total_errors}}
      }
    },
    "results": {
      "docker_compose": {...},
      "env_files": {...},
      "settings_json": {...},
      "integrations": {...}
    },
    "all_errors": [...]
  }
  </example>

  <action>Salvar em {validation_report_json}</action>
  <action>Informar {user_name}: Relatório JSON salvo em {{validation_report_json}}</action>
</step>

<step n="9" goal="Gerar relatório Markdown">
  <action>Criar relatório humanizado em Markdown:</action>

  <action>Incluir cabeçalho com projeto, data, score (com emoji: 🟢 HIGH / 🟡 MEDIUM / 🔴 LOW)</action>
  <action>Incluir resumo executivo com contagem de validações</action>
  <action>Incluir detalhamento por categoria com status e erros</action>
  <action>Incluir seção "Próximos Passos" priorizada (ALTA/MÉDIA/BAIXA)</action>

  <action>Salvar em {validation_report_md}</action>
  <action>Informar {user_name}: Relatório Markdown salvo em {{validation_report_md}}</action>
</step>

<step n="10" goal="Sugerir correções" optional="true">
  <action>Para cada erro em all_errors:</action>
  <action>- Apresentar descrição clara do problema</action>
  <action>- Fornecer solução específica com exemplo de código</action>
  <action>- Indicar se é auto_fixable</action>
  <action>- Referenciar workflow relacionado se aplicável</action>

  <action>Agrupar sugestões por prioridade (CRITICAL → HIGH → MEDIUM → LOW)</action>
  <action>Apresentar para {user_name} em {communication_language}</action>
</step>

<step n="11" goal="Aplicar correções automáticas" if="auto_fix == true AND tem erros auto_fixable">
  <action>Filtrar erros com auto_fixable == true</action>

  <action>Para cada erro auto-fixável:</action>
  <action>1. Criar backup do arquivo original ({{arquivo}}.bak)</action>
  <action>2. Aplicar correção específica</action>
  <action>3. Validar sintaxe do arquivo modificado</action>
  <action>4. Registrar ação em log de mudanças</action>

  <action>Correções automáticas possíveis:</action>
  <action>- Remover campo 'version' do docker-compose.yaml</action>
  <action>- Adicionar 'external: true' a networks e volumes</action>
  <action>- Criar arquivos .env.example ausentes com estrutura base</action>
  <action>- Adicionar entradas ao .gitignore</action>
  <action>- Criar .embrapa/settings.json com estrutura mínima</action>

  <action>Informar {user_name}: {{count}} correções automáticas aplicadas</action>
  <action>Listar arquivos modificados e backups criados</action>

  <ask>Deseja re-executar validação para verificar correções? (y/n)</ask>
  <check if="resposta == yes">
    <goto step="2">Re-executar validação</goto>
  </check>
</step>

<step n="12" goal="Finalizar e apresentar resultados">
  <action>Apresentar resumo final para {user_name} em {communication_language}:</action>

  <action>🎯 Validação de Conformidade Embrapa I/O</action>
  <action>📂 Projeto: {{project_name}}</action>
  <action>📍 Caminho: {{project_path}}</action>
  <action>🏷️ Tipo: {{project_type}}</action>
  <action>📊 Score: {{compliance_score_emoji}} {{compliance_score}}</action>

  <action>📝 Relatórios gerados:</action>
  <action>- JSON: {{validation_report_json}}</action>
  <action>- Markdown: {{validation_report_md}}</action>

  <action>💡 Próximos passos:</action>
  <check if="critical_count > 0">
    <action>1. URGENTE: Corrigir {{critical_count}} erros CRITICAL</action>
  </check>
  <check if="high_count > 0">
    <action>2. ALTA PRIORIDADE: Corrigir {{high_count}} erros HIGH</action>
  </check>
  <check if="medium_count > 0">
    <action>3. MÉDIA PRIORIDADE: Considerar {{medium_count}} melhorias MEDIUM</action>
  </check>
  <check if="low_count > 0">
    <action>4. BAIXA PRIORIDADE: Implementar {{low_count}} sugestões LOW</action>
  </check>
</step>

## Notas Importantes

- **SEMPRE** ler arquivos completos antes de validar
- **SEMPRE** seguir as 38 regras de validação documentadas
- **SEMPRE** gerar relatórios nos dois formatos (JSON + Markdown)
- **SEMPRE** indicar próximos passos priorizados
- **NUNCA** inventar regras não documentadas
- **NUNCA** aplicar auto-fix sem criar backup

---

**Versão**: 2.0.0 (BMAD Core v6)
**Conversão**: 2025-10-17
**Autor**: Módulo Embrapa I/O BMAD
