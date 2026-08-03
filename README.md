# Alura-Solver

Bookmarklet em JavaScript puro que auxilia estudantes da plataforma Alura a visualizar a ordem correta de exercícios de ordenacao e destacar as alternativas corretas em questoes de multipla escolha, tudo direto no navegador, sem extensoes.

## Funcionalidades

- Decodifica valores Base64 codificados duas vezes presentes em elementos `.blocks[data-correct-order]` e exibe a ordem correta ao lado.
- Destaca visualmente alternativas marcadas com `data-correct="true"` em inputs de multipla escolha.
- Roda apenas se o elemento existir na pagina, sem interferir em outras paginas.
- Nao bloqueia o clique nas alternativas.
- Protecao contra duplicacao de resultados ao clicar mais de uma vez.
- Sem dependencias externas, apenas APIs nativas do navegador.

## Como usar

1. Crie um novo favorito no navegador.
2. Cole o codigo `javascript:` (presente na secao "Bookmarklet" deste README) no campo URL.
3. Salve o favorito.
4. Abra a pagina desejada e clique no favorito.

O script executa instantaneamente e exibe um painel com as respostas identificadas.

## Licenca

Apache 2.0
