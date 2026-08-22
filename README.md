# vminvest.nl

Statische one-pager voor VM Investments. Geen build-stap, geen framework: één
`index.html` met alle CSS en JS erin, plus een `assets/` map. Openen in de
browser werkt direct, ook lokaal.

## Inhoud

```
index.html      de hele site (HTML + CSS + JS in één bestand)
assets/
  vm-logo.svg            wordmark navy (vector, uit VMLOGO.pdf)
  vm-logo-white.svg      wordmark wit, voor de footer
  vm-monogram.svg        alleen de VM, voor de header
  vm-monogram-white.svg  alleen de VM in wit, voor de tegel "Commercieel"
  favicon*.png/.ico      tabblad-iconen
  apple-touch-icon.png   icoon als iemand de site op zijn homescreen zet
  residentieel.jpg       foto residentieel vastgoed
  studenten.jpg          foto studentenwoningen
  hotels.jpg             foto hotels
  logistiek.jpg          foto logistiek
  dick-van-manen.jpg     portret
  fonts/                 Inter + Cormorant Garamond (woff2, lokaal geserveerd)
CNAME           vertelt GitHub Pages dat de site op vminvest.nl draait
.nojekyll       zet Jekyll uit; GitHub serveert de bestanden ongewijzigd
robots.txt      zoekmachines
sitemap.xml     zoekmachines
```

## Openen in PyCharm

1. **File → Open** en kies deze map.
2. Rechtsklik op `index.html` → **Open in Browser** (of het browser-icoontje
   rechtsboven in de editor). PyCharm start een lokale server, dus paden naar
   `assets/` kloppen meteen.

## Wat je wilt aanpassen

**Tekst.** Alles staat gewoon in `index.html`. Zoek op de Nederlandse zin en
pas hem aan.

**Vertaling.** De site is tweetalig zonder tweede bestand: elk element met een
`data-en="..."` attribuut houdt de Nederlandse tekst in de HTML en de Engelse in
het attribuut. De taalknop wisselt ze om. Nieuwe zin vertalen = `data-en="..."`
op dat element zetten, verder niets. Twee dingen om op te letten:

- Zet nooit een `data-en` element binnen een ander `data-en` element — de
  buitenste overschrijft de binnenste bij het wisselen.
- Gebruik in het attribuut `&amp;` in plaats van `&`, en geen dubbele
  aanhalingstekens (`&quot;` mag wel).

Bezoekers met een niet-Nederlandse browsertaal krijgen automatisch Engels;
`vminvest.nl/?lang=en` linkt rechtstreeks naar de Engelse versie.

**Foto's.** Vervang een bestand in `assets/` en houd de naam gelijk. De foto's
uit de oude site zijn klein (343×343 px); als er scherpere originelen zijn,
zien de kaarten er beter uit. Twee plekken wachten nog op beeld:

- *Commercieel vastgoed* heeft nu een navy tegel met het monogram. Zet een foto
  neer als `assets/commercieel.jpg` en vervang in `index.html` het blok
  `<div class="card-media placeholder">…</div>` door
  `<div class="card-media"><img src="assets/commercieel.jpg" alt="Commercieel vastgoed" loading="lazy"/></div>`.
- Onder de hero staat nu een smalle navy balk. Wil je daar een brede foto
  (minimaal 1800 px breed), zet die neer als `assets/hero.jpg` en haal in
  `index.html` de opmerkingstekens rond het `<div class="band">` blok weg.

**Het logo** is vector (SVG), dus scherp op elk formaat en op elk scherm. De
kleur zit als gewone `fill` in het bestand: open `assets/vm-logo.svg` in
PyCharm en vervang `#1a2e4f` (de VM) of `#4a5c7a` (het woord INVESTMENTS) door
een andere waarde. Het originele `VMLOGO.pdf` is grijs; voor de site is die
grijstint vervangen door het navy van de oude Wix-site.

**Kleuren en typografie.** Bovenin de `<style>` staat een blok `:root` met alle
kleuren, maten en lettertypen. Daar één waarde aanpassen verandert de hele site.

## Naar GitHub

In de terminal van PyCharm, vanuit deze map:

```bash
git init
git add .
git commit -m "VM Investments website"
git branch -M main
git remote add origin https://github.com/<jouw-account>/vminvest.git
git push -u origin main
```

Maak de repository eerst leeg aan op github.com (zonder README, anders botst de
eerste push).

## GitHub Pages aanzetten

Repository → **Settings** → **Pages**:

- **Source:** Deploy from a branch
- **Branch:** `main`, map `/ (root)` → **Save**

Na een minuut staat de site op `https://<jouw-account>.github.io/vminvest/`.

> **Let op bij het testen.** Zolang het bestand `CNAME` in de repo staat, zet
> GitHub `vminvest.nl` als custom domain en stuurt het de `github.io`-URL
> daarheen door. Dat werkt pas nadat de DNS klaar is. Wil je eerst op de
> `github.io`-URL kijken, hernoem `CNAME` dan tijdelijk naar `CNAME.txt` en zet
> hem terug zodra de DNS staat.

## Hoe de DNS er nu bij staat (gemeten augustus 2026)

```
vminvest.nl.        NS     ns4.wixdns.net., ns5.wixdns.net.
vminvest.nl.        A      185.230.63.107, .171, .186     <- Wix
www.vminvest.nl.    CNAME  cdn3.wixdns.net.               <- Wix
vminvest.nl.        MX     0 vminvest-nl.mail.protection.outlook.com.
```

Twee dingen springen eruit:

1. **De DNS-zone wordt door Wix beheerd**, niet door de registrar. De
   nameservers staan op `wixdns.net`, dus records wijzig je in het
   Wix-dashboard — tenzij je de nameservers eerst terugzet naar je registrar.
2. **De e-mail van vminvest.nl loopt via Microsoft 365** (dat MX-record). Dat
   record staat in dezelfde zone die door Wix wordt beheerd. Raakt die zone
   weg, dan stopt de e-mail op `info@vminvest.nl`. Neem het MX-record dus over
   voordat je iets verplaatst.

Er staan verder geen SPF-, DKIM- of DMARC-records op het domein. Los van deze
verhuizing is dat een zwakke plek: zonder SPF/DMARC kan iedereen mail namens
vminvest.nl versturen, en komt jullie eigen mail sneller in de spamfilter.
Goed moment om dat er meteen bij te zetten.

### Route A — DNS bij Wix laten staan (minste risico)

Log in op het Wix-dashboard, ga naar het domein en de DNS-instellingen, en:

- vervang de drie A-records op `@` door de vier GitHub A-records hieronder;
- wijzig het CNAME op `www` van `cdn3.wixdns.net` naar `<jouw-account>.github.io`;
- **laat het MX-record ongemoeid.**

Het abonnement voor de wébsite kan weg; je houdt alleen het domein bij Wix.

### Route B — nameservers terug naar de registrar

Zet de nameservers om naar die van je registrar en bouw de zone daar opnieuw op:
de GitHub-records hieronder **plus** met de hand het MX-record
`0 vminvest-nl.mail.protection.outlook.com`. Vergeet dat laatste niet — zonder
dat record komt er geen e-mail meer binnen.

### De records voor GitHub Pages

| Type  | Naam  | Waarde |
|-------|-------|--------|
| A     | `@`   | `185.199.108.153` |
| A     | `@`   | `185.199.109.153` |
| A     | `@`   | `185.199.110.153` |
| A     | `@`   | `185.199.111.153` |
| AAAA  | `@`   | `2606:50c0:8000::153` |
| AAAA  | `@`   | `2606:50c0:8001::153` |
| AAAA  | `@`   | `2606:50c0:8002::153` |
| AAAA  | `@`   | `2606:50c0:8003::153` |
| CNAME | `www` | `<jouw-account>.github.io` |

De AAAA-records (IPv6) zijn optioneel maar netjes. Let op dat de CNAME naar
`<jouw-account>.github.io` wijst, dus **zonder** `/vminvest` erachter.

Daarna in **Settings → Pages → Custom domain** `vminvest.nl` invullen. GitHub
doet dan een DNS-controle; zodra die groen is, **Enforce HTTPS** aanzetten. Het
certificaat komt automatisch van GitHub en dat kan tot een uur duren.

Controleren waar het domein op dat moment naartoe wijst kan zonder extra
gereedschap, vanuit de terminal in PyCharm:

```bash
python3 -c "import socket; print(socket.gethostbyname('vminvest.nl'))"
```

Staat daar `185.199.1xx.153`, dan is de DNS doorgekomen. Reken op een half uur
tot een paar uur; soms langer als de oude waarden nog in caches zitten.

## Bijwerken

Tekst aanpassen, opslaan, dan:

```bash
git add .
git commit -m "tekst bijgewerkt"
git push
```

Binnen een minuut staat het live.

## Nog te doen / af te wegen

- E-mailadres en telefoonnummer staan zichtbaar op de pagina. Het e-mailadres
  wordt pas in de browser samengesteld (`assets` van spam-scrapers), maar
  volledig waterdicht is dat niet.
- Er zit bewust geen contactformulier in: dat vraagt om een backend. Wil je er
  later toch een, dan kan dat met een dienst als Formspree zonder de site
  ergens anders te hoeven hosten.
- Er staat nog geen KvK-nummer op de site. Voor een Nederlandse onderneming is
  dat op een zakelijke website gebruikelijk; overweeg het bij de contactgegevens
  te zetten.
