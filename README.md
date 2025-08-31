# THE SLORMANCER — TRADUZIONE ITA (con Aurie e YYToolkit IO Redirect)

## ☕ Supporta il progetto

Se ti piace questo progetto, puoi offrirmi un caffè:

[![Buy Me a Coffee](https://img.shields.io/badge/Buy%20Me%20a%20Coffee-%E2%98%95-yellow?style=for-the-badge)](https://www.buymeacoffee.com/lele344)

---

Mod/DLL per abilitare l’italiano in **The Slormancer** senza toccare la logica di gioco.  
Reindirizza i file di testo verso versioni `_it.json` e **promuove automaticamente** i campi IT → EN (solo: `EN`, `EN_NAME`, `EN_DESCRIPTION`, `EN_DIAL`).  
Funziona sia con JSON normale che con JSON **Base64**. I file originali restano **intatti**: la mod genera copie in **cache**.

---

## Caratteristiche

- Redirect dei path di I/O verso `mods\Aurie\traduzioni\*_it*.json`
- **Promozione IT→EN** su: `EN`, `EN_NAME`, `EN_DESCRIPTION`, `EN_DIAL`
- Supporto **JSON** e **JSON Base64** (decodifica/ricodifica trasparente)
- **Cache shadow** in `mods\Aurie\traduzioni\cache\` (nessuna modifica ai file originali)
- Log operativo in `IO_LOG.txt`
- `REF` e `REF_NAME` **non vengono mai modificati**

---

## Struttura cartelle (accanto a `The Slormancer.exe`)

```text
The Slormancer.exe
mods\
└─ Aurie\
   ├─ YYToolkit.dll
   ├─ The_Slormancer_ITA.dll      <-- questa mod/DLL
   └─ traduzioni\
      ├─ redirects.json
      ├─ dat_str_it.json
      ├─ dat_cat_it.json
      ├─ ... (altri *_it.json)
      └─ cache\
         └─ PROM_EN_*.json
```

---

## Installazione

1. **Requisiti**: The Slormancer (PC/Steam) e **YYToolkit** funzionante.  
2. **Download**: scarica lo ZIP della mod ed **estrailo** nella stessa cartella dell’eseguibile del gioco.  
3. **Avvio**: alla prima lettura, la mod crea in `traduzioni\cache\` i file `PROM_EN_*.json` con i campi EN riempiti dai corrispondenti IT.

---

## `redirects.json` (esempio)

> **Nota**: `"from"` è una *substring* del path originale confrontata in minuscolo.  
> Usa pattern specifici (nome file completo) per evitare collisioni.

```json
[
  { "from": "dat_str.json", "to": "dat_str_it.json" },
  { "from": "dat_cat.json", "to": "dat_cat_it.json" }
]
```

---

## Come funziona la promozione IT→EN

| EN target        | IT source      |
|------------------|----------------|
| `EN`             | `IT`           |
| `EN_NAME`        | `IT_NAME`      |
| `EN_DESCRIPTION` | `IT_DESCRIPTION` |
| `EN_DIAL`        | `IT_DIAL`      |

**Regole:**
- si copia **solo** se il campo IT esiste e **non è vuoto**
- `REF`, `REF_NAME` e **tutti gli altri campi non whitelistati** restano invariati
- se il file è **Base64**: _decodifica → promozione → ricodifica_ in **cache**
- se **nulla cambia**, la cache **non viene creata**

---

## Uso e aggiornamenti

- dopo aver modificato un `_it.json`, **elimina** la relativa copia in `traduzioni\cache\` per forzarne la rigenerazione
- controlla `IO_LOG.txt` per vedere:
  - `REDIR OK (TRAD)` — solo redirect
  - `REDIR OK (TRAD+PROM)` — redirect + promozione EN
  - `REDIR MISS` — nessun match di redirect

---

## Risoluzione problemi

**Vedo inglese**
- verifica che i campi **IT** siano presenti/non vuoti  
- controlla `redirects.json`  
- elimina la **cache** e riavvia

**Comportamenti strani in combattimento**
- probabilmente un pattern `"from"` troppo generico ha catturato file **non testuali**  
  → **Rendi i match più specifici** (nome file esatto `.json`)

**Base64**
- gestito automaticamente  
- se il contenuto **non è JSON**, la promozione viene **saltata** (nessun rischio di corruzione)

---

## Compilazione

- **Toolchain**: Visual Studio 2022, C++17  
- **Dipendenze**: YYToolkit (headers/librerie)  
- **Output**: `mods\Aurie\The_Slormancer_ITA.dll`

---

## FAQ

**D: La mod cambia il gameplay?**  
**R:** No. Modifica solo i **testi**. La logica di gioco **non** viene toccata.

**D: Posso disattivare la promozione IT→EN e tenere solo il redirect?**  
**R:** Sì. Usa file già promossi **oppure** aggiungi un semplice toggle a file-guard.

**D: Posso estendere ad altre lingue?**  
**R:** Sì, aggiungendo nuove coppie (es. **FR→EN**) nel codice.

---

## Crediti e licenza

- **YYToolkit**: crediti ai rispettivi autori  
- Questo progetto è **non ufficiale** e **non affiliato** a *The Slormancer* o ai suoi autori

---

## Changelog

- **v0.3** — Redirect I/O + promozione IT→EN (`EN`, `EN_NAME`, `EN_DESCRIPTION`, `EN_DIAL`), supporto JSON/Base64, cache shadow, logging
