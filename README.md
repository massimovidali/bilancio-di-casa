# Bilancio di Casa

Web app per registrare entrate e uscite familiari, pensata per iPhone.
Dati su **Firebase** (Firestore + accesso con email/password), pagina pubblicata con **GitHub Pages**.

Tutto rientra nei piani gratuiti: Firebase "Spark" (50.000 letture e 20.000 scritture al giorno) e GitHub Pages su repository pubblico.

## File

| File | A cosa serve |
|---|---|
| `index.html` | L'app completa |
| `firebase-config.js` | I dati del tuo progetto Firebase (da compilare) |
| `firestore.rules` | Modello delle regole di sicurezza: chi può leggere e scrivere |
| `manifest.webmanifest`, `icon-*.png` | Nome e icona quando la aggiungi alla Home dell'iPhone |
| `.nojekyll` | Dice a GitHub Pages di pubblicare i file così come sono |

---

## 1. Crea il progetto Firebase

1. Vai su <https://console.firebase.google.com> ed entra con un account Google.
2. **Crea un progetto** → nome `bilancio-di-casa` → Google Analytics puoi disattivarlo → **Crea progetto**.

### Accesso con email e password
3. Menu a sinistra **Build → Authentication → Inizia**.
4. Scheda **Metodo di accesso** → **Email/password** → attiva il primo interruttore → **Salva**.
5. Scheda **Utenti** → **Aggiungi utente**: crea un account per te e uno per ogni familiare (email + password).
6. Consigliato: scheda **Impostazioni → Azioni utente** → togli la spunta a **Abilita creazione (registrazione)**. Così nessuno può crearsi un account da solo.

### Database
7. **Build → Firestore Database → Crea database**.
8. Posizione: **eur3 (Europe)** → modalità **produzione** → **Crea**.
9. Scheda **Regole**: incolla il contenuto di `firestore.rules`, **sostituisci le email di esempio con quelle vere** degli account del punto 5, poi **Pubblica**.

### Collega l'app
10. In alto a sinistra **⚙ → Impostazioni progetto** → in fondo, **Le tue app** → icona **Web `</>`**.
11. Nome `Bilancio`, **non** spuntare Firebase Hosting → **Registra app**.
12. Copia i valori di `firebaseConfig` (apiKey, authDomain, projectId…) dentro `firebase-config.js`, al posto delle scritte `INCOLLA_QUI`.

> I valori di `firebase-config.js` non sono segreti: possono stare su GitHub. A proteggere i dati sono le regole del punto 9.

## 2. Prova sul computer

Dalla cartella del progetto:

```bash
python -m http.server 8000
```

Apri <http://localhost:8000>, accedi con uno degli account creati e inserisci un movimento di prova. Lo vedi comparire in Firestore, nella raccolta `movimenti`.

## 3. Pubblica su GitHub Pages

1. Su <https://github.com/new> crea un repository chiamato `bilancio-di-casa`, **Public** (GitHub Pages gratuito richiede un repository pubblico), senza README.
2. Dalla cartella del progetto, sostituendo `TUO-UTENTE`:

```bash
git remote add origin https://github.com/TUO-UTENTE/bilancio-di-casa.git
```

```bash
git push -u origin main
```

3. Sul repository: **Settings → Pages** → *Source*: **Deploy from a branch** → branch **main**, cartella **/ (root)** → **Save**.
4. Dopo un minuto l'app è online su `https://TUO-UTENTE.github.io/bilancio-di-casa/`.
5. In Firebase: **Authentication → Impostazioni → Domini autorizzati** → **Aggiungi dominio** → `TUO-UTENTE.github.io`.

Per pubblicare una modifica: `git add -A`, `git commit -m "descrizione"`, `git push`. GitHub Pages si aggiorna da solo.

## 4. Sull'iPhone

1. Apri l'indirizzo GitHub Pages in **Safari**.
2. Accedi con email e password.
3. **Condividi → Aggiungi alla schermata Home**.

L'app si apre a tutto schermo con la sua icona. Ricorda l'accesso e funziona anche senza rete: i movimenti inseriti offline si sincronizzano quando torna la connessione.

## Come sono salvati i dati

- `movimenti`: un documento per ogni entrata o uscita (`tipo`, `importo`, `categoria`, `nota`, `data`, `anno`, `mese`).
- `ricorrenti`: le voci fisse mensili (`giorno`, `dal`, `al` vuoto se senza scadenza). Non vengono copiate in ogni mese: l'app le calcola al volo.
