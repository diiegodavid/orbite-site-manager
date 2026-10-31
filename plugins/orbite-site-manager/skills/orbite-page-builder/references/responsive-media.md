# Contrato único de mídia responsiva

Use estas regras para mídia principal, item de lista, galeria e background.

## Uma mídia, duas telas

- `src` é a mídia padrão do desktop.
- `src-mobile` é uma substituição opcional usada somente no mobile.
- Sem `src-mobile`, o mobile herda `src`.
- Desktop e mobile podem usar arquivos diferentes do mesmo formato ou formatos diferentes.
- Imagem, vídeo, Lottie e componente interativo são identificados depois que a variante da tela é escolhida.

Nunca monte as duas variantes ao mesmo tempo. O controlador responsivo do Site mantém a variante escondida inerte, monta somente a variante ativa e encerra vídeo, iframe, Lottie ou componente quando ela sai da tela.

## Intenções aceitas

| Pedido | Operação | Alcance |
| --- | --- | --- |
| Trocar somente no desktop | `replace` | `desktop` |
| Trocar somente no mobile | `replace` | `mobile` |
| Usar a nova mídia nas duas telas | `replace` | `both` |
| Remover de uma tela | `remove` | `desktop` ou `mobile` |
| Remover das duas telas | `remove` | `both` |
| Voltar a usar a mídia do desktop no mobile | `inherit` | `mobile` |

`both` atualiza `src` e remove todas as propriedades terminadas em `-mobile`. `inherit` apenas remove as propriedades `-mobile` e preserva o desktop.

## Fluxo pelo MCP

1. Leia a página com `get_page`.
2. Localize seção, template e papel da mídia: `main`, `item`, `gallery` ou `background`.
3. Guarde `content_hash` e a fonte atual do alcance escolhido. Um mobile ainda sem substituição usa string vazia como estado atual. Em um background legado de vídeo, a fonte atual do desktop pode estar em `video-src`.
4. Mostre a alteração e peça autorização.
5. Para imagem, vídeo ou Lottie, use `update_responsive_media`.
6. Para aplicar um componente interativo, use a operação atômica da skill de componentes e informe `viewport_scope`: `desktop`, `mobile` ou `both`.
7. Confira o retorno da escrita e leia a página novamente.

Não aplique uma rota de componente interativo por `update_responsive_media`. As ferramentas próprias validam versão publicada, runtime, host e ciclo de vida antes de aplicar.

## Componentes interativos

Todo componente novo usa o ciclo central `prepare`, `resize`, `activate`, `play`, `pause`, `deactivate`, `reset` e `destroy`. O componente não cria observador próprio para decidir quando tocar e não presume autoplay. O Site decide quando ativar, pausar, retomar e destruir.

