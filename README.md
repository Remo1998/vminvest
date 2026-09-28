# vminvest.nl

Statische website van VMINVEST, gehost op GitHub Pages onder **https://vminvest.nl**.
Geen build-stap, geen framework, geen dependencies: HTML, één CSS-bestand en één klein script.

## Structuur

```
index.html        stuurt door naar nl/ (en vangt oude links op, zie hieronder)
404.html          foutpagina van GitHub Pages
nl/  en/          per taal 12 pagina's met dezelfde bestandsnamen
                  (index, contact, our-approach, international-network, 8 expertisepagina's)
assets/
  site.css        alle vormgeving; latere regels gaan bewust vóór eerdere
  site.js         mobiel menu en Escape-toets
  fonts/          Michroma (SIL OFL 1.1, licentie in OFL-Michroma.txt)
  vm-monogram-white.svg   het VM-logo in header en footer
  vm-logo*.svg, vm-monogram.svg   overige logovarianten (vector, uit VMLOGO.pdf)
  architecture.jpg, background-*.jpg   beeld
  favicon*, apple-touch-icon.png       tabblad-iconen
CNAME             koppelt de site aan vminvest.nl — niet verwijderen
.nojekyll         GitHub serveert de bestanden ongewijzigd
robots.txt, sitemap.xml
```

## Lokaal bekijken in PyCharm

Rechtsklik `nl/index.html` → **Open in Browser**. Of in de terminal:

```bash
python -m http.server 8000
```

en open http://localhost:8000/.

## Aanpassen

- **Tekst**: in de HTML van `nl/` en `en/`. Header, menu en footer staan op elke
  pagina los; pas ze bij een wijziging in alle 24 bestanden aan (PyCharm:
  *Edit → Find → Replace in Files*).
- **Taalwissel**: elke pagina linkt naar dezelfde bestandsnaam in de andere taal.
  Nieuwe pagina? Maak hem in beide mappen met dezelfde naam, en voeg hem toe aan
  `sitemap.xml`.
- **Vormgeving**: `assets/site.css`. Het bestand is in stappen verfijnd; latere
  regels overschrijven eerdere. Zet een wijziging dus onderaan.
- **Head van elke pagina**: bevat `canonical` en `hreflang`-links met de volledige
  URL. Houd die kloppend als je een pagina hernoemt.

## Oude links

De vorige site was één pagina. `index.html` stuurt oude links door:

| Oud | Nieuw |
|---|---|
| `vminvest.nl/?lang=en` | `/en/` |
| `vminvest.nl/#contact` | `/nl/contact.html` |
| `vminvest.nl/#logistiek` | `/nl/logistics-industrial.html` |
| `vminvest.nl/#projecten` | `/nl/#expertise` |
| `vminvest.nl/#over-ons` | `/nl/` |

## Publiceren

```bash
git add -A
git commit -m "beschrijving"
git push
```

Binnen een minuut live. De vorige versie van de site staat in de git-historie.

## Domein en DNS

- Registrar en DNS: Antagonist (nameservers `ns1/ns2/ns3.webhostingserver.nl`),
  records in DirectAdmin → Accountbeheer → DNS Beheer.
- `vminvest.nl` A → `185.199.108.153`, `.109.153`, `.110.153`, `.111.153`;
  AAAA → `2606:50c0:8000::153` t/m `8003::153`.
- `www` CNAME → `remo1998.github.io.`
- E-mail loopt via Microsoft 365 (MX, autodiscover, SPF, DMARC en de overige
  Microsoft-records in de zone). **Niet wijzigen** bij aanpassingen aan de website.
- GitHub → Settings → Pages: custom domain `vminvest.nl`, Enforce HTTPS aan.

## Beeld en rechten

- `architecture.jpg`: Simon Clotour, Unsplash ([licentie](https://unsplash.com/license)).
- `background-*.jpg`: AI-gegenereerde sfeerbeelden, geen echte VMINVEST-projecten.
- Michroma: SIL Open Font License 1.1.
- VM-logo: eigen logo van VMINVEST (bron: VMLOGO.pdf).
