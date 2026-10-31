---
name: orbite-global-settings
description: Consulte e altere com segurança campos simples, coleções e outras configurações globais do Site conectado por um MCP da Orbite, sem acessar credenciais.
---

# Orbite Global Settings

Gerencie o Design System do Site sem inventar chaves e sem alterar configurações fora do pedido.

## Regras obrigatórias

- A chamada desta skill não autoriza alterações. Primeiro consulte, explique a proposta e peça autorização.
- Confirme a conexão MCP, o ambiente e o Site identificado por ela antes de trabalhar. Se houver dúvida, não altere nada.
- Use somente o Site identificado pela conexão MCP escolhida.
- Nunca aceite outra URL enviada no chat nem troque de conexão silenciosamente.
- Use as ferramentas contextuais. Não use `update_global_settings` em um fluxo novo.
- Nunca leia nem altere tokens de acesso, senhas, segredos, credenciais, campos internos ou campos marcados como somente leitura.
- Essa proteção também vale para históricos e pontos antigos. Nunca tente descobrir um dado protegido por diferenças entre versões.
- Atualize apenas uma área por vez e somente os campos necessários.
- Trate a configuração da Agenda e os agendamentos como recursos separados.
- Uma restauração afeta o Design System inteiro e exige autorização explícita.

## Fluxo

1. Leia [references/mcp.md](references/mcp.md).
2. Escolha o fluxo correto:
   - campos de uma área, inclusive listas marcadas como `writable`: `list_global_settings_areas` e `get_global_settings_context`;
   - Equipe, Depoimentos, Tabela de preços, Formulários, configuração da Agenda, Dúvidas comuns e Logos: `list_global_collections` e `get_global_collection_context`;
   - agendamentos de visitantes: use somente as ferramentas próprias de agendamentos.
3. Quando o usuário citar uma versão anterior, data, hora ou característica antiga, consulte primeiro a evolução dos campos ou a lista de pontos históricos. Abra somente o contexto das candidatas e compare com o estado atual.
4. Se houver mais de uma candidata ou se não estiver claro qual campo representa a característica descrita, mostre as opções e peça ao usuário para escolher.
5. Explique em linguagem simples o valor atual, a parte antiga escolhida, a menor mudança proposta e tudo o que será preservado.
6. Peça autorização antes de gravar.
7. Releia o contexto atual e grave com o hash mais recente:
   - campos simples: `update_global_settings_context`;
   - coleções: `update_global_collection` com uma operação por chamada.
8. Se a ferramenta devolver impacto e um `confirmation_token`, mostre esse impacto e peça uma nova autorização. Só então repita exatamente a mesma mudança com o token descartável. Não confirme automaticamente.
9. Consulte novamente o mesmo recurso e confirme o resultado.

Se o hash estiver desatualizado, releia a área e apresente novamente a proposta. Não repita a gravação automaticamente.

## Limites

- As sete coleções catalogadas usam o fluxo de coleções. Outras listas da área podem usar o fluxo contextual somente quando a consulta indicar `writable: true`. Não tente contornar uma proteção usando o JSON amplo.
- Formulários gerenciam campos, modelos e relações. Respostas e contatos enviados não pertencem a esta skill.
- O contexto da configuração da Agenda não inclui os agendamentos de visitantes.
- Conteúdo de páginas pertence a `$orbite-page-builder`.
- Links e estrutura dos menus pertencem a `$orbite-navigation-builder`.
- Arquivos de templates e modelos visuais pertencem exclusivamente ao fluxo interno de código da Orbite. Esta skill trabalha apenas com configurações permitidas pelo MCP.
- Criar, editar, versionar ou aplicar um componente interativo de mídia pertence a `$orbite-media-component-builder`. Esta skill altera somente configurações globais permitidas; a skill de componente apenas consulta e usa os valores visuais necessários.

