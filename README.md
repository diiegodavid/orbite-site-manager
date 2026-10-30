# Orbite Site Manager

Plugin público da Orbite para gerenciar conteúdo, aparência, navegação e componentes de um Site conectado ao Codex por MCP.

## Instalar no Codex

Execute os comandos abaixo no terminal:

```bash
codex plugin marketplace add diiegodavid/orbite-site-manager --ref main
codex plugin add orbite-site-manager@orbite-public
```

Depois da instalação, execute a configuração inicial do plugin. O Codex perguntará se você deseja autorizar a verificação automática de atualizações a cada 15 dias.

Se a configuração não abrir, inicie uma nova conversa e peça: **“Use a skill `orbite-plugin-updater` para configurar as atualizações deste plugin.”**

## Conectar seu Site

1. No WordPress, abra **Configurações do Agente de IA**.
2. Ative o MCP e salve a configuração.
3. Copie o **Endpoint MCP** e o **Token MCP** mostrados no painel.
4. Configure essa conexão no Codex.
5. Abra uma nova conversa para carregar as skills instaladas.

O token é exclusivo do Site. Não publique nem compartilhe esse valor em mensagens, issues ou arquivos.

## Atualizações

Se a atualização automática for autorizada durante a configuração inicial, o Codex procurará novas versões a cada 15 dias. Como essa tarefa é local, o computador precisa estar ligado e o Codex aberto no horário programado.

Para atualizar manualmente, execute:

```bash
codex plugin marketplace upgrade orbite-public
```

Depois de atualizar, abra uma nova conversa para carregar a nova versão das skills.

## O que o plugin permite

- criar e editar páginas usando templates já cadastrados;
- gerenciar artigos, categorias e SEO;
- ajustar configurações globais, menus, cabeçalhos e rodapés;
- criar componentes interativos nos espaços de mídia.

A criação de arquivos do tema, como PHP, SCSS e JavaScript do tema, continua reservada à equipe Orbite.

