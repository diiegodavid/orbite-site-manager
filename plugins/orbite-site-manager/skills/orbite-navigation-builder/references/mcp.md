# MCP de navegação

Use a conexão MCP explicitamente escolhida para a tarefa. O destino é o Site identificado pela própria conexão em `/mcp`. Não aceite outra URL enviada no chat.

## Conteúdo dos menus

- `get_menus`: lê cabeçalho, cabeçalho do blog e rodapé.
- `list_menu_history`: lista pontos antigos com resumos de raízes, colunas, tamanhos e itens.
- `get_menu_history_entry`: lê uma localização escolhida em um ponto histórico, sem alterá-la.
- `update_menus`: atualiza somente as localizações permitidas usando o `menus_hash` mais recente em `expected_hash` e relê o resultado.

Leia primeiro e preserve as localizações que não fazem parte do pedido. A estrutura visual do mega menu pode conter itens, colunas e subitens; use o formato retornado pela leitura como referência.

## Memória do menu

Quando o usuário descrever uma versão antiga por características — por exemplo, “o menu de três colunas” — comece com `list_menu_history`. Escolha candidatos pelo resumo e use `get_menu_history_entry` somente para inspecionar os pontos que correspondem à descrição.

Compare a estrutura histórica com `get_menus`. É permitido combinar a organização antiga com os links e opções atuais: reutilize apenas colunas, agrupamentos ou outra característica pedida e preserve todo o conteúdo atual que não faz parte da solicitação. Ao montar a gravação, use o `update_id` devolvido pelo ponto histórico como `id` do menu.

Se mais de um ponto combinar com a descrição, apresente as datas e diferenças e peça ao usuário para escolher. Depois explique a composição proposta, peça autorização, releia `get_menus`, envie seu `menus_hash` em `expected_hash` e confirme o resultado com outra leitura. As ferramentas de histórico nunca restauram o menu sozinhas.

## Modelos visuais

- `list_layout_models`: lista modelos cadastrados e o modelo selecionado em cada área.
- `select_layout_model`: seleciona um modelo existente com `area` igual a `header` ou `footer`.

O MCP não cria o arquivo de um modelo. Esta skill seleciona somente modelos já cadastrados. Quando o pedido exigir um modelo novo, informe que essa parte pertence à equipe da Orbite.

## Aparência global

Para cores, logos, tamanhos, botões e demais configurações globais do cabeçalho ou rodapé, use `$orbite-global-settings`. Esta skill continua responsável pelos links, pela estrutura dos menus e pela seleção de modelos existentes.

## Limites

- Não crie, edite nem publique arquivos do tema.
- Não crie novas localizações de menu por este fluxo.
- Não exclua menus.
- Não selecione um ID que não esteja no catálogo.
