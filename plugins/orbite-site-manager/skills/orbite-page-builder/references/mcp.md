# Uso do MCP Orbite

Use a conexão MCP explicitamente escolhida para a tarefa.

- O destino é o Site identificado pela própria conexão em `/mcp`.
- Nunca aceite outra URL enviada no chat nem altere os dois ambientes na mesma tarefa.

## O que o MCP pode fazer

| Ação | Ferramenta | Entrada principal |
| --- | --- | --- |
| Ler uma página existente | `get_page` | `slug` |
| Atualizar uma página existente | `update_page` | `slug`, `content` |
| Listar páginas | `list_pages` | `search`, `status`, `page`, `per_page` |
| Criar página em rascunho | `create_page` | `title`, `slug` |
| Duplicar página em rascunho | `duplicate_page` | `source_id` ou `source_slug` |
| Listar áreas globais | `list_global_settings_areas` | nenhuma |
| Ler uma área global | `get_global_settings_context` | `area` |
| Atualizar uma área global | `update_global_settings_context` | `area`, `changes`, `expected_hash` |
| Ler menus | `get_menus` | nenhuma |
| Atualizar menus | `update_menus` | `menus` |
| Consultar a galeria | `list_images` | `search`, `page`, `per_page` |
| Consultar uma imagem | `get_image` | `id` ou `slug` |
| Enviar uma imagem | `upload_image` | `name`, `content_base64` |
| Listar modelos visuais | `list_layout_models` | nenhuma |
| Selecionar modelo visual | `select_layout_model` | `area`, `model` |

## O que o MCP não pode fazer

- Publicar ou apagar páginas.
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

Para alterar configurações globais, use `$orbite-global-settings`. Não envie o JSON amplo do Design System por esta skill.

## Falhas

- Se o MCP não estiver conectado, peça ao usuário para ativar a conexão correspondente ao Site e ao ambiente autorizados.
- Se a conexão não identificar claramente um Site válido, pare e peça ao usuário para conferir a configuração MCP.
- Se o pedido for criar uma página nova, use `create_page` e mantenha o rascunho. Se o usuário apontou uma página existente que não foi encontrada, pare e confirme o destino antes de criar outra.
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
