---
name: orbite-media-component-builder
description: Crie, edite, aplique e exclua componentes interativos simples nos espaços de mídia do Site conectado por um MCP da Orbite.
---

# Orbite Media Component Builder

Componentes interativos possuem somente um nome e um HTML. O ID numérico é criado pelo banco e usado internamente na rota da mídia.

## Regras obrigatórias

- Confirme a conexão MCP, o ambiente e o Site identificado por ela. Antes de escrever em produção, informe explicitamente que o destino é produção.
- Use somente o Site identificado pela conexão MCP escolhida.
- Nunca aceite outra URL enviada no chat nem troque de conexão silenciosamente.
- Trabalhe com o componente pelo nome. O ID do banco não faz parte da conversa nem precisa ser informado pelo usuário.
- O HTML é um único documento com uma raiz, um `<style>` e um `<script>`, nesta ordem.
- A raiz deve possuir um ID simples. O script deve declarar `const root = document.getElementById('ID-DA-RAIZ');` usando exatamente esse ID.
- O componente deve ser completo por conta própria. Nunca presuma que `root` será fornecido pelo Site.
- Use CSS direto. Nunca gere SCSS, Sass, Less, TypeScript, imports ou uma etapa de compilação.
- Não use `setInterval`, `clearInterval`, workers ou APIs bloqueadas pelo executor. Para repetição, use `setTimeout` recursivo ou `requestAnimationFrame` e encerre o trabalho nos eventos de pausa e destruição.
- Não aceite PHP, segredos, tokens nem código desconhecido de terceiros.
- Trate cada criação como um elemento gráfico pequeno e responsivo, sem rolagem interna.
- O Site valida o HTML antes de salvar. Uma edição atualiza o próprio registro e não cria versão.
- Não existem rascunho, publicação, arquivamento, versão, manifesto ou restauração.
- A rota usada no `src` da mídia é criada pelo Site no formato `/media-component/{id}/`.

## Fluxo

1. Leia [references/mcp.md](references/mcp.md).
2. Para localizar um componente, liste os nomes. Quando houver um único nome claro, use-o; se houver dúvida, peça o nome exato.
3. Para editar, leia o componente exato e altere seu HTML no próprio registro.
4. Para criar, gere um nome curto e claro quando o usuário não tiver informado um.
5. Gere uma raiz HTML com ID, um `<style>` com CSS direto e um `<script>` que localize essa raiz pelo próprio ID.
6. Use `create_media_component` para criar e `update_media_component` para editar.
7. Quando houver uma mídia exata da página, prefira a operação combinada de criar ou editar e aplicar.
8. Para aplicar um componente existente, use `apply_media_component_to_page` com o nome e o alvo responsivo.
9. Depois da escrita, releia o componente. Quando houver aplicação, confira também a página e o `src`.
10. Responda com o nome e, quando houver aplicação, o local e o alcance responsivo.

## Limites

- Imagem raster pertence ao fluxo de imagem.
- Templates de página, cabeçalho e rodapé pertencem aos fluxos próprios.
- Cores e tipografia globais pertencem a `$orbite-global-settings`; o componente apenas usa os tokens disponíveis.
- Alterar proporção ou `fit` pertence ao espaço de mídia da página.
