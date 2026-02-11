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
javascript:(()=>{if(window.__autoDecoder)return;window.__autoDecoder=1;const s=document.createElement("style");s.textContent="#__ansBox{position:fixed;bottom:24px;right:24px;z-index:999999;background:#111827;color:#fff;padding:18px;border-radius:18px;font:600 14px system-ui;width:340px;max-height:60vh;overflow:auto;box-shadow:0 20px 60px rgba(0,0,0,.6);border:2px solid #22c55e}#__ansBox h4{margin:0 0 10px 0;font-size:14px;color:#22c55e}#__ansBox div{margin-bottom:8px;padding:6px 8px;background:#1f2937;border-radius:8px}.__correctHighlight{outline:3px solid #22c55e!important;background:rgba(34,197,94,.15)!important;box-shadow:0 0 15px rgba(34,197,94,.7);border-radius:10px;position:relative}.__correctHighlight::after{content:'✔ Correta';position:absolute;top:-10px;right:-10px;background:#22c55e;color:#000;font-size:11px;padding:3px 6px;border-radius:999px;font-weight:700}";document.head.appendChild(s);const b=document.createElement("div");b.id="__ansBox";b.innerHTML="<h4>Respostas</h4><div id='__ansContent'></div>";document.body.appendChild(b);const c=b.querySelector("#__ansContent");function collect(){const r=new Set;document.querySelectorAll("input.alternativeList-item-input[data-correct='true']").forEach(i=>{const li=i.closest("li");if(li){li.classList.add("__correctHighlight");let t=li.innerText.trim();t=t.replace(/^([A-Za-z])[\.\s\)]*/,"$1) ");r.add(t)}});document.querySelectorAll(".blocks[data-correct-order]").forEach(e=>{try{const d=atob(atob(e.dataset.correctOrder));e.classList.add("__correctHighlight");r.add(d.trim())}catch{}});if(r.size){c.innerHTML=[...r].map(t=>"<div>"+t+"</div>").join("");b.style.display="block"}}const o=new MutationObserver(()=>{clearTimeout(window.__deb);window.__deb=setTimeout(collect,200)});o.observe(document.body,{childList:1,subtree:1});collect()})();
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
