---
layout: ~/layouts/MainLayout.astro
title: Seiten
description: Eine Einführung in die Astro-Seiten
---

**Seiten** sind ein spezieller Typ von [Astro-Komponenten](/de/core-concepts/astro-components/), die sich im Unterverzeichnis `src/pages/` befinden. Sie sind verantwortlich für das Routing, das Laden von Daten und das gesamte Seitenlayout für jede HTML-Seite in deiner Website.

### Dateibasiertes Routing

Astro nutzt eine Routing-Strategie, die **dateibasiertes Routing** genannt wird. Jede `.astro`-Datei im `src/pages`-Verzeichnis wird zu einer Seite oder einem Endpunkt auf deiner Website, basierend auf ihrem Dateipfad.

📚 Lies mehr über [Routing in Astro](/de/core-concepts/routing/)

### Seiten-HTML

Astro-Seiten müssen eine vollständige `<html>...</html>`-Seitenantwort zurückgeben, einschließlich `<head>` und `<body>`. (`<!doctype html>` ist optional und wird automatisch hinzugefügt.)

```astro
---
// Beispiel: src/pages/index.astro
---
<html>
  <head>
    <title>Meine Homepage</title>
  </head>
  <body>
    <h1>Willkommen auf meiner Website!</h1>
  </body>
</html>
```

### Nutzung von Seitenlayouts

Um zu vermeiden, dass sich dieselben HTML-Elemente auf jeder Seite wiederholen, kannst du gemeinsame `<head>`- und `<body>`-Elemente in ihre eigenen [Layout-Komponenten](/de/core-concepts/layouts/) verschieben. Du kannst so viele oder so wenige Layout-Komponenten verwenden, wie du möchtest.

```astro
---
// Beispiel: src/pages/index.astro
import MeinLayout from '../layouts/MeinLayout.astro';
---
<MeinLayout>
  <p>Mein Seiteninhalt, umgeben von einem Layout!</p>
</MeinLayout>
```

📚 Lies mehr über [Layout-Komponenten](/de/core-concepts/layouts/) in Astro.


## Markdown-Seiten

Astro behandelt auch alle Markdown-Dateien (`.md`) innerhalb von `/src/pages/` als Seiten in deiner finalen Website. Diese werden üblicherweise für textlastige Seiten wie Blogbeiträge und Dokumentationen verwendet.

Seitenlayouts sind besonders nützlich für [Markdown-Dateien](#markdown-seiten). Markdown-Dateien können die spezielle Frontmatter-Eigenschaft `layout` verwenden, um eine [Layout-Komponente](/de/core-concepts/layouts/) zu spezifizieren, welche den Markdown-Inhalt in ein vollständiges `<html>...</html>`-Dokument einbettet.

```md
---
# Beispiel: src/pages/page.md
layout: '../layouts/MeinLayout.astro'
title: 'Meine Markdown-Seite'
---
# Titel

Das hier ist meine Seite, geschrieben in **Markdown.**
```

📚 Lies mehr über [Markdown](/de/guides/markdown-content/) in Astro.


## Nicht-HTML-Seiten

Nicht-HTML-Seiten, z. B. `.json`, `.xml` oder sogar Bilder, können über API-Routen erstellt werden, die gemeinhin als **Dateirouten** bezeichnet werden.

**Dateirouten** sind Skriptdateien, die mit der Erweiterung `.js` oder `.ts` enden und sich im Verzeichnis `src/pages/` befinden.

Erstellte Dateinamen und Erweiterungen basieren auf den Namen der Quelldateien. Die Datei `src/pages/data.json.ts` wird z. B. so erstellt, dass sie der `/data.json`-Route in deinem endgültigen Build entspricht.

Bei SSR (server-side rendering) spielt die Erweiterung keine Rolle und kann weggelassen werden, da zum Zeitpunkt der Erstellung keine Dateien erzeugt werden. Stattdessen erzeugt Astro eine einzige Serverdatei.

```js
// Beispiel: src/pages/builtwith.json.ts
// Ausgabe: /builtwith.json

// Dateirouten exportieren eine get()-Funktion, die aufgerufen wird, um die Datei zu erzeugen.
// Gib ein Objekt mit `body` zurück, um den Inhalt der Datei in deinem endgültigen Build zu speichern.
export async function get() {
  return {
    body: JSON.stringify({
      name: 'Astro',
      url: 'https://astro.build/',
    }),
  };
}
```

API-Routen erhalten ein `APIContext`-Objekt, das [Parameter (params)](/de/reference/api-reference/#params) und eine [Anfrage (request)](https://developer.mozilla.org/en-US/docs/Web/API/Request) enthält:

```ts
import type { APIContext } from 'astro';

export async function get({ params, request }: APIContext) {
  return {
    body: JSON.stringify({
      path: new URL(request.url).pathname
    })
  };
}
```

Optional kannst du deine API-Routenfunktionen auch unter Verwendung des Typs `APIRoute` eingeben. Dadurch erhältst du bessere Fehlermeldungen, wenn deine API-Route den falschen Typ zurückgibt:

```ts
import type { APIRoute } from 'astro';

export const get: APIRoute = ({ params, request }) => {
  return {
    body: JSON.stringify({
      path: new URL(request.url).pathname
    })
  };
};
```

## Benutzerdefinierte 404-Fehlerseite

Für eine benutzerdefinierte 404-Fehlerseite kannst du eine Datei namens `404.astro` oder `404.md` in `/src/pages` erstellen.

Aus dieser wird die Seite `404.html` erstellt. Die meisten [Hosting-Anbieter](/de/guides/deploy/) werden sie finden und verwenden.

