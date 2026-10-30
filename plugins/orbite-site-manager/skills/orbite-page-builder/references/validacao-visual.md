# Validação visual no ChatPanel

Use este fluxo somente quando a tarefa precisar observar a aparência dos wireframes ou conferir a página pelo ChatPanel. Ele pode ser usado no ambiente local e em produção.

## Navegador e Site

- Use somente o navegador interno do Codex (`@Browser`). Não use o Chrome ou outro navegador externo.
- Acesse o mesmo Site e ambiente já confirmados para a tarefa.
- Termine os endereços de página com `/`.
- A navegação e a inspeção visual não autorizam selecionar ou aplicar outro template.

### Endereço local

No ambiente local, nunca continue por um endereço que recebeu uma porta interna.

- Correto: `https://cliente.local/wp-admin/`
- Errado: `https://cliente.local:10012/wp-admin/`

Se houver redirecionamento para uma URL com porta, volte para a URL canônica do Site sem a porta e com a barra final antes de continuar.

## Login no WordPress

1. Abra `/wp-admin/` no Site confirmado.
2. Se o painel já estiver autenticado, continue.
3. Se aparecer a tela de login, use somente as credenciais que já estiverem preenchidas pelo próprio navegador do Codex.
4. Clique em **Acessar** apenas quando os campos de usuário e senha já estiverem preenchidos.
5. Se algum campo estiver vazio, não digite, não procure e não tente adivinhar credenciais. Pare e informe que o acesso não está salvo no navegador.
6. Confirme que o painel do WordPress abriu antes de prosseguir.

Não registre usuário ou senha na skill, no repositório, no MCP ou no resumo da tarefa.

## Abrir os templates

1. Abra a página inicial do Site usando a URL com `/` no final.
2. Localize o campo do ChatPanel centralizado no canto inferior da página.
3. Passe o mouse sobre o campo para revelar a barra de ícones.
4. Mantenha o ponteiro dentro da área do ChatPanel. Se ele sair antes de um ícone ser acionado, os ícones voltam a ficar ocultos.
5. Passe o mouse sobre o segundo ícone da esquerda.
6. Espere o tooltip aparecer e confirme que o texto é **Templates**. Não escolha o ícone apenas pela aparência.
7. Clique no ícone somente depois dessa confirmação.
8. Abra a categoria solicitada. Para ver todos os modelos, abra **Tudo**.
9. Role dentro do painel de templates, não a página por trás dele, até localizar o número do wireframe solicitado.
10. Observe o wireframe e use capturas do navegador para comparar sua aparência antes e depois dos ajustes.

Não clique no cartão do wireframe nem aplique um template diferente quando o pedido for apenas observar, comparar ou melhorar sua aparência. Faça isso somente quando o usuário pedir explicitamente a seleção ou a aplicação do modelo.

## Ciclo de conferência

Ao ajustar uma página pelo MCP:

1. observe a aparência atual no ChatPanel;
2. altere somente os campos autorizados no JSON da página;
3. releia a página pelo MCP para confirmar a gravação;
4. recarregue a página no navegador do Codex;
5. volte a **Templates**, abra a categoria necessária e localize novamente o wireframe;
6. compare o resultado e faça apenas ajustes pequenos e verificáveis.
