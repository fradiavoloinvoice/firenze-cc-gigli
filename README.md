# Fradiavolo Firenze — I Gigli · Landing 20% di sconto

Landing page per il punto vendita Fradiavolo al **primo piano del Centro Commerciale I Gigli**
(Via San Quirico 165, Campi Bisenzio – FI).

- Offerta **20% di sconto** in alto con form di registrazione (nome, cognome, email)
- Dopo l'invio mostra il **codice sconto** (`FRNHpg`) da esibire alla cassa
- Le lead vengono inviate a un **webhook Make** (nessun backend da gestire)
- Sezioni: racconto Fradiavolo (tradizionali/diavole/pop, 5 impasti, paste/insalate/secondi/fritti),
  riconoscimento **50 Top Pizza**, info punto vendita + mappa

Pagina **100% statica** (un solo file `index.html`, immagini incorporate in base64).
Design replicato dalla landing di Roma: colori Fradiavolo, font Typekit `roc-grotesk`.

---

## Configurazione webhook Make

1. In **Make** crea uno scenario con trigger **Webhooks → Custom webhook**. Copia l'URL
   (es. `https://hook.eu2.make.com/xxxxxxxxxxxxxxxx`).
2. Apri `index.html` e incolla l'URL nella costante in cima allo `<script>`:

   ```js
   var WEBHOOK_URL = 'https://hook.eu2.make.com/xxxxxxxxxxxxxxxx';
   ```

3. Il form invia un **POST** con `Content-Type: text/plain` (così non scatta il preflight CORS)
   e questo JSON nel body:

   ```json
   {
     "nome": "Mario",
     "cognome": "Rossi",
     "email": "mario@example.com",
     "codice": "FRNHpg",
     "fonte": "Firenze I Gigli",
     "timestamp": "2026-06-16T12:34:56.000Z"
   }
   ```

   In Make, dopo il primo invio di test, fai **"Determine data structure"** sul webhook per
   mappare i campi, poi collega il modulo che preferisci (Google Sheets, email, CRM…).

> Se `WEBHOOK_URL` non è configurato, il form mostra comunque il codice all'utente
> (per non perdere il cliente) ma **non** invia nulla.

---

## Deploy

Essendo statica, va bene qualsiasi hosting. Esempi:

```bash
# Cloudflare Pages
npx wrangler pages deploy . --project-name=firenze-live-experience

# oppure collega il repo GitHub da Cloudflare/Vercel/Netlify
# build command: vuoto · output directory: /
```

Test locale: apri direttamente `index.html` nel browser, oppure `npx serve .`.

---

## Note

- `noindex,nofollow`: pagina promozionale non destinata all'indicizzazione.
- Immagini ottimizzate (lato max ~1100px, JPEG) e incorporate in base64. `index.html` ≈ 1,5 MB.
- Codice sconto attuale: **fisso** `FRNHpg`. Per codici univoci/monouso serve logica aggiuntiva.
- Riconoscimento mostrato: logo **50 Top Pizza** (50 Top World Artisan Pizza Chains 2025, #22).
