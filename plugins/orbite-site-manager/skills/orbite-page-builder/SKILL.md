---
name: orbite-page-builder
description: Crie, duplique e edite páginas do Orbite usando somente templates já cadastrados, gerencie o conteúdo pelo MCP correto e confira visualmente o resultado no ChatPanel.
---

# Orbite Page Builder

Construa o conteúdo da página o mais próximo possível da referência usando os templates e o Design System já disponíveis no Site.

## Regras obrigatórias

- A chamada desta skill não autoriza edições automaticamente. Primeiro faça um diagnóstico curto, apresente as soluções e peça autorização para implementar.
- Confirme a conexão MCP, o ambiente e o Site identificado por ela antes de trabalhar. Nunca use produção quando o ambiente estiver ambíguo.
- O destino é somente o Site identificado pela conexão MCP escolhida. Nunca aceite uma URL enviada no chat.
- Trabalhe somente com templates já cadastrados no Site. Esta skill não cria, edita nem orienta arquivos de template ou outros arquivos do tema.
- Se a referência exigir um template que não existe, explique o limite e encaminhe essa parte para a equipe da Orbite. Não tente contornar o limite gerando PHP, SCSS ou JavaScript do tema.
- Código criado pelo cliente fica limitado aos componentes interativos de mídia e pertence a `$orbite-media-component-builder`.
- Use componentes, escalas, tokens e padrões que já existem no Design System.
- Toda página criada ou duplicada deve começar como rascunho.
- Só publique uma página ou a devolva para rascunho quando o usuário pedir e autorizar essa mudança claramente.
- Nunca mova páginas para a lixeira nem as exclua definitivamente por este fluxo.
- Não exponha tokens, credenciais ou segredos em mensagens, arquivos ou logs.
- Use o histórico quando o usuário citar uma versão anterior, data, hora ou característica antiga, ou quando ele ajudar materialmente a cumprir o pedido. Não percorra revisões sem necessidade.
- Consultar uma revisão não autoriza restaurar nem editar a página.

## Antes de trabalhar

1. Confirme a imagem de referência, a página existente ou o título e slug da nova página, a conexão, o modo e o ambiente usados na visualização.
2. Leia [references/fluxo-de-pagina.md](references/fluxo-de-pagina.md).
3. Leia [references/mcp.md](references/mcp.md) antes de usar o MCP Orbite.
4. Leia [references/responsive-media.md](references/responsive-media.md) quando o pedido envolver mídia principal, lista, galeria ou background no desktop e no mobile.
5. Leia [references/validacao-visual.md](references/validacao-visual.md) antes de abrir o ChatPanel ou conferir wireframes no navegador.

## Fluxo resumido

1. Analise a referência e identifique quais partes podem ser atendidas pelos templates e componentes já disponíveis.
2. Explique, em linguagem simples, o que será ajustado e se alguma parte depende de um template novo da equipe Orbite.
3. Quando o pedido mencionar o passado, liste primeiro os resumos das revisões, abra somente as candidatas e compare-as com a página atual. Se houver mais de uma interpretação possível, peça ao usuário para escolher.
4. Mostre o que virá da versão antiga, o que será preservado da versão atual e peça autorização antes de editar conteúdo.
5. Se a página ainda não existir, crie-a como rascunho pelo MCP correto.
6. Leia o JSON atual pelo MCP antes de qualquer alteração.
7. Atualize somente os campos necessários da página ou dos menus. Para configurações globais, use `$orbite-global-settings`.
   Para criar ou editar código interativo dentro de um espaço de mídia, use `$orbite-media-component-builder`; esta skill continua responsável por localizar a página, a seção e o alvo exato.
   Para imagem, vídeo ou Lottie com variação por tela, use `update_responsive_media`; para componente interativo, entregue também `viewport_scope` à ferramenta atômica da skill de componentes.
8. Leia novamente pelo MCP para confirmar que a gravação funcionou.
9. Quando o usuário pedir publicação ou retorno para rascunho, releia a página e use `set_page_status` com o `content_hash` e o status atuais. Confirme o status final com outra leitura.
10. Abra a página no navegador, siga o fluxo de validação visual do ChatPanel quando precisar conferir wireframes, compare com a imagem e faça ajustes pequenos e verificáveis.
11. Termine com um resumo do que mudou, do que foi testado e das limitações restantes.

## Limite de responsabilidade

O MCP cria e duplica páginas somente como rascunho. Depois, pode publicar ou devolver uma página para rascunho com autorização explícita, além de cuidar do conteúdo, imagens, configurações globais, menus e seleção de modelos já cadastrados no Site identificado pela conexão.

Templates de página, cabeçalho e rodapé são responsabilidade exclusiva do fluxo interno da Orbite. O cliente pode gerar código somente dentro dos componentes interativos de mídia.

Componentes interativos em espaços de mídia pertencem a `$orbite-media-component-builder`. Alterar apenas proporção, `fit` ou o alvo da página continua neste fluxo e deve preservar o `src` e a versão do componente.
