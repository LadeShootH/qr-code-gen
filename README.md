# QR- & Barcode-Generator

Eine einzelne, statische HTML-Seite zum Erzeugen von QR-Codes und Barcodes direkt im Browser (kein Server, kein Build-Schritt, keine Datenübertragung). Gedacht zum Hosting unter **qr.fless.me** via GitHub + Cloudflare Pages.

## Lokal ansehen

Einfach `index.html` im Browser öffnen, oder z. B.:

```bash
npx serve .
```

## Deployment: GitHub → Cloudflare Pages → qr.fless.me

### 1. Repository auf GitHub anlegen

Falls du die [GitHub CLI](https://cli.github.com/) installiert hast:

```bash
cd qr-barcode-site
git init
git add .
git commit -m "Initial commit: QR- & Barcode-Generator"
gh repo create qr-barcode-generator --public --source=. --remote=origin --push
```

Ohne `gh` CLI: Repo manuell auf github.com/new anlegen (z. B. `qr-barcode-generator`), dann:

```bash
cd qr-barcode-site
git init
git add .
git commit -m "Initial commit: QR- & Barcode-Generator"
git branch -M main
git remote add origin https://github.com/<dein-username>/qr-barcode-generator.git
git push -u origin main
```

### 2. Cloudflare Pages Projekt erstellen

1. Cloudflare-Dashboard → **Workers & Pages** → **Create application** → Tab **Pages** → **Connect to Git**.
2. Das eben erstellte GitHub-Repo auswählen und autorisieren.
3. Build-Einstellungen:
   - **Framework preset:** `None`
   - **Build command:** *(leer lassen)*
   - **Build output directory:** `/` (bzw. leer lassen – Root)
4. **Save and Deploy**. Nach kurzer Zeit ist die Seite unter `https://<projektname>.pages.dev` erreichbar.

### 3. Custom Domain `qr.fless.me` verbinden

Deine Domain `fless.me` läuft bereits über Cloudflare-Nameserver – Cloudflare kann den DNS-Eintrag daher automatisch für dich anlegen.

1. Im Pages-Projekt: **Custom domains** → **Set up a domain**.
2. `qr.fless.me` eingeben → **Continue** → **Activate domain**.
3. Cloudflare legt automatisch einen CNAME-Eintrag (`qr` → `<projektname>.pages.dev`, proxied) in der DNS-Zone von `fless.me` an. Kein manueller Schritt im DNS-Tab nötig.
4. Nach wenigen Minuten (SSL-Zertifikat wird automatisch ausgestellt) ist die Seite unter **https://qr.fless.me** erreichbar.

> Wichtig: Den Custom-Domain-Dialog in Pages nutzen (Schritt 1–2), **nicht** manuell im DNS-Tab einen CNAME anlegen — sonst zeigt der Eintrag ohne die Pages-Zuordnung einen 522-Fehler.

### 4. Updates ausliefern

Jeder Push auf den `main`-Branch löst automatisch ein neues Cloudflare-Pages-Deployment aus. Für Änderungen an der Seite also einfach:

```bash
git add .
git commit -m "Update"
git push
```

## Projektstruktur

```
qr-barcode-site/
├── index.html   # komplette Anwendung (HTML/CSS/JS in einer Datei)
└── README.md
```

## Technik

- QR-Codes: [qrcodejs](https://github.com/davidshimjs/qrcodejs) (CDN: cdnjs)
- Barcodes: [JsBarcode](https://github.com/lindell/JsBarcode) (CDN: cdnjs)
- Kein Build-Prozess, kein Backend — alles läuft clientseitig im Browser.
