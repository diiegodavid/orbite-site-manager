# MCP de componentes de mídia

Use a conexão MCP explicitamente escolhida para a tarefa.

- Em modo `self`, o destino é o próprio Site conectado em `/mcp`.
- Em modo `customer`, o destino é o Site salvo no painel da Orbite e a conexão usa `/mcp/customer`.
- Nenhuma ferramenta aceita uma URL de destino.

## Leituras

| Objetivo | Ferramenta |
| --- | --- |
| Obter Design System, runtime, limites e contexto assinado | `get_media_component_context` |
| Listar componentes sem fonte | `list_media_components` |
| Ler metadados de um componente | `get_media_component` |
| Listar versões sem fonte | `list_media_component_versions` |
| Ler uma versão exata para edição | `get_media_component_version` |
| Descobrir usos em páginas | `list_media_component_usages` |
| Validar sem salvar ou executar | `validate_media_component` |

`get_media_component_version` exige número de versão e `include_source: true`. Não use essa leitura para montar uma galeria ou procurar componentes.

## Escolha do runtime

- Use `trusted-inline@1` para componentes novos criados ou aprovados por administrador/Codex. Ele monta o componente em Shadow DOM, sem iframe.
- Preserve `vanilla-web@1` somente para versões legadas ou código que precise continuar isolado em iframe.
- Não migre silenciosamente um componente existente entre runtimes. Uma mudança de runtime precisa aparecer na proposta e ser autorizada.

## Manifesto `trusted-inline@1`

Envie ao validador somente o manifesto proposto: `schema_version`, `kind`, `runtime`, `runtime_version`, `source_format`, `source_format_version`, `protocol_version`, `host_profiles`, `root_id`, `color_policy`, `requested_capabilities` e `assets`. Campos desconhecidos são rejeitados.

- `kind`: `interactive-graphic`.
- Runtime e formato: `trusted-inline@1` e `html-css-js@1`.
- Protocolo e host: `orbite-media-v1` e `['media-inline-v1']`.
- `root_id`: use exatamente o valor criado pelo Site no contexto.
- `requested_capabilities`: combinação sem repetição de `pointer`, `keyboard`, `animation` e `host-action`.
- `assets`: vazio nesta entrega.

A fonte continua sendo um único valor versionado, mas contém três blocos nesta ordem: uma raiz HTML com o `root_id`, um `<style>` com CSS direto e um `<script>` com JavaScript. Nunca envie SCSS ou outro pré-processador.

O runtime inline é confiável: Shadow DOM separa a estrutura e o CSS visualmente, mas o JavaScript roda no mesmo documento do Site. Use somente fonte criada ou aprovada por administrador/Codex. Ações próprias do Site passam pela API do host e por uma lista permitida; para contratação, use `OrbiteMedia.requestHostAction('open-start-plan')` com a capacidade `host-action`.

## Manifesto legado `vanilla-web@1`

Ao editar uma versão que já use o runtime legado, preserve `single-document@1`, `media-slot-v1` e somente as capacidades `pointer`, `keyboard` e `animation`. Siga [runtime-vanilla-web.md](runtime-vanilla-web.md); não use esse contrato como padrão de uma criação nova.

O manifesto não contém placement, página, seção, proporção, fit ou binding. Esses dados pertencem ao `target` da operação. Não envie `runtime_artifact` nem `granted_version_capabilities`; o Site acrescenta esses campos no manifesto canônico validado e pinado por hash.

## Escritas

| Objetivo | Ferramenta |
| --- | --- |
| Criar somente na biblioteca | `create_media_component` |
| Criar nova versão | `create_media_component_version` |
| Criar prévia temporária | `create_media_component_preview` |
| Publicar versão | `publish_media_component_version` |
| Renomear | `rename_media_component` |
| Restaurar versão como uma nova versão | `restore_media_component_version` |
| Arquivar ou reativar | `archive_media_component`, `unarchive_media_component` |
| Aplicar versão já publicada | `apply_media_component_to_page` |
| Criar e aplicar em uma operação | `create_media_component_and_apply` |
| Versionar e aplicar em uma operação | `create_media_component_version_and_apply` |
| Restaurar e aplicar em uma operação | `restore_media_component_version_and_apply` |

Uma prévia também é escrita e precisa de autorização. Envie a revisão, o estado do componente e o estado da versão lidos imediatamente antes, além de um TTL entre 30 e 600 segundos. Arquivar não apaga histórico; quando houver usos, faça a primeira chamada para receber o impacto e o `archive_confirmation_id`. Só repita o arquivamento com esse identificador no campo de mesmo nome depois de o usuário confirmar. Ele é curto, descartável e vinculado ao Site, ator, revisão e conjunto atual de usos; nunca tente substituí-lo por um hash criado pelo agente.

## Concorrência e confirmação

- Toda escrita usa `request_id` e `idempotency_key` únicos.
- Toda escrita sobre componente existente exige `expected_component_revision` e `expected_component_status` lidos imediatamente antes.
- Criar versão, publicar, restaurar e suas variantes atômicas também exigem `expected_latest_version`, `expected_published_version` e `expected_version_status`. Em criação de versão, esse estado é o da versão mais recente; em publicação, é o da versão alvo; em restauração, é o da versão de origem.
- Renomear, arquivar e desarquivar exigem os dois ponteiros esperados (`expected_latest_version` e `expected_published_version`), inclusive quando o valor correto for `null`.
- Aplicação e prévia exigem o estado esperado da versão. A aplicação também usa `expected_page_hash`, `expected_current_src`, `template_id` e o `media_path` exato.
- Aplique apenas uma versão publicada e compatível com o host do próprio runtime: `media-inline-v1` para `trusted-inline@1` ou `media-slot-v1` para o legado.
- Se já houver alvo, use a operação atômica correspondente. Não crie primeiro e tente consertar a página depois.
- Ao editar, só aplique automaticamente quando o alvo já usar o mesmo componente ou quando o usuário pedir explicitamente para usar/aplicar. Caso contrário, publique a nova versão somente na biblioteca.
- Só informe sucesso depois do readback real do componente, versão e ponteiros. Quando houver aplicação, confira também a página e o `src`. Respostas confirmadas incluem nome, `reference_code`, revisão, versão, estado e `src` quando houver aplicação.

## Fonte e imagens

Listas, históricos e usos nunca retornam fonte. A validação devolve manifesto canônico e `validation_hash`; use exatamente esses valores na escrita seguinte. O Site valida novamente antes de gravar.

Anexos usados como inspiração ficam somente no contexto do modelo. Não inclua base64 nem o conteúdo binário da imagem nas chamadas de componentes. No runtime `trusted-inline@1`, uma imagem pública pode aparecer em `<img src="https://...">`; URLs no CSS, scripts, imports e outros recursos remotos continuam proibidos. Para maior estabilidade, prefira uma mídia aprovada da galeria quando o usuário não pedir um link externo.

## Falhas

- Contexto expirado: leia um novo contexto e gere novamente.
- ID não encontrado, nome ambíguo ou nenhum nome parecido: peça o ID curto e oriente a consulta em Componentes no painel.
- Hash ou revisão desatualizados: releia e apresente uma nova proposta.
- Runtime ou host incompatível: não tente trocar artefatos, pacotes ou hashes manualmente.
- Readback incompleto: trate como operação não confirmada.
