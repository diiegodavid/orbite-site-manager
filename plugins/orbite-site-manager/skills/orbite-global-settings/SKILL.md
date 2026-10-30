---
name: orbite-global-settings
description: Consulte e altere com segurança cores, tipografia, componentes e outras configurações globais do Site conectado por um MCP da Orbite.
---

# Orbite Global Settings

Gerencie o Design System do Site sem inventar chaves e sem alterar configurações fora do pedido.

## Regras obrigatórias

- A chamada desta skill não autoriza alterações. Primeiro consulte, explique a proposta e peça autorização.
- Confirme a conexão MCP, o ambiente e o modo antes de trabalhar. Se houver dúvida, não altere nada.
- Em modo `self`, use somente o próprio Site conectado. Em modo `customer`, use somente o Site salvo no painel da Orbite.
- Nunca aceite outra URL enviada no chat nem troque de conexão silenciosamente.
- Use as ferramentas contextuais. Não use `update_global_settings` em um fluxo novo.
- Nunca altere tokens, credenciais, campos internos ou campos marcados como somente leitura.
- Atualize apenas uma área por vez e somente os campos necessários.
- Uma restauração afeta o Design System inteiro e exige autorização explícita.

## Fluxo

1. Leia [references/mcp.md](references/mcp.md).
2. Use `list_global_settings_areas` para descobrir a área correta.
3. Use `get_global_settings_context` para conhecer valores atuais, explicações, tipos e opções permitidas.
4. Explique em linguagem simples o valor atual, a mudança proposta e onde ela aparecerá no Site.
5. Peça autorização antes de gravar.
6. Use `update_global_settings_context` com uma alteração pequena e o `settings_hash` da leitura mais recente.
7. Consulte novamente a mesma área e confirme o resultado.

Se o hash estiver desatualizado, releia a área e apresente novamente a proposta. Não repita a gravação automaticamente.

## Limites

- Campos complexos marcados como somente leitura precisam de um fluxo especializado; não tente contornar essa proteção usando o JSON amplo.
- Conteúdo de páginas pertence a `$orbite-page-builder`.
- Links e estrutura dos menus pertencem a `$orbite-navigation-builder`.
- Arquivos de templates e modelos visuais pertencem exclusivamente ao fluxo interno de código da Orbite. Esta skill trabalha apenas com configurações permitidas pelo MCP.
- Criar, editar, versionar ou aplicar um componente interativo de mídia pertence a `$orbite-media-component-builder`. Esta skill altera os tokens globais; a skill de componente apenas os consulta e usa.

