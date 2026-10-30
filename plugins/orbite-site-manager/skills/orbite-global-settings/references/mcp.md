# MCP das configurações globais

Use a conexão MCP explicitamente escolhida para a tarefa. Em modo `self`, o destino é o próprio Site conectado em `/mcp`. Em modo `customer`, o destino é o Site salvo no painel da Orbite e a conexão usa `/mcp/customer`. Nenhuma ferramenta aceita uma URL de destino.

## Ferramentas

| Ação | Ferramenta |
| --- | --- |
| Listar áreas disponíveis | `list_global_settings_areas` |
| Consultar campos e valores de uma área | `get_global_settings_context` |
| Alterar campos validados de uma área | `update_global_settings_context` |
| Listar pontos de restauração | `list_global_settings_backups` |
| Restaurar um ponto | `restore_global_settings_backup` |

## Leitura contextual

A consulta retorna:

- `settings_hash`: identifica a versão lida.
- `settings`: contém os valores atuais solicitados.
- `fields`: explica nome, grupo, tipo, ajuda, opções e limites.
- `writable`: informa se o campo pode ser alterado por este fluxo.

Use o ID da área devolvido pela listagem. Quando o pedido envolver poucos campos, informe suas chaves em `keys` para reduzir o contexto.

## Atualização

Envie:

- `area`: a mesma área consultada.
- `changes`: somente as chaves que precisam mudar.
- `expected_hash`: o `settings_hash` da leitura mais recente.

O Site valida os valores, cria um backup, salva e devolve os campos confirmados. Se outro processo alterar as configurações depois da leitura, o hash será recusado.

## Restauração

Liste os backups antes de escolher um ID. A restauração recupera o Design System inteiro, preserva as credenciais atuais e cria um novo backup do estado anterior.

Não restaure apenas para testar e não escolha um backup sem identificação do usuário.

