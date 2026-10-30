# MCP para artigos

Use a conexão MCP explicitamente escolhida para a tarefa.

- O destino é o Site identificado pela própria conexão em `/mcp`.
- Nunca aceite uma URL de destino enviada no chat nem altere os dois ambientes na mesma tarefa.

## Artigos

| Ação | Ferramenta |
| --- | --- |
| Listar e pesquisar | `list_articles` |
| Ler conteúdo completo | `get_article` |
| Criar rascunho | `create_article` |
| Atualizar campos | `update_article` |
| Duplicar como rascunho | `duplicate_article` |
| Publicar ou voltar para rascunho | `set_article_status` |
| Listar revisões | `list_article_revisions` |
| Restaurar revisão | `restore_article_revision` |
| Mover para lixeira | `trash_article` |
| Restaurar da lixeira | `restore_article` |

## Categorias

| Ação | Ferramenta |
| --- | --- |
| Listar | `list_categories` |
| Consultar | `get_category` |
| Criar | `create_category` |
| Atualizar | `update_category` |

Não há ferramenta para excluir uma categoria.

## Imagens

| Ação | Ferramenta |
| --- | --- |
| Listar galeria | `list_images` |
| Consultar imagem | `get_image` |
| Enviar imagem | `upload_image` |
| Atualizar título, alt e legenda | `update_image_metadata` |

O ID devolvido pela galeria pode ser usado como `featured_image_id` ao criar ou atualizar o artigo.

## Sequência segura

1. Leia o artigo com `get_article`.
2. Modifique apenas os campos necessários.
3. Envie o `content_hash` em `expected_hash` ao usar `update_article`.
4. Leia novamente para confirmar.
5. Use publicação e lixeira somente após pedido explícito.

Artigos criados ou duplicados sempre começam como rascunho. O MCP não exclui um artigo definitivamente.

