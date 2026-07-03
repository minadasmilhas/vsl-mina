# Como Criar Uma Nova Página VSL

## Passo 1: Criar a Pasta
```bash
mkdir nome-da-pagina
```

## Passo 2: Copiar o Template
```bash
cp sistema/index.html nome-da-pagina/index.html
cp -r sistema/images nome-da-pagina/
```

## Passo 3: Editar o Arquivo
- Abra `nome-da-pagina/index.html`
- Altere o `<title>` com o nome da página
- Customize o conteúdo conforme necessário

## Passo 3.1: Adicionar Meta Tags (Importante!)

**Adicione no `<head>` após o `<title>`:**

```html
<!-- Open Graph Meta Tags (para compartilhamento em redes sociais) -->
<meta property="og:title" content="Seu Título Aqui">
<meta property="og:description" content="Sua descrição curta aqui">
<meta property="og:image" content="https://lp.amindasmilhas.com.br/sistema/images/thumb-meta-open-graph.webp">
<meta property="og:url" content="https://lp.amindasmilhas.com.br/nome-da-pagina/">
<meta property="og:type" content="website">
```

**Substitua:**
- `Seu Título Aqui` → Título da página
- `Sua descrição curta aqui` → Descrição (1-2 linhas)
- `nome-da-pagina` → Nome da sua página

Essas meta tags controlam o preview quando compartilha no Instagram, WhatsApp, Facebook, etc.

**Para validar/atualizar o cache:**
1. Vá ao Meta Debugger: https://developers.facebook.com/tools/debug/
2. Digite sua URL
3. Clique "Scrape Again"

## ⚠️ IMPORTANTE: Script de Rastreio

**O script de rastreio de UTMs DEVE estar presente em TODAS as páginas!**

Ele já vem incluído quando você copia de `sistema/index.html`, mas VERIFIQUE:

1. Procure por `<!-- ============================================================ Script de Rastreio de UTMs`
2. Se não encontrar, adicione **antes do fechamento `</body>`**:

```html
<!-- ============================================================
     Script de Rastreio de UTMs e Parâmetros de Anúncios
============================================================ -->
<script>
(function () {
    function getParameterByName(name, url) {
        if (!url) url = window.location.href;
        name = name.replace(/[\[\]]/g, "\\$&");
        var regex = new RegExp("[?&]" + name + "(=([^&#]*)|&|#|$)"),
            results = regex.exec(url);
        if (!results) return null;
        if (!results[2]) return '';
        return decodeURIComponent(results[2].replace(/\+/g, " "));
    }

    function setCookie(cookieName, cookieValue, expirationTime) {
        var cookiePath = "/";
        expirationTime = expirationTime * 1000;
        var date = new Date();
        var dateTimeNow = date.getTime();
        date.setTime(dateTimeNow + expirationTime);
        var expirationDate = date.toUTCString();
        document.cookie = cookieName + "=" + cookieValue + "; expires=" + expirationDate + "; path=" + cookiePath;
    }

    function getCookieValue(name) {
        const value = `; ${document.cookie}`;
        const parts = value.split(`; ${name}=`);
        if (parts.length === 2) return parts.pop().split(';').shift();
        return null;
    }

    const adParams = ['fbclid', 'gclid'];
    const urlParams = new URLSearchParams(window.location.search);
    let isAdClick = false;
    adParams.forEach(param => {
        if (urlParams.has(param)) {
            isAdClick = true;
        }
    });

    if (isAdClick) {
        const utmSource = getParameterByName('utm_source');
        const utmMedium = getParameterByName('utm_medium');
        const utmCampaign = getParameterByName('utm_campaign');
        const utmContent = getParameterByName('utm_content');
        const utmTerm = getParameterByName('utm_term');

        if (utmSource) setCookie('cookieUtmSource', utmSource, 63072000);
        if (utmMedium) setCookie('cookieUtmMedium', utmMedium, 63072000);
        if (utmCampaign) setCookie('cookieUtmCampaign', utmCampaign, 63072000);
        if (utmContent) setCookie('cookieUtmContent', utmContent, 63072000);
        if (utmTerm) setCookie('cookieUtmTerm', utmTerm, 63072000);
    }

    let parametros = ["utm_source", "utm_medium", "utm_campaign", "utm_content", "utm_term"];
    const urlParamsCapt = new URLSearchParams(window.location.search);
    const urlParamsCaptReferrer = new URLSearchParams(document.referrer.split('?')[1] || '');
    let utms = {};

    const cookieUtmSource = getCookieValue('cookieUtmSource');
    const cookieUtmMedium = getCookieValue('cookieUtmMedium');
    const cookieUtmCampaign = getCookieValue('cookieUtmCampaign');
    const cookieUtmContent = getCookieValue('cookieUtmContent');
    const cookieUtmTerm = getCookieValue('cookieUtmTerm');

    parametros.forEach(el => {
        if (el === "utm_source") {
            utms[el] = urlParamsCapt.get(el) ?? (document.referrer ? (urlParamsCaptReferrer.get(el) ?? new URL(document.referrer).hostname) : "direto");
        } else {
            utms[el] = urlParamsCapt.get(el) ?? (urlParamsCaptReferrer.get(el) ?? "");
        }
    });

    let scks = Object.values(utms).filter(value => value !== "");
    let currentSckValues = [];
    if (urlParamsCapt.get('sck')) {
        currentSckValues = urlParamsCapt.get('sck').split('|');
    }
    scks = scks.filter(value => !currentSckValues.includes(value));

    let srcValues = [cookieUtmSource, cookieUtmMedium, cookieUtmCampaign, cookieUtmContent, cookieUtmTerm].filter(value => value !== null && value !== "");

    const updateLinks = (el, elURL) => {
        const elSearchParams = new URLSearchParams(elURL.search);
        let modified = false;

        urlParams.forEach((value, key) => {
            if (!elSearchParams.has(key)) {
                elSearchParams.append(key, value);
                modified = true;
            }
        });

        for (let key in utms) {
            if (!elSearchParams.has(key)) {
                elSearchParams.append(key, utms[key]);
                modified = true;
            }
        }

        if (cookieUtmSource) elSearchParams.append('cookieUtmSource', cookieUtmSource);
        if (cookieUtmMedium) elSearchParams.append('cookieUtmMedium', cookieUtmMedium);
        if (cookieUtmCampaign) elSearchParams.append('cookieUtmCampaign', cookieUtmCampaign);
        if (cookieUtmContent) elSearchParams.append('cookieUtmContent', cookieUtmContent);
        if (cookieUtmTerm) elSearchParams.append('cookieUtmTerm', cookieUtmTerm);

        if (!elSearchParams.has('sck') && scks.length > 0) {
            elSearchParams.append('sck', scks.join('|'));
            modified = true;
        }

        if (!elSearchParams.has('src') && srcValues.length > 0) {
            elSearchParams.append('src', srcValues.join('|'));
            modified = true;
        }

        if (modified) {
            return elURL.origin + elURL.pathname + "?" + elSearchParams.toString();
        }
        return el.href;
    };

    document.querySelectorAll('a').forEach(el => {
        try {
            const elURL = new URL(el.href, window.location.origin);
            if (!elURL.hash) {
                el.href = updateLinks(el, elURL);
            }
        } catch (e) {
            console.warn('Erro ao processar URL no link:', el.href);
        }
    });

    document.querySelectorAll('iframe').forEach(iframe => {
        let actualSrc = iframe.hasAttribute('data-src') ? iframe.getAttribute('data-src') : iframe.src;
        if (actualSrc) {
            try {
                const iframeURL = new URL(actualSrc, window.location.origin);
                if (iframe.hasAttribute('data-src')) {
                    iframe.setAttribute('data-src', updateLinks(iframe, iframeURL));
                } else {
                    iframe.src = updateLinks(iframe, iframeURL);
                }
            } catch (e) {
                console.warn('Erro ao processar URL no iframe:', actualSrc);
            }
        }
    });
})();
</script>
```

## Passo 4: Atualizar Links de Pagamento

Procure por `https://pay.hotmart.com/Q106074371A` e atualize com os links corretos, se necessário.

## Passo 5: Commit e Push

```bash
git add nome-da-pagina/
git commit -m "adiciona página: nome-da-pagina"
git push
```

## Checklist Antes de Fazer Push

- [ ] Arquivo `index.html` existe na pasta
- [ ] Pasta `images/` existe com as imagens necessárias
- [ ] Script de rastreio está presente (procure por "Script de Rastreio de UTMs")
- [ ] Links de pagamento (Hotmart) estão corretos
- [ ] Title da página foi atualizado
- [ ] Conteúdo foi customizado

## URLs Geradas Automaticamente

Quando você fizer push, o GitHub Pages cria a URL automaticamente:
- `nome-da-pagina/index.html` → `vsl.amindasmilhas.com.br/nome-da-pagina`

Pronto! 🚀
