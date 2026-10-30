# Padrão de conteúdo dos artigos

Adapte tema, vocabulário, tom, localização, exemplos e chamada final ao Site conectado. Não use a identidade da Orbite por padrão.

## Estrutura

- O título do WordPress funciona como o único `h1`; não repita um `h1` dentro do conteúdo.
- Use `h2` para blocos principais e `h3` somente quando houver uma subdivisão real.
- Prefira parágrafos curtos, listas úteis e exemplos concretos.
- Não use estilos inline, scripts, iframes ou classes inventadas.
- Entregue HTML simples e semântico compatível com o editor do WordPress.
- Não coloque FAQ no conteúdo do artigo.

## Tabela obrigatória

Todo artigo deve ter ao menos uma tabela que realmente ajude a explicar ou comparar o assunto. Não escreva uma tag HTML `<table>`.

Use exatamente o shortcode abaixo, com as aspas do JSON convertidas para `&quot;`:

```text
[orbite_table]{&quot;thead&quot;:[&quot;Coluna 1&quot;,&quot;Coluna 2&quot;],&quot;tbody&quot;:[[&quot;Valor 1&quot;,&quot;Valor 2&quot;]]}[/orbite_table]
```

- Use apenas `thead` e `tbody`.
- Não adicione `<br>`, espaços ou outros caracteres entre o shortcode e o JSON, nem entre `}` e `[/orbite_table]`.
- Não use `class`, `style` ou `id` no conteúdo da tabela.

## SEO

- O título precisa explicar com clareza o benefício ou assunto central.
- O slug deve ser curto, descritivo e sem palavras desnecessárias.
- A meta description deve resumir a promessa do artigo sem repetir mecanicamente o título.
- Use a palavra central naturalmente; não repita termos apenas para aumentar frequência.
- Evite criar um artigo que concorra com outro conteúdo já existente sobre o mesmo assunto.

## Qualidade

- Não invente dados, números, clientes, depoimentos ou resultados.
- Quando a tarefa exigir pesquisa atual, priorize fontes oficiais e mantenha apenas os links relevantes.
- Adapte o tom ao público e às orientações dadas pelo usuário.
- Termine com uma próxima ação útil quando isso fizer sentido para o objetivo do artigo.

