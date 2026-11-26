# MCP de componentes interativos

Use a conexão MCP escolhida para o Site da tarefa. Nenhuma ferramenta recebe uma URL de destino.

## Dados do componente

A tabela oficial possui somente:

- `id`: número automático do banco, usado internamente;
- `name`: nome único usado pelo usuário e pela IA;
- `html`: raiz HTML, `<style>` e `<script>`.

O `src` não é uma coluna. O Site o gera como `/media-component/{id}/`.

## Leituras

| Objetivo | Ferramenta |
| --- | --- |
| Listar nomes | `list_media_components` |
| Ler nome e HTML | `get_media_component` |

## Escritas

| Objetivo | Ferramenta |
| --- | --- |
| Criar | `create_media_component` |
| Editar o próprio registro | `update_media_component` |
| Excluir | `delete_media_component` |
| Aplicar na mídia de uma página | `apply_media_component_to_page` |
| Criar e aplicar | `create_media_component_and_apply` |
| Editar e aplicar | `update_media_component_and_apply` |

Criação e edição recebem somente `name` e `html`. O Site valida o documento e devolve o registro salvo. Não envie ID, status, versão, manifesto, runtime, hash de validação ou dados de publicação.

## Aplicação

- A aplicação localiza o componente pelo nome e grava sua rota no `src` da mídia escolhida.
- Use `viewport_scope` como `desktop`, `mobile` ou `both`.
- O alvo também confere o hash atual da página e o `src` atual para não sobrescrever outra mudança.
- `both` aplica no desktop e remove a substituição mobile; `mobile` preserva o desktop; `desktop` preserva o mobile.

## HTML

O valor deve conter, nesta ordem:

1. uma única raiz HTML;
2. um `<style>` sem atributos;
3. um `<script>` sem atributos.

A raiz deve possuir um ID simples. O script precisa declarar exatamente uma vez:

```js
const root = document.getElementById('ID-DA-RAIZ');
```

O ID usado no script deve ser o mesmo da raiz. O Site não fornece a variável `root` automaticamente.

Não use `setInterval`, `clearInterval`, workers ou outras APIs bloqueadas pelo executor. `Promise` é permitido para organizar sequências assíncronas. Para animações repetidas, use `setTimeout` recursivo ou `requestAnimationFrame`, respeitando os eventos de pausa e destruição do `OrbiteMedia`.

O Site rejeita PHP, recursos executáveis externos, eventos HTML inline, imports de CSS e JavaScript importado. Imagens HTTPS podem ser usadas em `<img>`.

## Falhas

- Nome duplicado: escolha outro nome ou edite o componente existente.
- Nome não encontrado: liste os componentes e confirme o nome exato.
- HTML inválido: corrija somente o problema informado e tente novamente.
- Página ou mídia alterada: leia a página novamente antes de aplicar.
- Leitura após escrita incompleta: não informe sucesso.
