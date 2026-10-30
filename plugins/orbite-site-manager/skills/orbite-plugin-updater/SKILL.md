---
name: orbite-plugin-updater
description: Configure e execute atualizações manuais ou automáticas, a cada 15 dias, do plugin Orbite instalado no Codex, sempre após autorização do usuário.
---

# Orbite Plugin Updater

Mantenha o plugin da Orbite atualizado sem ativar rotinas escondidas. A instalação oferece esta configuração, mas somente a autorização explícita do usuário permite criar a tarefa recorrente.

## Identificar o plugin

- Leia o nome do plugin no namespace completo desta skill, antes dos dois-pontos.
- Consulte `codex plugin list --json` sem alterar nada e localize o item instalado cujo campo `name` corresponde ao plugin identificado.
- Use somente o valor `marketplaceName` desse item como destino da atualização.
- Se o plugin ou o marketplace continuar ambíguo, pare e peça confirmação. Nunca atualize todos os marketplaces para tentar descobrir o correto.

## Regras obrigatórias

- Instalar o plugin não autoriza uma atualização nem a criação de uma tarefa agendada.
- Antes de agendar, explique em linguagem simples que a verificação acontecerá a cada 15 dias e depende do computador ligado e do Codex aberto.
- Pergunte se o usuário autoriza o agendamento e em qual horário prefere executá-lo.
- Crie, altere, pause ou remova a automação somente após um pedido ou uma autorização explícita do usuário.
- Se o Codex em uso não oferecer tarefas agendadas locais, explique o limite e entregue o comando manual. Não finja que a automação foi criada.
- Use sempre o comando específico do marketplace identificado. Nunca execute `codex plugin marketplace upgrade` sem informar o nome.
- Não remova nem reinstale o plugin como parte da atualização.
- Não exponha tokens, credenciais ou outros segredos em mensagens, comandos ou registros.

## Configuração após a instalação

1. Identifique o plugin e seu `marketplaceName` pelo fluxo seguro acima.
2. Explique que a atualização automática procura uma nova versão a cada 15 dias.
3. Informe que a tarefa é local e precisa do computador ligado e do Codex aberto no horário agendado.
4. Peça autorização e o horário preferido.
5. Se o usuário autorizar e o Codex oferecer essa função, crie uma tarefa recorrente local para executar o fluxo desta skill a cada 15 dias.
6. Na tarefa salva, registre o marketplace exato e instrua o Codex a avisar somente quando houver atualização, falha ou ação necessária.
7. Confirme o agendamento e informe quando será a próxima verificação.
8. Se o usuário não autorizar, não crie a tarefa e mostre somente o comando manual correspondente.

## Atualizar e conferir

1. Execute o comando de atualização correspondente ao pacote identificado.
2. Confira o resultado com `codex plugin list --marketplace <marketplace> --json`.
3. Se houver uma nova versão, informe a versão encontrada e oriente o usuário a abrir uma nova conversa para carregar as skills atualizadas.
4. Em uma tarefa agendada, permaneça em silêncio quando nada mudar. Avise somente se houver atualização, falha ou necessidade de ação do usuário.
5. Em uma execução manual, informe de forma breve se o plugin já estava atualizado.

## Atualização manual

Depois de identificar o `marketplaceName`, mostre ao usuário o comando exato no formato:

`codex plugin marketplace upgrade <marketplaceName>`

Substitua o marcador pelo valor confirmado em `codex plugin list --json`. Nunca entregue nem execute o comando com o marcador ainda presente.

