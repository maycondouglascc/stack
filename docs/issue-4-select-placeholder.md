# Issue #4 — Item 5: Placeholder do `<select>`

## O que foi feito

No formulário da página inicial (`index.html`), a primeira opção do `<select>` foi alterada de:

```html
<option>Por favor, selecione uma categoria</option>
```

para:

```html
<option value="" disabled selected hidden>
  Por favor, selecione uma categoria
</option>
```

## Por que é necessário

### O problema original

Antes da correção, a primeira opção não possuía `value=""`, nem os atributos `disabled`, `selected` e `hidden`. Isso significa que:

- Se o usuário **não selecionasse nenhuma categoria** e enviasse o formulário, o texto `"Por favor, selecione uma categoria"` seria enviado como o valor do campo — um dado inválido.
- A opção ficava visualmente selecionável como qualquer outra, podendo confundir o usuário sobre se aquilo é um placeholder ou uma opção real.

### A correção

Cada atributo resolve um aspecto:

| Atributo | Função |
|---|---|
| `value=""` | Garante que o valor enviado seja uma string vazia (e não o texto do placeholder) caso o usuário não escolha nada |
| `disabled` | Impede que o usuário selecione esta opção intencionalmente |
| `selected` | Define esta opção como a padrão ao carregar a página |
| `hidden` | Remove a opção da lista visível (o navegador mostra o texto mas não aparece no dropdown) |

### Acessibilidade

Para leitores de tela, o atributo `hidden` combinado com `disabled` faz com que a opção seja ignorada na lista de opções disponíveis, enquanto o texto continua visível como label inicial do campo — o usuário de tecnologia assistiva ouve apenas as opções válidas ao abrir o select.

## Referências

- [MDN: `<select>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/select)
- [WCAG 3.3.2: Labels or Instructions (Level A)](https://www.w3.org/WAI/WCAG21/Understanding/labels-or-instructions)
