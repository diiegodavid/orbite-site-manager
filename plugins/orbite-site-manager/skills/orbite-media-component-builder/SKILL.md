---
name: orbite-media-component-builder
description: Crie, edite, versione, publique, aplique e gerencie componentes interativos confiáveis nos espaços de mídia do Site conectado por um MCP da Orbite. Use para elementos gráficos em HTML, CSS direto e JavaScript; imagens raster e templates reutilizáveis pertencem a fluxos próprios.
---

# Orbite Media Component Builder

Crie componentes interativos responsivos em HTML, CSS direto e JavaScript. Componentes novos e aprovados usam Shadow DOM no front-end; o iframe permanece apenas para versões legadas ou código não confiável.

## Regras obrigatórias

- A chamada desta skill não autoriza alterações. Faça as leituras necessárias, mostre uma proposta curta e peça autorização imediatamente antes da primeira escrita.
- Confirme a conexão MCP, o ambiente e o modo. Se houver dúvida, não altere nada. Antes de uma escrita em produção, diga explicitamente que o destino é produção.
- Em modo `self`, use somente o próprio Site conectado. Em modo `customer`, use somente o Site salvo no painel da Orbite.
- Nunca aceite outra URL enviada no chat nem troque de conexão silenciosamente.
- Componentes novos usam `trusted-inline@1`, `html-css-js@1`, `orbite-media-v1`, `media-slot` e `media-inline-v1`. Preserve `vanilla-web@1` e `media-slot-v1` somente ao editar uma versão legada que já use esse runtime.
- Use CSS direto dentro de `<style>`. Nunca gere SCSS, Sass, Less nem dependa de compilação posterior.
- `trusted-inline@1` executa como código confiável do Site, sem iframe. Use-o somente para código criado ou aprovado por administrador/Codex; não trate Shadow DOM como barreira de segurança para JavaScript desconhecido.
- Trate cada criação como um elemento gráfico pequeno e responsivo. Nunca aceite rolagem interna; reduza ou reorganize o conteúdo para caber no espaço de mídia.
- O ID público e o `root_id` são criados pelo Site. Nunca invente esses identificadores.
- Listagens não devem ler fonte. Ao editar, leia somente a versão exata escolhida pelo usuário.
- Imagens anexadas servem como referência para o modelo. Não as inclua nas ferramentas MCP; envie ao Site apenas mídias aprovadas pela ferramenta própria de upload.
- Não exponha fonte, manifesto completo, tokens ou imagens em logs, histórico de aprovação ou mensagens de erro. Se o usuário incluir algo parecido com senha ou token no conteúdo, troque o valor por um exemplo seguro e informe a substituição sem repetir o original.
- Conflitos de hash, revisão ou estado exigem nova leitura e uma nova proposta. Não repita a escrita automaticamente.

## Fluxo

1. Leia [references/mcp.md](references/mcp.md).
2. Confirme a conexão com uma leitura simples e use `get_media_component_context` para receber tokens, tipografia, limites, `root_id`, envelope responsivo e contexto assinado.
3. Para editar, renomear ou aplicar, aceite o ID curto ou um nome aproximado. O ID tem prioridade; pelo nome, procure na biblioteca. Se houver um único resultado claro, use o nome canônico e o ID do Site. Se houver duplicidade ou nenhum resultado, peça o ID e oriente o usuário a conferi-lo em Componentes no painel. Leia somente a versão exata com fonte.
4. Para componente novo ou inline, leia [references/runtime-trusted-inline.md](references/runtime-trusted-inline.md). Leia [references/runtime-vanilla-web.md](references/runtime-vanilla-web.md) somente ao manter uma versão legada que já use `vanilla-web@1`.
5. Gere um nome curto e claro quando for um componente novo; não peça nome nem ID ao usuário. O Site gera o ID curto e o `root_id`. O nome pode mudar; o ID curto permanece estável.
6. Gere um único documento com raiz HTML, `<style>` contendo CSS direto e `<script>` contendo JavaScript. Use `validate_media_component`, corrija somente erros devolvidos pelo validador e faça no máximo duas novas tentativas.
7. Apresente nome, operação, runtime e destino. Peça autorização antes de criar, publicar, renomear, restaurar, arquivar, desarquivar, gerar preview ou aplicar. Imediatamente antes da escrita em componente existente, releia e fixe revisão, estado, ponteiros e o estado da versão exigido pela ação; nunca complete esses campos por suposição.
8. Sem alvo de página, grave e publique somente na biblioteca. Em uma edição, só aplique no alvo se ele já usar esse mesmo componente ou se o usuário pedir aplicação explicitamente. Com alvo exato e autorizado, prefira `create_media_component_and_apply`, `create_media_component_version_and_apply` ou `restore_media_component_version_and_apply`.
9. Depois da escrita, confira o componente e a versão; quando houver aplicação, confira também a página e o `src` do alvo.
10. Responda com nome, ID curto, versão, estado e local aplicado. Diga de forma simples se algo não pôde ser confirmado.

## Limites

- Imagem raster pertence ao fluxo de imagem.
- Criação ou edição estrutural de templates de página, cabeçalho ou rodapé pertence exclusivamente ao fluxo interno da Orbite. O cliente gera código somente dentro dos componentes interativos de mídia desta skill.
- Cores e tipografia globais pertencem a `$orbite-global-settings`; esta skill apenas consome os tokens devolvidos pelo contexto.
- Alterar proporção ou `fit` pertence ao espaço de mídia da página e não cria uma versão do componente.
