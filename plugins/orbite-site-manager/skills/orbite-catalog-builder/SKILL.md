---
name: orbite-catalog-builder
description: Consulte e gerencie itens, categorias, preços, disponibilidade e imagens do catálogo no Site conectado pelo MCP da Orbite. Use quando o negócio chamar os itens de produtos, serviços, quartos, pratos ou outro nome configurado.
---

# Orbite Catalog Builder

Gerencie o catálogo de exibição do Site conectado, respeitando os nomes usados pelo próprio negócio.

## Regras

- A skill não autoriza gravações. Leia, proponha a menor mudança e peça autorização antes de alterar.
- Comece com `get_catalog_context`. Se o catálogo estiver desativado, não crie itens.
- Trate o catálogo como genérico. Use os nomes devolvidos pelo contexto, sem presumir que todo item seja um produto.
- Leia [references/mcp.md](references/mcp.md) antes de usar as ferramentas.
- Todo item novo nasce como rascunho.
- Diferencie publicação de disponibilidade: um item publicado pode estar indisponível.
- Antes de atualizar ou mudar a publicação, leia o item e use o `state_hash` mais recente.
- Preserve campos que não fazem parte do pedido e confira o resultado com outra leitura.
- Mova itens ou exclua categorias somente após pedido e autorização explícitos.
- Para ordenar categorias, percorra todas as páginas de `list_catalog_categories`, apresente a sequência completa proposta e peça autorização antes de gravar.
- Ao reordenar, envie todos os IDs uma única vez e use o `order_hash` da leitura mais recente como `expected_hash`. Depois, releia para conferir.
- Para ordenar itens dentro de uma categoria, percorra todas as páginas de `list_catalog_category_items`, mostre a sequência completa e peça autorização. Depois, envie todos os IDs uma única vez com o `order_hash` mais recente e releia a categoria.
- Nunca exclua definitivamente um item da lixeira.
- Em `content`, use somente HTML semântico simples. Nunca envie `class`, `style`, `id` ou atributos de aparência; o template controla o visual.

## Imagens

- `featured_image_id` é a imagem de capa.
- `gallery_media_ids` é a galeria ordenada e aceita até 20 imagens.
- Consulte ou envie imagens pelas ferramentas da galeria antes de associar seus IDs ao item.
- A geração de imagens acontece no Codex; o MCP apenas consulta, envia e associa os arquivos.

