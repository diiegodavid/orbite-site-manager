# MCP de navegação

Use a conexão MCP explicitamente escolhida para a tarefa. O destino é o Site identificado pela própria conexão em `/mcp`. Não aceite outra URL enviada no chat.

## Conteúdo dos menus

- `get_menus`: lê cabeçalho, cabeçalho do blog e rodapé.
- `update_menus`: atualiza somente as localizações permitidas e relê o resultado.

Leia primeiro e preserve as localizações que não fazem parte do pedido. A estrutura visual do mega menu pode conter itens, colunas e subitens; use o formato retornado pela leitura como referência.

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
