# Uso do MCP Orbite

Use a conexão MCP explicitamente escolhida para a tarefa.

- O destino é o Site identificado pela própria conexão em `/mcp`.
- Nunca aceite outra URL enviada no chat nem altere os dois ambientes na mesma tarefa.

## O que o MCP pode fazer

| Ação | Ferramenta | Entrada principal |
| --- | --- | --- |
| Ler uma página existente | `get_page` | `slug` |
| Consultar a estrutura completa de propriedades | `get_page_props_schema` | — |
| Obter uma seção pronta com o preset oficial | `get_template_preset` | `template_id` |
| Testar capacidades reais de um template | `get_template_capabilities` | `template_id`, `probes` |
| Atualizar uma página existente | `update_page` | `slug`, `content` |
| Trocar, remover ou restaurar mídia responsiva | `update_responsive_media` | `slug`, seção, papel, operação, alcance, estado atual e hash |
| Listar páginas | `list_pages` | `search`, `status`, `page`, `per_page` |
| Localizar revisões por data ou características | `list_page_revisions` | `id` ou `slug`, `page`, `per_page` |
| Ler uma revisão escolhida | `get_page_revision` | `id` ou `slug`, `revision_id` |
| Criar página em rascunho | `create_page` | `title`, `slug` |
| Duplicar página em rascunho | `duplicate_page` | `source_id` ou `source_slug` |
| Publicar ou voltar para rascunho | `set_page_status` | `id` ou `slug`, `status`, `expected_hash`, `expected_status` |
| Listar áreas globais | `list_global_settings_areas` | nenhuma |
| Ler uma área global | `get_global_settings_context` | `area` |
| Atualizar uma área global | `update_global_settings_context` | `area`, `changes`, `expected_hash` |
| Ler menus | `get_menus` | nenhuma |
| Atualizar menus | `update_menus` | `menus`, `expected_hash` |
| Consultar a galeria | `list_images` | `search`, `page`, `per_page` |
| Consultar uma imagem | `get_image` | `id` ou `slug` |
| Enviar uma imagem | `upload_image` | `name`, `content_base64` |
| Listar modelos visuais | `list_layout_models` | nenhuma |
| Selecionar modelo visual | `select_layout_model` | `area`, `model` |

## O que o MCP não pode fazer

- Mover páginas para a lixeira ou excluí-las definitivamente.
- Criar, editar ou apagar arquivos de template.
- Gerar imagens por conta própria. A geração acontece no Codex e o MCP faz o upload.
- Escolher ou trocar o Site de destino por uma URL enviada no chat.
- Alterar ao mesmo tempo os ambientes local e de produção.

Esta skill não executa deploy nem altera arquivos do tema.

## Sequência segura

1. Comece com uma leitura simples para confirmar a conexão, o modo, o ambiente e o Site conectado.
2. Antes de atualizar, leia o recurso completo que será alterado.
3. Preserve campos desconhecidos e estruturas que não fazem parte da tarefa.
4. Faça a menor alteração necessária.
5. Grave somente depois de receber autorização do usuário.
6. Leia novamente o mesmo recurso e compare o resultado.

Ao atualizar uma página, envie o `content_hash` da leitura anterior em `expected_hash`. O MCP não cria cópias da página; o histórico fica por conta das revisões nativas do WordPress.

Ao adicionar uma seção, use `get_template_preset` para começar com o mesmo JSON padrão criado pela API interna. Quando houver dúvida sobre o suporte de um template, consulte `get_page_props_schema`, escolha apenas propriedades relacionadas ao pedido e envie sondagens pequenas para `get_template_capabilities`. Cada sondagem parte do mesmo preset oficial. A resposta contém somente as variações que alteraram a renderização; use apenas essas propriedades na proposta de edição.

`media` representa a mídia principal. `medias` representa uma coleção e, nos templates de cards, cada posição acompanha o item de mesmo índice em `contents.list`. Em ambos, `src` pode receber imagem, vídeo, arquivo Lottie ou a rota canônica de um componente interativo. Use `cover-src` para a capa de um vídeo. As proporções aceitas são `1:1`, `3:4`, `4:5`, `9:16`, `9:21`, `4:3`, `5:4`, `16:9`, `21:9` e `32:9`; use `aspect-ratio-mobile` quando o mobile precisar de outra proporção. Em `object-fit`, use `cover` para preencher com corte ou `contain` para mostrar a mídia inteira. Não use a coleção legada `videos`.

No conteúdo, `caption` pode representar legenda, categoria ou nome da categoria da seção. Em botões, `icon-position` aceita `left` ou `right`, `target` aceita `_blank` ou vazio, e `tracking` recebe um identificador de rastreamento. As margens aceitam valores de `0` a `6`; use `0` ou `none` para removê-las.

Consulte sempre o `guide` devolvido por `get_page_props_schema` para os significados de efeito, transição, duração, inversão e fixação dos fundos, além das regras de artigos. Para artigos, `post-type` aceita `post` ou `material`, `specific` recebe IDs, `category` recebe o nome exato, `recents` seleciona os mais recentes e `numberposts` limita a quantidade.

Em `maps`, cada item usa `title`, `subtitle`, `lat` e `lng`. Em `icons`, cada item usa `name`.

Para mídia responsiva, siga [responsive-media.md](responsive-media.md). Prefira `update_responsive_media` a editar `src-mobile` manualmente. A ferramenta aceita mídia principal, item de lista, galeria e background, preserva a outra tela e confirma o valor salvo. Uma rota de componente interativo pertence às ferramentas atômicas de `$orbite-media-component-builder`.

## Memória da página

Quando o usuário citar uma data, uma hora ou uma característica antiga, use `list_page_revisions` para localizar candidatos pelos resumos. Consulte com `get_page_revision` somente as revisões que realmente podem atender ao pedido.

Compare a candidata com `get_page` e monte uma proposta seletiva: reutilize apenas a parte antiga pedida e preserve todo o restante da página atual. Se duas revisões parecerem válidas ou a correspondência não estiver clara, mostre as opções e peça ao usuário para escolher.

Uma leitura histórica é somente consulta. Antes de salvar, explique qual revisão serviu de referência, o que será recuperado e o que continuará atual, peça autorização e releia a página para obter o `content_hash` mais recente. Depois use o fluxo normal de `update_page` e confirme com outra leitura. Não existe restauração automática de página por estas ferramentas.

Para publicar ou devolver uma página para rascunho, releia a página imediatamente antes da ação e use `set_page_status` com o `content_hash` em `expected_hash` e o status atual em `expected_status`. Use somente `publish` ou `draft`, após pedido e autorização explícitos do usuário, e releia a página para confirmar o status final.

Para alterar configurações globais, use `$orbite-global-settings`. Não envie o JSON amplo do Design System por esta skill.

## Falhas

- Se o MCP não estiver conectado, peça ao usuário para ativar a conexão correspondente ao Site e ao ambiente autorizados.
- Se a conexão não identificar claramente um Site válido, pare e peça ao usuário para conferir a configuração MCP.
- Se o pedido for criar uma página nova, use `create_page` e mantenha o rascunho. Se o usuário apontou uma página existente que não foi encontrada, pare e confirme o destino antes de criar outra.
- Se a página ou o status tiver mudado depois da última leitura, releia e apresente novamente a proposta. Não publique nem volte para rascunho automaticamente.
- Se a leitura após a gravação não confirmar a mudança, trate a operação como não concluída e explique o erro de forma simples.

## Conteúdo

- Use o formato retornado pela leitura como modelo para a atualização.
- Não invente chaves quando o esquema puder ser descoberto pela leitura.
- Não substitua todo o JSON se apenas uma parte precisa mudar.
- Nunca copie ou registre o token do MCP.

## Imagens geradas

1. Gere a imagem no Codex usando a ferramenta própria de geração.
2. Escolha um nome claro, em formato de slug.
3. Envie o arquivo com `upload_image`; não é necessária uma URL pública.
4. Use o ID ou a URL confirmados pelo retorno do MCP no JSON da página.
5. Confira a imagem no navegador.

Não exclua imagens da galeria por este fluxo. O upload aceita PNG, JPEG e WebP com até 10 MB.
