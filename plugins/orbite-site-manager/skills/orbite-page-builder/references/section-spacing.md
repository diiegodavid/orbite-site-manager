# Espaçamento vertical das seções

Use este contrato quando o usuário pedir margem, padding, espaço ou espaçamento vertical de uma seção. Apesar do nome histórico `margin`, esses campos controlam o padding do topo e da base.

## Campos e valores

Os níveis vão de 0x a 6x. Grave o zero explícito como `"none"`; o número `0` é legado e significa usar o padrão global do Site.

| Campo em `classes` | Efeito |
| --- | --- |
| `margin` | Topo e base no desktop e como nível herdado pelo mobile |
| `margin-top` | Somente o topo |
| `margin-bottom` | Somente a base |
| `margin-mobile` | Topo e base exclusivamente no mobile |
| `margin-top-mobile` | Somente o topo no mobile |
| `margin-bottom-mobile` | Somente a base no mobile |

Use números inteiros de 1 a 6 para os níveis positivos. Não escreva classes CSS nem converta níveis em pixels: cada template aplica os tamanhos já cadastrados para hero, seção comum e fundo arredondado.

## Herança

No desktop, cada campo individual tem prioridade sobre `margin`. Sem ajuste individual nem `margin`, o lado usa o padrão global do Site.

No mobile, cada campo individual mobile tem prioridade sobre `margin-mobile`. Sem ajuste mobile, o lado herda o nível efetivo correspondente do desktop. Assim, a ausência dos campos mobile mantém o comportamento responsivo já existente.

Para descobrir o valor atual de um pedido relativo, resolva cada lado nessa mesma ordem. Se o desktop depender do padrão global, consulte `root_section_default_margin` pelas ferramentas contextuais de configurações globais apenas para leitura; não altere a configuração global neste fluxo.

## Como interpretar o pedido

- Sem lado informado, ajuste topo e base.
- Com topo, acima ou superior, ajuste somente o topo.
- Com base, abaixo, inferior ou bottom, ajuste somente a base.
- Aumentar ou diminuir sem quantidade altera um nível.
- “Muito”, “bastante”, “bem mais” ou “bem menos” altera dois níveis.
- Uma quantidade explícita de níveis prevalece e o resultado sempre fica entre 0 e 6.
- Remover, zerar ou deixar sem espaço grava `"none"`.
- “Somente no mobile”, “no celular” ou equivalente usa exclusivamente os campos mobile.

Referências como “acima do título” ou “abaixo do último botão, formulário ou conjunto de imagens” pertencem a este fluxo somente quando descrevem o espaço entre o conteúdo e a borda da seção. Espaço entre elementos internos não deve alterar a margem da seção.

## Alteração mínima

- Quando os dois lados receberem o mesmo nível, grave o campo geral correspondente e remova os ajustes individuais daquele mesmo ambiente.
- Quando apenas um lado for solicitado, altere somente seu campo individual e preserve o outro lado.
- Para voltar o mobile ao automático, remova os campos mobile envolvidos; não copie o valor do desktop para eles.
- Preserve todos os demais campos da página e envie o `content_hash` atual em `expected_hash` ao usar `update_page`.
- Depois de salvar, leia novamente a página e confirme os campos alterados.

Exemplo com desktop separado e mobile automático:

```json
{
  "classes": {
    "margin": 3,
    "margin-top": "none",
    "margin-bottom": 5
  }
}
```

Exemplo com ajuste exclusivo de mobile:

```json
{
  "classes": {
    "margin": 3,
    "margin-top-mobile": 1,
    "margin-bottom-mobile": "none"
  }
}
```
