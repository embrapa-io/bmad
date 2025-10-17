# Generate .env.io Instructions

<critical>Communicate with {user_name} in {communication_language}</critical>

## Objetivo
Gerar os arquivos `.env.io` e `.env.io.example` com as variáveis da plataforma Embrapa I/O seguindo as convenções estabelecidas.

## Variáveis da Plataforma

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

## Steps

<step n="1" goal="Coletar informações do projeto">
  <ask>Qual o nome unix do projeto? (ex: mecaniza, agro-tools, meu-projeto)</ask>
  <action>Validar que o nome está em lowercase e usa apenas letras, números e hífens</action>
  <action>Armazenar resposta como io_project</action>
  <template-output>io_project</template-output>
</step>

<step n="2" goal="Coletar informações da aplicação">
  <ask>Qual o nome unix da aplicação? (ex: api, frontend, web)</ask>
  <action>Validar que o nome está em lowercase e usa apenas letras, números e hífens</action>
  <action>Armazenar resposta como io_app</action>
  <template-output>io_app</template-output>
</step>

<step n="3" goal="Coletar email do desenvolvedor">
  <ask>Qual o email do desenvolvedor? (formato: name.surname@embrapa.br)</ask>
  <action>Validar formato de email @embrapa.br</action>
  <action>Armazenar resposta como io_deployer</action>
  <template-output>io_deployer</template-output>
</step>

<step n="4" goal="Gerar valores calculados">
  <action>Calcular current_year no formato YY (2 dígitos do ano atual)</action>
  <action>Calcular current_month no formato M (mês atual sem zero à esquerda: 1-12)</action>
  <action>Calcular COMPOSE_PROJECT_NAME como: {{io_project}}_{{io_app}}_development</action>
  <action>Calcular IO_VERSION como: 0.{{current_year}}.{{current_month}}-dev.1</action>
  <example>
    Se hoje é outubro/2025:
    - current_year = 25
    - current_month = 10
    - IO_VERSION = 0.25.10-dev.1
  </example>
  <example>
    Se hoje é julho/2025:
    - current_year = 25
    - current_month = 7
    - IO_VERSION = 0.25.7-dev.1
  </example>
  <template-output>current_year</template-output>
  <template-output>current_month</template-output>
</step>

<step n="5" goal="Gerar arquivo .env.io">
  <action>Substituir todas as variáveis {{variable}} no template</action>
  <action>Salvar arquivo completo em {default_output_file}</action>
  <action>Mostrar conteúdo gerado para aprovação de {user_name}</action>
</step>

<step n="6" goal="Gerar arquivo .env.io.example">
  <action>Copiar conteúdo de .env.io para .env.io.example</action>
  <action>Substituir valores sensíveis por placeholders genéricos:</action>
  <action>- IO_DEPLOYER: your.email@embrapa.br</action>
  <action>- Manter todos os outros valores como referência</action>
  <action>Salvar em {example_output_file}</action>
  <action>Mostrar conteúdo gerado para aprovação de {user_name}</action>
</step>

<step n="7" goal="Orientar usuário sobre próximos passos">
  <action>Informar {user_name} que SENTRY_DSN e MATOMO_ID devem ser obtidos em https://dashboard.embrapa.io</action>
  <action>Explicar que .env.io NÃO deve ser commitado (adicionar ao .gitignore)</action>
  <action>Explicar que .env.io.example DEVE ser commitado como referência</action>
  <action>Lembrar que em ambientes remotos estas variáveis são injetadas automaticamente pela plataforma</action>
</step>

## Regras Importantes

1. **Formato de nomes unix**: lowercase, apenas letras, números e hífens
2. **IO_VERSION**: SEMPRE no formato `0.YY.M-dev.1` onde:
   - YY = ano com 2 dígitos (ex: 25 para 2025)
   - M = mês SEM zero à esquerda (1 a 12, não 01 a 12)
3. **COMPOSE_PROJECT_NAME**: SEMPRE concatenação exata de `${IO_PROJECT}_${IO_APP}_development`
4. **IO_STAGE**: SEMPRE `development` no ambiente local
5. **SENTRY_DSN**: Sempre placeholder `GET_IN_DASHBOARD` inicialmente
6. **MATOMO_TOKEN**: Sempre vazio inicialmente
7. **Validação de email**: Deve terminar com @embrapa.br

## Validações

- [ ] IO_PROJECT está em formato unix válido (lowercase, letras, números, hífens)
- [ ] IO_APP está em formato unix válido (lowercase, letras, números, hífens)
- [ ] IO_DEPLOYER é um email válido @embrapa.br
- [ ] IO_VERSION segue formato 0.YY.M-dev.1
- [ ] COMPOSE_PROJECT_NAME é concatenação correta
- [ ] Arquivo .env.io foi criado
- [ ] Arquivo .env.io.example foi criado com placeholders
- [ ] Usuário foi orientado sobre dashboard e .gitignore
