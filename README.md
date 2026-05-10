# Chart Scanner AI — strona z dokumentami prawnymi

Statyczne pliki HTML do hostowania na **GitHub Pages**. Zawiera politykę prywatności
i regulamin wymagane przez Google Play / App Store.

## Pliki

```
legal-site/
├── index.html      ← strona główna z linkami
├── privacy.html    ← polityka prywatności (RODO + UGC + AI)
├── terms.html      ← regulamin (UGC + moderacja 24h + ostrzeżenia AI)
├── style.css       ← współdzielony arkusz stylów
└── README.md       ← ten plik
```

## Hosting na GitHub Pages — krok po kroku

### 1. Utwórz nowe publiczne repo na GitHubie

Nazwa sugerowana: `skanerbiznes-legal` (krótka, łatwa do wpisania).

```powershell
# Z poziomu projektu, w PowerShell:
cd ..
mkdir skanerbiznes-legal
Copy-Item -Recurse SkanerBiznesNative\legal-site\* skanerbiznes-legal\
cd skanerbiznes-legal
git init
git add .
git commit -m "Initial legal documents"
```

Następnie utwórz repo na github.com (https://github.com/new) — **Public**, bez README,
i sparuj z lokalnym:

```powershell
git branch -M main
git remote add origin https://github.com/TWOJ_USERNAME/skanerbiznes-legal.git
git push -u origin main
```

### 2. Włącz GitHub Pages

1. Wejdź w repo na github.com → zakładka **Settings**
2. W menu po lewej kliknij **Pages**
3. Source: **Deploy from a branch**
4. Branch: **main** / folder: **/ (root)** → **Save**
5. Po ~1 minucie strona będzie pod adresem:
   `https://TWOJ_USERNAME.github.io/skanerbiznes-legal/`

### 3. Zaktualizuj URL-e w aplikacji

Plik `constants/legalUrls.ts` w głównym projekcie SkanerBiznesNative ma placeholder
`GITHUB_USERNAME`. Podmień na rzeczywisty username, np.:

```ts
export const LEGAL_URLS = {
  privacy: 'https://mikolajsitek.github.io/skanerbiznes-legal/privacy.html',
  terms:   'https://mikolajsitek.github.io/skanerbiznes-legal/terms.html',
  home:    'https://mikolajsitek.github.io/skanerbiznes-legal/',
};
```

### 4. Wpisz URL-e w Google Play Console

Po założeniu konta Play Developer:

- **Store listing → Privacy Policy URL:** `https://TWOJ_USERNAME.github.io/skanerbiznes-legal/privacy.html`
- **App content → Data Safety form:** wskaż ten sam URL prywatności
- **App content → Target Audience:** zaznacz wiek 16+ (zgodnie z polityką)

## Aktualizacja treści

Treść Polityki / Regulaminu w aplikacji (`app/privacy.tsx`, `app/terms.tsx`) jest
ZSYNCHRONIZOWANA z plikami HTML — przy zmianie jednej, zaktualizuj drugą.

Jeśli zmieniasz tylko wersję online (np. literówki, drobne korekty):

```powershell
# w repo skanerbiznes-legal
git pull
# edytuj privacy.html / terms.html
git add .
git commit -m "Update privacy: ..."
git push
```

GitHub Pages odświeża się automatycznie w ciągu ~1 minuty.

## Custom domain (opcjonalnie, na później)

Jeśli kupisz domenę (np. `skanerbiznes.pl`):

1. W repo Settings → Pages → Custom domain wpisz `legal.skanerbiznes.pl`
2. U dostawcy domeny dodaj rekord CNAME `legal` → `TWOJ_USERNAME.github.io`
3. Po propagacji DNS (kilka godzin) URL-e zmieniają się na `https://legal.skanerbiznes.pl/privacy.html`
4. Zaktualizuj `constants/legalUrls.ts` i Play Console

## Kontakt e-mail

W obu HTML-ach widnieje `kontakt@mjgweb.pl` jako adres do zgłoszeń UGC i pytań
RODO. Zgodnie z polityką moderacji deklarujemy odpowiedź w ciągu 24h w dni
robocze — pamiętaj o sprawdzaniu skrzynki.
