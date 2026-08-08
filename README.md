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

## Bookmarklet

Copie o codigo a seguir e cole como URL de um favorito (a fonte legivel esta em `bookmarklet.js`):

```javascript
javascript:(() => { if (window.__autoDecoder) return; window.__autoDecoder = 1; const STYLE_ID = "__ansStyle"; const BOX_ID = "__ansBox"; const LAST_KEY = "__ansLast"; if (!document.getElementById(STYLE_ID)) { const s = document.createElement("style"); s.id = STYLE_ID; s.textContent = [ `#${BOX_ID}{display:none;position:fixed;bottom:24px;right:24px;z-index:999999;background:#111827;color:#fff;padding:18px;border-radius:18px;font:600 14px system-ui;width:340px;max-height:60vh;overflow:auto;box-shadow:0 20px 60px rgba(0,0,0,.6);border:2px solid #22c55e}`, `#${BOX_ID} h4{margin:0 0 10px 0;font-size:14px;color:#22c55e}`, `#${BOX_ID} div{margin-bottom:8px;padding:6px 8px;background:#1f2937;border-radius:8px}`, ".__correctHighlight{outline:3px solid #22c55e!important;background:rgba(34,197,94,.15)!important;box-shadow:0 0 15px rgba(34,197,94,.7);border-radius:10px;position:relative}", ".__correctHighlight::after{content:'✔ Correta';position:absolute;top:-10px;right:-10px;background:#22c55e;color:#000;font-size:11px;padding:3px 6px;border-radius:999px;font-weight:700}", ".__correctSort{outline:3px solid #22c55e!important;background:rgba(34,197,94,.15)!important;border-radius:8px}", ".__correctSort::after{content:'ordem correta';position:absolute;top:-10px;right:-10px;background:#22c55e;color:#000;font-size:11px;padding:3px 6px;border-radius:999px;font-weight:700}" ].join(""); document.head.appendChild(s); } let box = document.getElementById(BOX_ID); if (!box) { box = document.createElement("div"); box.id = BOX_ID; const h = document.createElement("h4"); h.textContent = "Respostas"; box.appendChild(h); document.body.appendChild(box); } const content = document.createElement("div"); content.id = "__ansContent"; box.appendChild(content); function collect() { const items = []; document.querySelectorAll("input.alternativeList-item-input[data-correct='true']").forEach((input) => { const li = input.closest("li"); if (!li) return; li.classList.add("__correctHighlight"); const text = (li.innerText || li.textContent || "").trim().replace(/^([A-Za-z])[.\s)]*/, "$1) "); if (text) items.push(text); }); document.querySelectorAll(".blocks[data-correct-order]").forEach((el) => { try { const decoded = atob(atob(el.dataset.correctOrder)).trim(); el.classList.add("__correctSort"); if (decoded) items.push(decoded); } catch (_) { /* dado malformado: ignora */ } }); const html = items.map((t) => "<div>" + escapeHtml(t) + "</div>").join(""); if (content.innerHTML !== html) { content.innerHTML = html; box.style.display = items.length ? "block" : "none"; } } function escapeHtml(text) { const div = document.createElement("div"); div.textContent = text; return div.innerHTML; } const observer = new MutationObserver(() => { clearTimeout(window.__deb); window.__deb = setTimeout(collect, 200); }); observer.observe(document.body, { childList: true, subtree: true }); collect(); })();
```

## Licenca

Apache 2.0
