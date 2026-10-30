# Runtime `trusted-inline@1`

Use este runtime para componentes novos criados ou aprovados por administrador/Codex. Ele monta HTML, CSS e JavaScript diretamente no front-end, dentro de Shadow DOM e sem iframe.

## Confiança

- Shadow DOM evita que o CSS do componente vaze para o restante da página, mas não limita o poder do JavaScript.
- O JavaScript inline roda no mesmo documento do Site. Considere a fonte confiável e revise-a antes de publicar.
- Não aceite PHP, templates de servidor, segredos, tokens ou código desconhecido de terceiros.
- Dependências executáveis externas só podem entrar por um catálogo aprovado pelo Site. Imagens podem usar `<img src="https://...">`; scripts, folhas de estilo, imports e outros recursos remotos continuam proibidos.

## Documento

Gere um único documento com exatamente estes três blocos, nesta ordem:

1. Uma raiz HTML cujo `id` seja o `root_id` recebido do contexto.
2. Um `<style>` com CSS direto.
3. Um `<script>` com JavaScript de front-end.

Nunca gere SCSS, Sass, Less ou outra fonte que precise de compilação. O valor validado e versionado já deve estar pronto para o navegador.

A raiz deve ocupar o espaço disponível, aceitar largura e altura temporariamente iguais a zero e evitar tamanho fixo imposto pelo próprio componente. O host continua responsável pela proporção e pelo `fit` do espaço de mídia.

## HTML e CSS

- Campos de texto, botões, formulários locais, imagens HTTPS, SVG, Canvas e demais elementos de interface são permitidos.
- Use elementos semânticos, rótulos acessíveis, foco visível e suporte a teclado.
- O CSS vive no Shadow DOM, portanto pode usar seletores internos, `:host`, media queries, container queries, animações e transições.
- Use os tokens `--color-brand-1` até `--color-brand-20` e as famílias tipográficas entregues pelo contexto quando o pedido não definir outra direção visual.
- Cores personalizadas continuam exigindo `color_policy: explicit-custom`.
- Para mídia de primeira dobra, evite rolagem interna. Reorganize ou reduza o conteúdo quando o espaço ficar pequeno.
- Animações devem respeitar `prefers-reduced-motion`.

## JavaScript

O script pode usar JavaScript normal de front-end: funções, arrays, objetos, Promises, eventos, timers, Canvas e APIs do navegador adequadas à interface. Entregue JavaScript pronto para execução; não use `import`, TypeScript ou uma etapa de bundling.

O runtime entrega acesso à raiz local do componente e à API `OrbiteMedia`. Prefira consultar e alterar elementos a partir da raiz, mesmo sendo código confiável. Registre a limpeza de listeners, timers e outros recursos em `onDestroy`.

Responda ao ciclo quando a interação depender de visibilidade ou reprodução:

- `onPrepare`
- `onResize`
- `onActivate`
- `onPlay`
- `onPause`
- `onDeactivate`
- `onReset`
- `onDestroy`

Não presuma autoplay. Pause trabalho contínuo quando o componente for desativado e restaure o estado inicial em `onReset`.

## Ações do Site

Não acople o componente à estrutura interna do Site ou a seletores da página. Quando o contexto do Site informar uma ação permitida, solicite-a pela API `OrbiteMedia.requestHostAction()` e inclua `host-action` em `requested_capabilities`.

Use somente o nome de ação informado pelo contexto e aprovado para o Site conectado. URLs, nomes de função, seletores ou comandos arbitrários não são ações válidas.

## Responsividade

A mesma versão deve reorganizar seu conteúdo nos envelopes `1:1`, `3:4`, `4:5`, `9:16`, `9:21`, `4:3`, `5:4`, `16:9` e `21:9`. Use flex, grid, porcentagens, `clamp()` e media/container queries. Preserve primeiro a ação principal e reduza detalhes decorativos em espaços estreitos.
