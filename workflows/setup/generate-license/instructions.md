# Generate LICENSE Instructions

<critical>Este é um workflow autônomo - execução silenciosa sem interação com usuário</critical>

## Objetivo
Gerar automaticamente o arquivo `LICENSE` na raiz do projeto com o copyright padrão da Embrapa.

## Características
- **Execução silenciosa**: Não solicita confirmação do usuário
- **Ano automático**: Calcula o ano atual (YYYY) automaticamente
- **Conteúdo fixo**: Copyright padrão da Embrapa

## Steps

<step n="1" goal="Calcular ano atual">
  <action>Obter ano atual no formato YYYY (4 dígitos)</action>
  <action>Armazenar em variável current_year</action>
  <example>Se hoje é 2025-10-10, então current_year = 2025</example>
  <template-output>current_year</template-output>
</step>

<step n="2" goal="Gerar arquivo LICENSE">
  <action>Substituir {{current_year}} no template</action>
  <action>Salvar arquivo completo em {default_output_file}</action>
  <critical>NÃO solicitar confirmação - criar automaticamente (workflow autônomo)</critical>
</step>

## Conteúdo do Arquivo

```
Copyright ⓒ YYYY Brazilian Agricultural Research Corporation (Embrapa). All rights reserved.
```

Onde `YYYY` é substituído pelo ano atual.

## Regras Importantes

1. **Execução silenciosa**: Este workflow NÃO interage com o usuário
2. **Ano automático**: Sempre usa o ano atual da execução
3. **Sem confirmação**: Cria o arquivo diretamente
4. **Sobrescreve**: Se LICENSE já existir, sobrescreve com o template padrão

## Uso por Agentes

Este workflow deve ser invocado automaticamente durante a estruturação inicial do projeto:

```xml
<step n="X" goal="Gerar arquivo LICENSE da Embrapa">
  <invoke-workflow>
    <path>{project-root}/bmad/embrapa-io/workflows/setup/generate-license/workflow.yaml</path>
    <description>Cria LICENSE com copyright da Embrapa</description>
  </invoke-workflow>
</step>
```

## Validação

- [ ] Arquivo LICENSE criado na raiz do projeto
- [ ] Conteúdo contém "Copyright ⓒ YYYY Brazilian Agricultural Research Corporation (Embrapa). All rights reserved."
- [ ] Ano (YYYY) corresponde ao ano atual
- [ ] Nenhuma interação com usuário ocorreu
