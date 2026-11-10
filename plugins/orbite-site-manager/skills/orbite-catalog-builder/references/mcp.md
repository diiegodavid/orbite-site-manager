# MCP do catálogo

## Consulta

| Ação | Ferramenta |
| --- | --- |
| Ler nomes, moeda e limites | `get_catalog_context` |
| Listar itens | `list_catalog_items` |
| Consultar item completo | `get_catalog_item` |
| Listar categorias | `list_catalog_categories` |
| Consultar categoria | `get_catalog_category` |

## Alterações

| Ação | Ferramenta |
| --- | --- |
| Criar rascunho | `create_catalog_item` |
| Atualizar conteúdo, preço, disponibilidade ou imagens | `update_catalog_item` |
| Publicar ou voltar para rascunho | `set_catalog_item_status` |
| Mover para lixeira | `trash_catalog_item` |
| Restaurar da lixeira | `restore_catalog_item` |
| Criar categoria | `create_catalog_category` |
| Atualizar categoria | `update_catalog_category` |
| Excluir categoria | `delete_catalog_category` |

## Campos do item

- `title`, `slug`, `excerpt`, `content` e `meta_description`: textos do item.
- `category_ids`: categorias atuais. Ao atualizar, a lista enviada substitui a lista anterior.
- `featured_image_id`: capa; use zero para removê-la.
- `catalog_data.available`: disponibilidade comercial, independente de publicação.
- `catalog_data.price`: preço normal.
- `catalog_data.discount_price`: preço promocional, sempre menor que o preço normal.
- `catalog_data.gallery_media_ids`: IDs ordenados da galeria, com até 20 imagens.
- `status`: `draft` ou `publish`, alterado pela ferramenta própria.

Antes de alterar, consulte o item e envie seu `state_hash` como `expected_hash`. Depois releia o item para confirmar todos os campos.
