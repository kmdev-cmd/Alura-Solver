# Bookmarklet – Decodificador de Ordem + Destaque de Alternativas

Bookmarklet em **JavaScript puro** que adiciona duas funcionalidades úteis diretamente no navegador, sem extensões:

1. **Decodifica valores Base64 duplos** presentes em elementos `.blocks[data-correct-order]` e exibe a ordem correta ao lado.
2. **Destaca visualmente** alternativas corretas quando existe `data-correct="true"` em inputs de múltipla escolha.

Cada função roda **apenas se o elemento existir na página**.

---

## Funcionalidades

### 1. Decodificação de ordem correta

Detecta elementos como:

```html
<div class="blocks" data-correct-order="TXl3eUxEUXNNUT09"></div>
```

O valor de `data-correct-order` é:

* Base64
* codificado **duas vezes**

O script:

* decodifica uma vez
* decodifica novamente
* exibe o resultado ao lado do elemento

Exemplo de processo:

```
TXl3eUxEUXNNUT09
↓ Base64
MywyLDQsMQ==
↓ Base64
3,2,4,1
```

Resultado exibido na página:

```
← ordem correta: 3,2,4,1
```

---

### 2. Destaque de alternativa correta

Detecta inputs como:

```html
<input type="radio" class="alternativeList-item-input" data-correct="true">
```

Ação:

* encontra o `<li>` pai
* aplica destaque visual (borda + fundo verde claro)
* **não bloqueia clique**

---

## Bookmarklet (linha única)

Copie **tudo** abaixo e cole como URL de um favorito:

```javascript
javascript:(()=>{const a=document.querySelectorAll('.blocks[data-correct-order]');a.length&&a.forEach(e=>{try{const t=atob(atob(e.getAttribute('data-correct-order')));if(e.nextSibling&&e.nextSibling.className==='decoded-order')return;const s=document.createElement('span');s.className='decoded-order';s.textContent=' ← ordem correta: '+t;s.style.cssText='color:#e74c3c;font-weight:700;margin-left:8px';e.after(s)}catch{}});const b=document.querySelectorAll('input.alternativeList-item-input[data-correct="true"]');b.length&&b.forEach(i=>{const l=i.closest('li');l&&(l.style.cssText+='outline:3px solid #2ecc71;background:rgba(46,204,113,.15);border-radius:6px')})})();
```

---

## Como usar

1. Crie um novo favorito no navegador
2. Cole o código acima no campo **URL**
3. Salve
4. Abra a página desejada
5. Clique no favorito

O script roda instantaneamente.

---

## Comportamento inteligente

* Se **existir apenas `.blocks[data-correct-order]`** → só a decodificação roda
* Se **existir apenas `data-correct="true"`** → só o destaque roda
* Se existirem ambos → ambos funcionam
* Não duplica resultados ao clicar mais de uma vez

---

## Tecnologias usadas (fontes oficiais)

* `atob()` – Base64 decoding
  **MDN Web Docs – Window.atob()**
* `querySelectorAll()`
  **MDN Web Docs**
* `Element.closest()`
  **MDN Web Docs**
* `data-* attributes`
  **HTML Living Standard**

Tudo nativo do navegador. Sem bibliotecas.

---

## Observações

* Funciona em Chrome, Firefox, Edge e derivados modernos
* Não altera backend
* Não faz requisições externas
* Apenas leitura e estilo visual

---

Uso educacional 
