# MCP das configurações globais

Use a conexão MCP explicitamente escolhida para a tarefa. O destino é o Site identificado pela própria conexão em `/mcp`. Nenhuma ferramenta aceita uma URL de destino.

## Ferramentas

| Ação | Ferramenta |
| --- | --- |
| Listar áreas disponíveis | `list_global_settings_areas` |
| Consultar campos e valores de uma área | `get_global_settings_context` |
| Alterar campos ou listas validadas de uma área | `update_global_settings_context` |
| Listar coleções complexas | `list_global_collections` |
| Consultar uma coleção complexa | `get_global_collection_context` |
| Alterar uma coleção complexa | `update_global_collection` |
| Listar agendamentos | `list_schedule_appointments` |
| Criar um agendamento | `create_schedule_appointment` |
| Confirmar, concluir ou cancelar um agendamento | `update_schedule_appointment_status` |
| Remarcar um agendamento | `reschedule_appointment` |
| Listar pontos de restauração | `list_global_settings_backups` |
| Restaurar um ponto | `restore_global_settings_backup` |

## Leitura contextual

A consulta retorna:

- `settings_hash`: identifica a versão lida.
- `settings`: contém os valores atuais solicitados.
- `fields`: explica nome, grupo, tipo, ajuda, opções e limites.
- `writable`: informa se o campo pode ser alterado por este fluxo.

Use o ID da área devolvido pela listagem. Quando o pedido envolver poucos campos, informe suas chaves em `keys` para reduzir o contexto.

Uma lista que não pertence às sete coleções abaixo pode ser alterada neste fluxo somente quando seu campo vier com `writable: true`. Campos marcados como somente leitura precisam do gerenciador indicado pelo próprio contexto.

## Atualização

Envie:

- `area`: a mesma área consultada.
- `changes`: somente as chaves que precisam mudar.
- `expected_hash`: o `settings_hash` da leitura mais recente.

O Site valida os valores, cria um backup, salva e devolve os campos confirmados. Se outro processo alterar as configurações depois da leitura, o hash será recusado.

## Coleções complexas

Use este fluxo para:

| Coleção | ID |
| --- | --- |
| Equipe | `team` |
| Depoimentos | `testimonials` |
| Tabela de preços | `price-table` |
| Formulários | `forms` |
| Configuração da Agenda | `schedules` |
| Dúvidas comuns | `faq` |
| Logos | `brands` |

`get_global_collection_context` devolve os dados atuais, as operações permitidas e `collection_hash`. Não invente nomes de operações: use somente os apresentados em `capabilities`.

Para alterar, envie a `collection`, uma única operação em `operations` e o `expected_hash` mais recente. A operação informa seu `type`, o identificador do item quando necessário e somente os dados que precisam mudar. Faça mudanças maiores em etapas, relendo o hash depois de cada uma.

Exclusões, alterações financeiras ou itens compartilhados podem devolver o impacto e um `confirmation_token` descartável. Mostre o impacto ao usuário. Somente depois de uma nova autorização repita exatamente a mesma mudança com esse `confirmation_token`. O token vence em dez minutos e não serve para outra operação.

Depois de salvar, consulte novamente a coleção e confira os itens e relações alterados.

## Agenda e formulários

A coleção `schedules` contém apenas a configuração: agendas, disponibilidade e campos personalizados. Use as ferramentas de agendamentos para consultar ou administrar horários já reservados.

A coleção `forms` contém somente a configuração dos formulários. Não use este fluxo para ler respostas, contatos ou dados enviados pelos visitantes.

## Dados protegidos

Tokens de acesso, senhas, segredos e credenciais não pertencem a nenhum fluxo desta skill. Não tente encontrá-los no JSON amplo, em backups ou por outro caminho.

## Restauração

Liste os backups antes de escolher um ID. A restauração recupera o Design System inteiro, preserva as credenciais atuais e cria um novo backup do estado anterior.

Não restaure apenas para testar e não escolha um backup sem identificação do usuário.

