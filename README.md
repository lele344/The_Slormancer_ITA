THE SLORMANCER — TRADUZIONE ITA (YYToolkit IO Redirect)

Descrizione
Mod/DLL per abilitare l’italiano in "The Slormancer" senza toccare la logica di gioco. Reindirizza i file di testo verso versioni *_it*.json e promuove automaticamente i campi IT → EN (solo: EN, EN_NAME, EN_DESCRIPTION, EN_DIAL). Funziona sia con JSON normale sia con JSON in Base64. I file originali restano intatti: la mod genera copie in cache.

Caratteristiche
- Redirect dei path di I/O verso mods\Aurie\traduzioni\*_it*.json
- Promozione IT→EN su: EN, EN_NAME, EN_DESCRIPTION, EN_DIAL
- Supporto JSON e JSON Base64 (decodifica/ricodifica trasparente)
- Cache shadow in mods\Aurie\traduzioni\cache\ (nessuna modifica ai file originali)
- Log operativo in IO_LOG.txt
- REF e REF_NAME non vengono mai modificati

Struttura cartelle
  The Slormancer\
    The Slormancer.exe
    mods\
      Aurie\
        YYToolkit.dll
        The_Slormancer_ITA.dll    <-- questa mod
        traduzioni\
          redirects.json
          dat_str_it.json
          dat_cat_it.json
          cache\
            _PROM_EN__*.json

Installazione
1) Requisiti: The Slormancer (PC/Steam) e YYToolkit funzionante.
2) Scarica il file ZIP ed estrailo nella stessa cartella dell'eseguibile del gioco.
3) Avvio: alla prima lettura, la mod crea in traduzioni\cache\ i file _PROM_EN__*.json con i campi EN riempiti dai corrispondenti IT.

Alcuni dettagli per gli smanettoni:

redirects.json (esempio)
[ 
  { "from": "dat_str.json", "to": "dat_str_it.json" },
  { "from": "dat_cat.json", "to": "dat_cat_it.json" }
]
Note: "from" è una substring del path originale confrontata in minuscolo. Usa pattern specifici (nome file completo) per evitare collisioni.

Come funziona la promozione IT→EN
- EN              ← IT
- EN_NAME         ← IT_NAME
- EN_DESCRIPTION  ← IT_DESCRIPTION
- EN_DIAL         ← IT_DIAL

Regole:
- si copia solo se il campo IT esiste e non è vuoto
- REF, REF_NAME e tutti gli altri campi non whitelistati restano invariati
- se il file è Base64: decodifica → promozione → ricodifica in cache
- se nulla cambia, la cache non viene creata

Uso e aggiornamenti
- dopo aver modificato un *_it*.json, elimina la relativa copia in traduzioni\cache\ per rigenerarla
- controlla IO_LOG.txt per vedere: REDIR OK (TRAD), REDIR OK (TRAD+PROM), REDIR MISS

Risoluzione problemi
- Vedo inglese: verifica che i campi IT siano presenti/non vuoti; controlla redirects.json; elimina la cache e riavvia.
- Comportamenti strani in combattimento: probabilmente un pattern "from" troppo generico ha catturato file non testuali. Rendi i match più specifici (nome file esatto .json).
- Base64: gestito automaticamente. Se il contenuto non è JSON, la promozione viene saltata (nessun rischio di corruzione).

Compilazione
- Toolchain: Visual Studio 2022, C++17
- Dipendenze: YYToolkit (headers/librerie)
- Output: The_Slormancer_ITA.dll in mods\Aurie\

FAQ
Q: La mod cambia il gameplay?
A: No. Modifica solo i testi. La logica di gioco non viene toccata.

Q: Posso disattivare la promozione IT→EN e tenere solo il redirect?
A: Sì. Usa file già promossi oppure richiedi un semplice toggle a file-guard.

Q: Posso estendere ad altre lingue?
A: Sì, aggiungendo nuove coppie (es. FR→EN) nel codice.

Crediti e licenza
- YYToolkit: crediti ai rispettivi autori
- Questo progetto è non ufficiale e non affiliato a The Slormancer o ai suoi autori

Changelog
- v0.3 — Redirect I/O + promozione IT→EN (EN, EN_NAME, EN_DESCRIPTION, EN_DIAL), supporto JSON/Base64, cache shadow, logging.
