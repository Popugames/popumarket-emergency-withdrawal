# Retragere de urgență PopuMarket

> **Stare: încă nu e activă pe mainnet.** Funcțiile sunt în testare pe testnet. Nu trimite aceste tranzacții pe mainnet până când acest anunț nu e scos.

[Read in English](README.md)

Acest ghid explică cum îți iei TCL-ul direct din contractul PopuMarket, fără site-ul sau serverele PopuMarket.

## Înainte să ai nevoie

Poți folosi această metodă doar dacă ți-ai legat portofelul cât timp PopuMarket funcționa. Fă asta acum:

1. În PopuMarket, deschide pagina de retragere TCL.
2. Conectează portofelul și alege **Leagă portofelul**. Dacă ai un portofel asociat în joc, trebuie să-l folosești pe acela.
3. Salvează **ID-ul contului**, afișat în aceeași secțiune.

## De ce ai nevoie

- Portofelul legat
- Puțin EGLD pentru taxele de rețea
- ID-ul contului, în format hexazecimal

**Dacă nu ai salvat ID-ul contului:** găsește tranzacția ta `bindWallet` în [MultiversX Explorer](https://explorer.multiversx.com). La **Logs**, deschide evenimentul `wallet_bound`. ID-ul contului e al doilea topic.

## Cum îți iei TCL-ul

1. Deschide [wallet.multiversx.com](https://wallet.multiversx.com) și conectează portofelul legat.
2. Alege **Send** și completează:
   - **To:** `erd1qqqqqqqqqqqqqpgq65t4mhrg4g8ymc83n4rhrwpa6x9uvsf60ufsfgcu9z`
   - **Amount:** `0`
   - **Gas limit:** `30000000`
   - **Data:** `requestEmergencyWithdrawal@<ID_CONT>`
3. Așteaptă 24 de ore.
4. Trimite a doua tranzacție cu aceleași date, în afară de câmpul Data:
   - **Data:** `executeEmergencyWithdrawal@<ID_CONT>`

Tot soldul contului ajunge în portofelul legat. Dacă te răzgândești înainte de pasul 4, trimite în schimb `cancelEmergencyWithdrawal@<ID_CONT>`.

Cât timp PopuMarket funcționează, cererea anulează și ordinele și ofertele tale deschise care țin TCL, iar ce țin revine în sold. Nu poți cheltui sau retrage TCL din piață până nu anulezi cererea sau iei TCL-ul.

## Detalii despre contract

| | |
|---|---|
| Rețea | MultiversX mainnet |
| Contract | `erd1qqqqqqqqqqqqqpgq65t4mhrg4g8ymc83n4rhrwpa6x9uvsf60ufsfgcu9z` |
| Proprietar (multisig, 2 din 3) | `erd1qqqqqqqqqqqqqpgqprl0xalqslxq9rh6qlg40mz9c6m3z6wg0ufszvw3j4` |
| Token | `TCL-fe459d` |

## Funcții

| Funcție | Cine o poate apela | Ce face |
|---|---|---|
| `bindWallet` | Jucătorul, din site | Leagă un portofel de cont. Are nevoie de un bilet semnat de serverul PopuMarket. |
| `requestEmergencyWithdrawal@<ID_CONT>` | Portofelul legat | Pornește cele 24 de ore și blochează debitările contului după 10 minute |
| `cancelEmergencyWithdrawal@<ID_CONT>` | Portofelul legat | Anulează cererea |
| `executeEmergencyWithdrawal@<ID_CONT>` | Portofelul legat | După 24 de ore, trimite tot soldul contului în portofelul legat |
| `proposeWalletRecovery` | Proprietarul (multisig) | Propune un portofel nou pentru cont, cu așteptare de 7 zile |
| `cancelWalletRecovery@<ID_CONT>` | Portofelul legat sau proprietarul | Anulează schimbarea propusă |
| `finalizeWalletRecovery@<ID_CONT>` | Oricine, după 7 zile | Aplică schimbarea, dacă nu există o retragere de urgență în curs |
| `getAccountWallet`, `getBalance`, `getEmergencyUnlock`, `getWalletRecovery` | Oricine (doar citire) | Arată portofelul legat, soldul, momentul deblocării și schimbarea propusă |

## Ce protejează și ce nu

- **După cerere:** guardianul, proprietarul și pauza contractului nu pot anula cererea sau plata. Operatorul nu poate debita contul la 10 minute după cerere, ca operațiunile în curs să se poată termina.
- **Înainte de cerere și în acele 10 minute:** operatorul PopuMarket poate executa în continuare operațiuni pe soldul tău.
- **Actualizări:** proprietarul multisig poate actualiza codul contractului.
- **Portofel pierdut:** doar multisig-ul poate lega un portofel nou. Schimbarea e publică timp de 7 zile, iar portofelul actual o poate anula.
- **Fără portofel legat:** această metodă nu e disponibilă.
- **Portofel furat:** cine controlează portofelul legat poate cere retragerea.

Acest ghid conține informații tehnice. Nu este un sfat financiar și nu promite nicio valoare pentru token.
