# MCP do catálogo

## Consulta

| Ação | Ferramenta |
| --- | --- |
| Ler nomes, moeda e limites | `get_catalog_context` |
| Listar itens | `list_catalog_items` |
| Consultar item completo | `get_catalog_item` |
| Listar categorias | `list_catalog_categories` |
| Consultar categoria | `get_catalog_category` |
| Listar itens de uma categoria na ordem atual | `list_catalog_category_items` |

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
| Reordenar todas as categorias | `reorder_catalog_categories` |
| Reordenar todos os itens de uma categoria | `reorder_catalog_category_items` |
| Excluir categoria | `delete_catalog_category` |

## Campos do item

- `title`, `slug`, `excerpt`, `content` e `meta_description`: textos do item. Em `content`, use HTML semântico simples, sem `class`, `style` ou `id`.
- `category_ids`: categorias atuais. Ao atualizar, a lista enviada substitui a lista anterior.
- `featured_image_id`: capa; use zero para removê-la.
- `catalog_data.available`: disponibilidade comercial, independente de publicação.
- `catalog_data.price`: preço normal.
- `catalog_data.discount_price`: preço promocional, sempre menor que o preço normal.
- `catalog_data.gallery_media_ids`: IDs ordenados da galeria, com até 20 imagens.
- `status`: `draft` ou `publish`, alterado pela ferramenta própria.

Antes de alterar, consulte o item e envie seu `state_hash` como `expected_hash`. Depois releia o item para confirmar todos os campos.

## Categorias

- `sort_order` mostra a posição atual da categoria.
- `list_catalog_categories` devolve as categorias na ordem de exibição e inclui `order_hash`. Percorra todas as páginas até reunir a quantidade indicada em `pagination.total`.
- Para reordenar, monte a sequência completa, sem repetir nem omitir IDs. Mostre a proposta ao usuário e peça autorização.
- Depois da autorização, chame `reorder_catalog_categories` com `category_ids` e o `order_hash` mais recente em `expected_hash`.
- Depois da alteração, use `list_catalog_categories` novamente e confirme a sequência retornada.
- Categorias novas entram no fim. A seção automática `Outros` não é uma categoria cadastrada e fica fora da lista enviada.
- `list_catalog_category_items` devolve `sort_order`, paginação e um `order_hash` próprio daquela categoria.
- Para reordenar os itens, reúna todas as páginas e envie cada ID atual uma única vez para `reorder_catalog_category_items`. Uma categoria pode ter uma sequência diferente da outra.
- Depois da gravação, releia a categoria e confirme a sequência completa retornada.
