# Relatório de Análise e Sugestões de Melhoria — StackHub

Olá! Como seu professor, analisei o código HTML e CSS da página inicial do **StackHub**. Parabéns pelo trabalho inicial! O layout está bem estruturado e o uso de OKLCH para cores mostra atenção às tendências modernas de design. 

No entanto, identifiquei alguns erros de sintaxe (bugs), redundâncias e excelentes oportunidades para melhorar a acessibilidade (a11y) e a semântica do seu código. Abaixo, organizei o relatório em categorias claras para te guiar na correção e evolução do projeto.

---

## 1. Bugs e Erros de Sintaxe (CSS)

Estes são erros que fazem com que algumas propriedades não funcionem ou que o navegador ignore certas regras:

*   **Variável de cor incompleta:**
    *   **Onde:** [styles.css:L57](file:///c:/Users/dtiDigital/Documents/Coding/stack/styles.css#L57) (`color: --tex;`)
    *   **Problema:** A sintaxe está incompleta e incorreta. Deveria ser algo como `color: var(--text-primary);` ou simplesmente removida, já que a cor padrão do texto já está definida no seletor universal `*`.
*   **Variáveis não declaradas no `:root`:**
    *   **Onde:** 
        *   [styles.css:L70](file:///c:/Users/dtiDigital/Documents/Coding/stack/styles.css#L70) (`color: var(--color-primary);`)
        *   [styles.css:L110](file:///c:/Users/dtiDigital/Documents/Coding/stack/styles.css#L110) (`color: var(--color-primary);`)
        *   [styles.css:L88](file:///c:/Users/dtiDigital/Documents/Coding/stack/styles.css#L88) (`box-shadow: 0 1px 2px 0 var(--shadow-sm);`)
    *   **Problema:** Nem `--color-primary` nem `--shadow-sm` estão declarados no seu bloco `:root` (linhas 25-34). Isso fará com que o navegador aplique o fallback ou a cor herdada.
    *   **Correção:** Adicione essas variáveis ao `:root` ou substitua-as pelas variáveis existentes (ex: `--text-primary` e `--shadow-lg`).
*   **Pseudo-elemento `::after` em `<select>`:**
    *   **Onde:** [styles.css:L280-291](file:///c:/Users/dtiDigital/Documents/Coding/stack/styles.css#L280-L291) (`.form select::after`)
    *   **Problema:** Elementos `<select>` são elementos substituídos (*replaced elements*), o que significa que o navegador controla sua renderização interna. Por conta disso, pseudo-elementos como `::before` e `::after` **não funcionam** diretamente neles na maioria dos navegadores modernos.
    *   **Correção:** Envolva o `<select>` em uma `div` contêiner (ex: `.select-wrapper`) e aplique o pseudo-elemento na `div`.

---

## 2. Acessibilidade (a11y) e Semântica (HTML)

Acessibilidade é fundamental para garantir que leitores de tela e navegação por teclado funcionem corretamente.

*   **Contraste de Cores Insuficiente:**
    *   **Onde:** A cor `--text-secondary` definida como `oklch(65.711% 0.02374 235.316)` possui uma luminosidade de aproximadamente 65%. 
    *   **Problema:** Quando usada para textos pequenos (como `.text-light` ou placeholders de formulário) sobre fundo branco, a taxa de contraste fica abaixo de **4.5:1** (violando as diretrizes WCAG AA). Isso dificulta a leitura para pessoas com baixa visão.
    *   **Sugestão:** Aumente o contraste escurecendo a cor secundária (reduzindo a luminosidade no OKLCH).
*   **Links Vazios e Comportamento Confuso:**
    *   **Onde:** [index.html:L20-45](file:///c:/Users/dtiDigital/Documents/Coding/stack/index.html#L20-L45) e botões do hero [L62-63](file:///c:/Users/dtiDigital/Documents/Coding/stack/index.html#L62-L63).
    *   **Problema:** Os links da navegação possuem `target="_blank"` combinados com `href="#"` ou `href="http://"`. Abrir uma página vazia ou a mesma página em uma nova aba gera uma péssima experiência de usuário.
    *   **Sugestão:** Remova `target="_blank"` para links internos/âncoras e corrija os `href` para apontar para seções válidas (ex: `href="#explorar"`).
*   **Imagens sem Alt Descritivo:**
    *   **Onde:** [index.html:L83](file:///c:/Users/dtiDigital/Documents/Coding/stack/index.html#L83), [L97](file:///c:/Users/dtiDigital/Documents/Coding/stack/index.html#L97) e [L111](file:///c:/Users/dtiDigital/Documents/Coding/stack/index.html#L111).
    *   **Problema:** O atributo `alt` de todas as imagens de feature está definido como `"feature1"`. Isso não explica o que a imagem representa para um usuário de leitor de tela.
*   **Seções sem Títulos/Landmarks:**
    *   **Onde:** [index.html:L126](file:///c:/Users/dtiDigital/Documents/Coding/stack/index.html#L126) e [L183](file:///c:/Users/dtiDigital/Documents/Coding/stack/index.html#L183).
    *   **Problema:** Você usou elementos `<section>` sem nenhum cabeçalho (`<h2>`-`<h6>`) ou atributo de identificação (`aria-labelledby`). O validador de HTML do W3C e leitores de tela esperam que toda `<section>` represente uma região temática com título.
*   **Primeira opção do Select como valor padrão:**
    *   **Onde:** [index.html:L219](file:///c:/Users/dtiDigital/Documents/Coding/stack/index.html#L219) (`<option>Por favor, selecione uma categoria</option>`)
    *   **Problema:** Se o usuário não selecionar nada, essa frase será enviada como o valor do formulário.
    *   **Correção:** Altere para `<option value="" disabled selected hidden>Por favor, selecione...</option>`.

---

## 3. Redundâncias e Melhores Práticas de CSS

*   **Estilos Redundantes no Seletor Universal:**
    *   Você definiu `font-weight: 500;` e `color: var(--text-primary);` no `*`. Isso força todos os elementos (incluindo títulos, botões e spans) a terem o mesmo peso e cor padrão, o que tira o benefício da herança natural do CSS a partir do `body`. 
    *   **Melhor Prática:** Aplique fontes e cores base no `body` ou `:root` e permita que a especificidade natural trabalhe.
*   **Seletores excessivamente amplos:**
    *   O seletor `.form div` ([styles.css:L239](file:///c:/Users/dtiDigital/Documents/Coding/stack/styles.css#L239)) aplica `display: flex; flex-direction: column; gap: 4px;` a **todas** as divs filhas, netas, etc., dentro do formulário. Isso pode causar problemas inesperados se você decidir aninhar divs para outros fins de layout.
    *   **Melhor Prática:** Use classes específicas para os grupos de campos (ex: `.form-group`).
*   **Altura Fixa no Form:**
    *   **Onde:** [styles.css:L221](file:///c:/Users/dtiDigital/Documents/Coding/stack/styles.css#L221) (`height: 480px;`)
    *   **Problema:** Fixar alturas em elementos com conteúdo dinâmico ou de texto (como formulários) pode causar transbordamento (*overflow*) se o texto aumentar ou a tela diminuir.
    *   **Melhor Prática:** Utilize `min-height: 480px;` ou deixe que o padding e o conteúdo definam a altura naturalmente.

