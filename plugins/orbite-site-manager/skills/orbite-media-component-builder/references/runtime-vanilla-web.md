# Runtime `vanilla-web@1`

Este é o runtime legado em iframe. Use-o somente ao editar uma versão que já dependa de `vanilla-web@1` ou quando código não confiável precisar continuar isolado. Para componentes novos criados ou aprovados por administrador/Codex, use [runtime-trusted-inline.md](runtime-trusted-inline.md).

## Documento

- Gere um único documento com HTML, CSS e JavaScript puros.
- O primeiro elemento de conteúdo é uma `div` com o `root_id` recebido do contexto.
- A raiz usa largura e altura de `100%`, `min-width: 0`, `min-height: 0`, `box-sizing: border-box` e `overflow: hidden`; não define proporção ou tamanho fixo.
- Todo seletor CSS começa em `#<root_id>`. Não use seletores globais, `:root`, `html`, `body` ou regras que escapem da raiz.
- Todo JavaScript consulta, altera e escuta somente a raiz e seus descendentes. Não acesse `parent`, `top`, `opener`, a página hospedeira ou elementos fora da raiz.

## Design System

- Use as variáveis de `--color-brand-1` até `--color-brand-20` por padrão.
- Use a variável de família tipográfica devolvida pelo contexto.
- Use `var(--body-font-family)` em textos, `var(--header-font-family)` em títulos e `var(--button-font-family)` em botões.
- Para texto comum, adote por padrão títulos de até `14px` e textos de até `12px`. Ultrapasse esses tamanhos somente quando o pedido indicar claramente um título grande, hero ou outra hierarquia intencional.
- Não copie os valores hexadecimais para o documento quando uma variável estiver disponível.
- Cores personalizadas só são permitidas quando o usuário pedir de forma explícita; nesse caso use `color_policy: explicit-custom`.

## Segurança

- Não use rede, CDN, `fetch`, WebSocket, import dinâmico, cookies, storage, service worker, formulários externos, navegação ou downloads.
- Não use `eval`, `Function`, handlers HTML inline nem APIs para abrir janelas.
- Controles locais aceitam `button`, `label` e somente `input type="checkbox"`; formulários, campos de texto, arquivos e envio continuam bloqueados.
- Não carregue bibliotecas por conta própria. Runtimes futuros resolverão bibliotecas aprovadas por catálogo e hash.
- `vanilla-web@1` não libera WebGL, `OffscreenCanvas`, áudio programático, WebAssembly, workers, microtasks manuais, observadores de DOM nem navegação. Three.js, Motion e outras bibliotecas exigirão outro runtime aprovado no futuro.
- Canvas é somente 2D e deve chamar `getContext('2d')` diretamente. Em `brand-only`, `fillStyle`, `strokeStyle`, `shadowColor` e `addColorStop` leem o token diretamente com `getComputedStyle(root).getPropertyValue('--color-brand-N')`; não use cor literal nem alias intermediário.
- Mudanças visuais usam classes que já tenham CSS validado. Não altere `style`, `cssText`, atributos de apresentação, folhas de estilo ou Web Animations pelo JavaScript.
- Não crie loops `for`, `while` ou `do`, funções recursivas, callbacks nomeados ou filas com `Promise`/`queueMicrotask`. Timers e frames pontuais passam pelo orçamento e pelo ciclo de vida do runtime.
- Não use arrays, colchetes, destructuring ou índices dinâmicos. Para poucos cards ou passos, declare cada elemento separadamente com `root.querySelector` e alterne os estados com `if/else` e classes.
- Eventos de mouse e teclado devem permanecer na raiz. Suporte teclado somente quando houver controle interativo e mantenha foco e rótulos acessíveis.
- Registre um listener delegado na raiz. Mude aparência por classes CSS já validadas e use propriedades booleanas `checked` e `disabled` para os controles.
- Toda `animation` ou `transition` precisa de uma regra `@media (prefers-reduced-motion: reduce)` que a desative.

## Responsividade

O host controla largura, altura, proporção e `fit`. O componente apenas responde ao viewport interno e às mensagens do protocolo.

A mesma versão deve funcionar em `1:1`, `3:4`, `4:5`, `9:16`, `9:21`, `4:3`, `5:4`, `16:9` e `21:9`, em tamanhos pequenos e grandes. Também deve aceitar temporariamente `0×0`: nesse estado, pause trabalho pesado e evite divisão por zero, `NaN` ou `Infinity`.

Trate o componente como um elemento gráfico pequeno, e não como uma página, formulário ou dashboard. Nunca crie rolagem interna: são proibidos `overflow: auto`, `overflow: scroll`, `overflow: overlay`, propriedades `scroll-*` e `overscroll-*`. Se algo não couber, reduza texto, ornamentos e espaçamentos ou reorganize o conteúdo. Em sequências de cards ou passos, prefira mostrar um estado de cada vez. Use grid/flex, porcentagens e `clamp()` e preserve sempre o conteúdo importante sem depender de barras de rolagem.

Não grave no código a proporção do alvo usado como preview. Uma troca de proporção reorganiza a mesma instância sem criar versão ou trocar o `src`.

## Ciclo de mídia

Implemente o ciclo completo negociado pelo host: `prepare`, `resize`, `activate`, `play`, `pause`, `deactivate`, `reset` e `destroy`. Animações não devem presumir autoplay; responda aos comandos do host, pare trabalho pesado em `deactivate`, restaure o estado inicial em `reset` e cancele frames no `destroy`.

Para o listener obrigatório, use exatamente `const interactionController = new AbortController();`, passe `{signal: interactionController.signal}` para `root.addEventListener(...)` e chame `interactionController.abort()` dentro de `onDestroy`. Mantenha o callback inline; não declare um handler de função separado.
