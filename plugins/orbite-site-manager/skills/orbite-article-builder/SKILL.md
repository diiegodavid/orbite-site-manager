---
name: orbite-article-builder
description: Planeje, escreva e gerencie artigos, categorias, SEO e imagens destacadas no Site conectado por um MCP da Orbite, em modo próprio ou Customer.
---

# Orbite Article Builder

Crie artigos úteis, organizados e prontos para revisão no blog do Site conectado. Siga a marca do Site; não presuma que o conteúdo seja para a própria Orbite.

## Regras obrigatórias

- A chamada desta skill não autoriza alterações automaticamente. Faça um diagnóstico curto, apresente a solução e peça autorização antes de gravar.
- Confirme a conexão MCP, o ambiente e o modo antes de trabalhar. Se houver dúvida, não altere nada.
- Em modo `self`, trabalhe somente no próprio Site conectado. Em modo `customer`, trabalhe somente no Site salvo no painel da Orbite.
- Nunca aceite uma URL de destino enviada no chat nem troque de conexão silenciosamente.
- Leia artigos e categorias existentes antes de criar conteúdo novo.
- Todo artigo novo deve nascer como rascunho.
- Todo artigo deve conter ao menos uma tabela útil no shortcode `orbite_table`.
- Trate FAQ como conteúdo separado. Nunca inclua a FAQ no HTML do artigo.
- Só publique, devolva para rascunho ou mova um artigo para a lixeira quando o usuário pedir isso claramente.
- Nunca exclua artigos definitivamente nem exclua categorias.
- Antes de atualizar, leia o artigo e envie o `content_hash` recebido em `expected_hash`.
- Preserve campos que não fazem parte do pedido e confirme a gravação com uma nova leitura.
- Não exponha tokens, credenciais ou segredos.

## Antes de trabalhar

1. Confirme tema, objetivo, público, idioma, tom da marca, ambiente, modo e Site conectado.
2. Leia [references/mcp.md](references/mcp.md) antes de usar o MCP.
3. Leia [references/conteudo.md](references/conteudo.md) antes de escrever ou reescrever um artigo.
4. Consulte artigos semelhantes e as categorias existentes.
5. Explique resumidamente o artigo proposto e peça autorização para criar ou alterar.

## Fluxo

1. Use `list_articles` para evitar repetição e entender o acervo atual.
2. Use `list_categories` e reaproveite uma categoria adequada quando ela já existir.
3. Crie uma categoria somente quando realmente faltar uma opção correta.
4. Escreva título, slug, meta description, resumo e conteúdo HTML simples. Inclua a tabela no formato definido na referência.
5. Use `create_article`; o resultado deve permanecer em rascunho.
6. Se houver imagem pronta, consulte ou envie pela galeria e defina seu ID como imagem destacada.
7. Leia novamente com `get_article` e confira os campos salvos.
8. Quando necessário, abra a URL de visualização no navegador do Codex e confira a aparência.
9. Publique somente após autorização explícita usando `set_article_status`.

## Imagens

- A geração acontece no Codex, não no MCP.
- Quando o Site de destino for a própria Orbite e houver um fluxo interno de imagens instalado, siga esse fluxo. Nos demais Sites, use a ferramenta de geração de imagem disponível no Codex.
- O MCP lista, envia e atualiza os dados da imagem, além de associá-la ao artigo.
- Sempre escreva um texto alternativo objetivo e coerente com a imagem.

## FAQ

Se o usuário pedir uma FAQ, gere-a como uma entrega separada do artigo. O MCP atual não grava o campo próprio de FAQ; informe isso claramente e não finja que ela foi salva.

## Recuperação

Use as revisões do WordPress para recuperar conteúdo. Liste primeiro com `list_article_revisions` e restaure somente a revisão identificada pelo usuário.

