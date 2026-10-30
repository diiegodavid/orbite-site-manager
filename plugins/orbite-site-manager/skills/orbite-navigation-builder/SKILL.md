---
name: orbite-navigation-builder
description: Gerencie menus e selecione modelos visuais de cabeçalho e rodapé já cadastrados no Site conectado pelo MCP, sem criar ou editar arquivos do tema.
---

# Orbite Navigation Builder

Trate o conteúdo dos menus e sua aparência como duas partes diferentes.

- Links, colunas e submenus são alterados pelo MCP.
- O MCP lista e seleciona somente modelos visuais já cadastrados.
- A criação de modelos de cabeçalho e rodapé pertence exclusivamente ao fluxo interno da Orbite.

## Regras obrigatórias

- A chamada desta skill não autoriza edições. Primeiro faça um diagnóstico curto, liste as soluções e peça autorização.
- Confirme a conexão MCP, o ambiente e o modo antes de trabalhar. Nunca use produção quando o ambiente estiver ambíguo.
- Em modo `self`, o destino é o próprio Site conectado. Em modo `customer`, o destino é somente o Site salvo no painel da Orbite. Nunca aceite uma URL enviada no chat.
- Não crie nem edite arquivos do tema. Se o pedido exigir um modelo visual novo, explique que essa parte precisa ser feita pela equipe da Orbite.
- O cliente pode gerar código somente em componentes interativos de mídia, nunca em cabeçalho, rodapé ou templates de página.
- Leia os menus e o modelo selecionado antes de alterar qualquer coisa.
- Confirme toda gravação com uma nova leitura.

## Referências

- Para editar menus ou selecionar modelos, leia [references/mcp.md](references/mcp.md).

## Fluxo

1. Descubra se o pedido trata de conteúdo do menu, seleção de um modelo existente ou de um modelo visual novo.
2. Leia os menus e liste os modelos já cadastrados.
3. Explique o que pode ser feito pelo MCP e, se um modelo novo for necessário, encaminhe essa parte para a equipe da Orbite.
4. Peça autorização antes de gravar.
5. Atualize links e estruturas pelo MCP somente quando necessário.
6. Selecione somente um modelo já cadastrado pelo MCP.
7. Quando a mudança envolver configurações globais do cabeçalho ou rodapé, use `$orbite-global-settings`.
8. Abra o Site e confira cabeçalho, submenus, rodapé, desktop e mobile.

Não crie novas localizações de menu sem um pedido específico. As localizações padrão são cabeçalho, cabeçalho do blog e rodapé.
