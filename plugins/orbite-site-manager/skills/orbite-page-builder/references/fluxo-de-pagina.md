# Fluxo para construir uma página

## 1. Entender o pedido

Identifique quatro itens:

- imagem de referência;
- slug da página existente ou título e slug da nova página;
- ambiente e endereço para visualização;
- partes que podem exigir conteúdo de página, menu ou configurações globais.

Se faltar algo indispensável, faça somente uma pergunta curta por vez.

## 2. Diagnosticar antes de editar

Compare a referência com os componentes disponíveis. Separe o trabalho em:

- templates já cadastrados que podem ser usados;
- componentes e estilos já existentes que podem ser reutilizados;
- conteúdo que será preenchido pelo MCP;
- diferenças que o Design System não permite reproduzir exatamente.

Se a referência exigir um template novo ou uma alteração em arquivo do tema, informe que essa parte pertence à equipe da Orbite. O cliente pode gerar código somente em componentes interativos de mídia.

Apresente uma lista curta das soluções e peça autorização. Não considere a chamada da skill como autorização para editar.

## 3. Preencher conteúdo pelo MCP

Depois da autorização:

1. Selecione explicitamente a conexão MCP correspondente ao modo, ao Site e ao ambiente autorizados.
2. Liste as páginas quando o destino ainda não estiver claro.
3. Se a página não existir, crie-a como rascunho com `create_page`.
4. Leia a página pelo slug e guarde seu `content_hash`.
5. Monte a alteração usando a estrutura real do JSON recebido e somente templates já cadastrados.
6. Atualize apenas as seções e campos necessários, enviando o hash. O WordPress cuidará das revisões nativas da página.
7. Use configurações globais e menus somente quando a referência realmente exigir.
8. Leia tudo novamente para confirmar a gravação.

Quando a página precisar de uma imagem nova, gere o arquivo no Codex e envie-o com `upload_image`. Use o ID ou a URL retornados pelo MCP no conteúdo da página. Para reaproveitar uma imagem existente, pesquise primeiro com `list_images`.

O MCP cria a página somente como rascunho. Não publique, exclua nem crie arquivos de template por este fluxo.

## 4. Conferir no navegador

Abra a URL configurada para o ambiente e compare com a imagem de referência.

Quando precisar abrir o ChatPanel, navegar pelos templates ou observar a aparência de um wireframe, leia e siga [validacao-visual.md](validacao-visual.md). Esse fluxo usa somente o navegador interno do Codex e funciona tanto no ambiente local quanto em produção.

Confira, no mínimo:

- ordem e quantidade das seções;
- hierarquia dos textos;
- espaçamentos segundo as escalas do Design System;
- cores, bordas e fundos;
- imagens e proporções;
- comportamento responsivo quando houver informação suficiente;
- menus e conteúdo global relacionados à página.

Faça ciclos pequenos: observar, ajustar uma causa, recarregar e comparar novamente. Busque o resultado mais próximo possível, sem quebrar os padrões do Design System para alcançar igualdade artificial.

## 5. Concluir

Só considere o trabalho concluído quando:

- o template escolhido já estiver cadastrado e carregando;
- o conteúdo estiver confirmado por uma leitura posterior do MCP;
- a página tiver sido conferida no navegador;
- os testes proporcionais ao risco tiverem sido executados;
- nenhum arquivo do tema tiver sido criado ou alterado.

Na entrega, diga de forma simples quais conteúdos foram atualizados, qual template existente foi usado, como a página foi validada e quais diferenças visuais ainda exigem trabalho interno da Orbite.
