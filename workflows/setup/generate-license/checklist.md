# Checklist de Validação - Generate LICENSE

## ✅ Arquivo LICENSE

- [ ] Arquivo criado em `{project-root}/LICENSE`
- [ ] Copyright da Embrapa presente
- [ ] Ano atual calculado automaticamente
- [ ] Texto correto: `Copyright ⓒ YYYY Brazilian Agricultural Research Corporation (Embrapa). All rights reserved.`
- [ ] Símbolo de copyright (ⓒ) renderizado corretamente
- [ ] Encoding UTF-8 preservado
- [ ] Sem caracteres estranhos ou quebras de linha incorretas

## ✅ Conteúdo do Arquivo

### Texto Obrigatório
- [ ] Linha 1: `Copyright ⓒ {ANO_ATUAL} Brazilian Agricultural Research Corporation (Embrapa).`
- [ ] Linha 2: `All rights reserved.`
- [ ] Quebra de linha entre as linhas (duas linhas distintas)
- [ ] Sem espaços extras no início ou fim das linhas

### Validação do Ano
- [ ] Ano substituído corretamente (não placeholder)
- [ ] Ano é o ano corrente (YYYY formato completo)
- [ ] Exemplo para 2025: `Copyright ⓒ 2025 Brazilian Agricultural Research Corporation (Embrapa).`
- [ ] Não usar ano fixo (deve ser dinâmico)

### Formatação
- [ ] Texto em inglês (padrão internacional)
- [ ] Capitalização correta: "Copyright", "Brazilian", "Agricultural", "Research", "Corporation", "Embrapa", "All"
- [ ] Pontuação correta (ponto final após "Embrapa." e após "reserved.")

## ✅ Integração com Git

- [ ] Arquivo LICENSE está na raiz do projeto
- [ ] Arquivo LICENSE NÃO está no .gitignore (deve ser versionado)
- [ ] Arquivo LICENSE foi adicionado ao repositório (se git inicializado)
- [ ] Conteúdo visível no repositório

## ✅ Conformidade Legal

- [ ] Texto segue padrão oficial da Embrapa
- [ ] Copyright atribuído corretamente à Embrapa
- [ ] Nome completo da instituição presente: "Brazilian Agricultural Research Corporation (Embrapa)"
- [ ] Não usar abreviações: "Empresa Brasileira de Pesquisa Agropecuária" (versão em português não é usada)
- [ ] Manter nome oficial em inglês para padrão internacional

## ✅ Execução Silenciosa

- [ ] Workflow executado SEM interação com usuário
- [ ] Nenhuma pergunta feita durante execução
- [ ] Ano calculado automaticamente (via sistema)
- [ ] Arquivo gerado e salvo automaticamente
- [ ] Apenas confirmação final de criação mostrada

## ✅ Validação de Sobrescrita

- [ ] Se arquivo LICENSE já existe, verificar se deve sobrescrever
- [ ] Avisar usuário se LICENSE existente tem conteúdo diferente
- [ ] Não sobrescrever silenciosamente se conteúdo personalizado existe
- [ ] Opção de preservar LICENSE personalizado (se aplicável)

## 🎯 Critérios de Sucesso

**O workflow é considerado bem-sucedido quando:**

1. ✅ Arquivo LICENSE criado em `{project-root}/LICENSE`
2. ✅ Ano atual inserido corretamente (dinâmico, não fixo)
3. ✅ Texto exato do copyright da Embrapa presente
4. ✅ Símbolo ⓒ renderizado corretamente
5. ✅ Encoding UTF-8 preservado
6. ✅ Arquivo versionado no git (não no .gitignore)
7. ✅ Execução foi silenciosa (sem prompts ao usuário)
8. ✅ Confirmação de criação exibida ao usuário

## ⚠️ Casos de Falha Comum

### Ano Incorreto
- ❌ `Copyright ⓒ YYYY ...` (placeholder não substituído)
- ❌ `Copyright ⓒ 2024 ...` (ano fixo, não dinâmico)
- ✅ `Copyright ⓒ 2025 ...` (ano atual em 2025, correto)

### Nome da Instituição
- ❌ `Copyright ⓒ 2025 Embrapa. All rights reserved.` (nome incompleto)
- ❌ `Copyright ⓒ 2025 Empresa Brasileira de Pesquisa Agropecuária (Embrapa). All rights reserved.` (português, incorreto)
- ✅ `Copyright ⓒ 2025 Brazilian Agricultural Research Corporation (Embrapa). All rights reserved.` (correto)

### Símbolo de Copyright
- ❌ `Copyright (c) 2025 ...` (símbolo ASCII, não Unicode)
- ❌ `Copyright © 2025 ...` (símbolo diferente)
- ✅ `Copyright ⓒ 2025 ...` (símbolo correto: U+24D2)

### Formatação
- ❌ Arquivo com uma linha única (sem quebra entre copyright e "All rights reserved")
- ❌ Espaços extras ou tabulações
- ✅ Duas linhas distintas, sem espaços extras

## 📝 Exemplo de Arquivo LICENSE Válido

```
Copyright ⓒ 2025 Brazilian Agricultural Research Corporation (Embrapa).
All rights reserved.
```

**Estrutura**:
- Linha 1: Copyright completo
- Linha 2: All rights reserved
- Total: 2 linhas

## 🔄 Manutenção

### Atualização Anual
- [ ] Ano é calculado dinamicamente (não precisa atualização manual)
- [ ] Sistema usa data do sistema operacional
- [ ] Formato: 4 dígitos (YYYY)

### Versionamento
- [ ] Arquivo LICENSE sempre versionado no git
- [ ] Mudanças no LICENSE devem ser commitadas
- [ ] Histórico de alterações preservado

---

**Checklist Version**: 1.0.0
**Última atualização**: 2025-10-21
**Módulo**: embrapa-io/setup/generate-license
**Execução**: Silenciosa (sem interação)
