# Muutoskone

Strateginen muutostyökalu. Kerää XP:tä päivittäisillä rutiineilla, kasvata niittyä ja täytä purkki.

Kaikki data tallentuu vain omalle laitteelle. Ei tiliä, ei pilveä, ei seurantaa.

---

## Julkaisu GitHubiin

Vie **tämän kansion kaikki tiedostot** repositoryn juureen — ei alikansioon, muuten tulee 404.

1. Luo repository, esim. `MUUTOSKONE`.
2. Raahaa tiedostot yksitellen latausikkunaan (älä koko kansiota — se säilyttää kansiorakenteen).
3. **Settings → Pages → Deploy from a branch → main → / (root) → Save**.
4. Osoite on minuutin päästä `https://<käyttäjänimesi>.github.io/MUUTOSKONE/`.

### Asennus laitteelle

- **iPhone:** Safari → jakonappi → *Lisää kotivalikkoon*
- **Android:** Chrome → valikko → *Asenna sovellus*
- **Työpöytä:** Chrome/Edge → osoitepalkin asennuskuvake

---

## Tiedostot

| Tiedosto | Mitä varten |
|---|---|
| `index.html` | Koko sovellus. Sisältää kaiken: tyylit, logiikan, kuvakkeet. |
| `manifest.json` | Tekee sovelluksesta asennettavan. |
| `sw.js` | Offline-käyttö. |
| `icon-192.png`, `icon-512.png` | Sovelluskuvakkeet. |
| `icon-maskable.png` | Android-kuvake, jossa turvamarginaali. |
| `apple-touch-icon.png` | iPhonen kotivalikon kuvake. |

`index.html` toimii yksinään myös ilman muita tiedostoja — muut lisäävät asennettavuuden ja offline-tuen.

---

## Tärkeää päivittäessä

Kun muutat `index.html`:ää, **nosta `sw.js`:n ensimmäisellä rivillä versionumeroa**:

```js
const C = 'muutoskone-v12';   →   'muutoskone-v13'
```

Ilman tätä selain tarjoilee vanhaa välimuistiversiota etkä näe muutoksiasi.

---

## Mitä sovelluksessa on

**XP ja purkki.** Rutiinit tuottavat XP:tä (1 / 5 / 20 / 50). Jokainen XP kerrotaan kertymäkertoimella, joka kasvaa päivästä 1 päivään 84 välillä ×1.00 → ×2.00. Purkki näyttää matkan tavoitteeseen, ja sen syaani-violetti on niityn oma väri: mesi kantaa kukkien sävyn.

**Niitty.** Jokainen kuitattu rutiini istuttaa kukan. Laji määräytyy XP-tason mukaan ja pysyy samana:

- 1 XP — ketoneilikka, kissankello
- 5 XP — päivänkakkara, ruiskaunokki
- 20 XP — ahdekaunokki, nurmikaunokki
- 50 XP — maarianohdake

Kukka **aukeaa vähitellen** nupusta terälehti kerrallaan, ei ilmesty kerralla valmiina.

Kortin yläreunassa näkyvät **tänään kukkineet** yksittäisinä ruutuina. Uusin niistä aukeaa silmiesi edessä, kun kuittaat rutiinin.

**Napauta mitä tahansa kukkaa** — niityllä tai päivän riviltä — niin näet sen lajin, mistä rutiinista se syntyi, milloin se on tehty ja monesko kukka se on niitylläsi. Kukka aukeaa ikkunassa alusta uudelleen.

Ikkunan viimeinen rivi on tarkoituksella tällainen: *"Ilman tätä yhtä niitty olisi pienempi. Jokaisella niistä on merkitystä."* Yksittäistä tekoa ei vähätellä rakennuspalikaksi — se on korvaamaton, ja niin on jokainen muukin.

Niitty ei koskaan kuihdu. Kukat vain kertyvät.

**Mehiläiset.** Määrä seuraa kertymäkerrointa: päivänä 1 yksi, päivänä 84 parvi. Kun kuittaat rutiinin, mehiläinen käy uudella kukalla ja vie sadon purkkiin. Niitty sijaitsee heti rutiininappien alla, jotta uusi kukka ilmestyy siihen mihin katse jo osuu.

**Ajastin.** Kasvi kasvaa jakson ajan sekä ajastinpalkissa että hyppyikkunassa, ja puhkeaa lopuksi kukkaan.

**Muut.** Aktiivisuusputki armopäivineen, tasot, virstanpylväät, päiväkirja, viikkohaaste, etenemisennuste, lämpökartta, tilastot, varmuuskopiointi.

---

## Muokkaus

Yksi tiedosto, ei riippuvuuksia, ei build-vaihetta. Avaa `index.html` editorissa.

Niitty, mehiläiset ja ajastimen kasvi ovat tiedoston lopussa omassa lohkossaan, joka alkaa kommentilla `NIITTY, MEHILÄISET JA AJASTIMEN KASVI`. Se ei muuta XP-logiikkaa, purkkia eikä tallennusta — kytkennät on tehty käärimällä `addXP`, `undoXP`, `updateUI`, `startTimer` ja `updateTimerChip`.

Hyödyllisiä kohtia lohkon sisällä:

- `TIER` — XP-tasojen kukkakoot ja lajit
- `drawHead` — yksittäisten lajien piirto
- `beeCount` — mehiläisten määrän kaava
- `SWAYMAX` — kuinka monella kukalla tuulen huojutus vielä on päällä
- `drawOne` — yksittäisen kukan muotokuva päivän riville ja kukkaikkunaan
- `mkFlowerInfo` — kukan napautus ja sen tietoikkuna

Kukkien aikaleimat tallentuvat taulukkoon `fts`, joka kulkee `flog`-merkkijonon rinnalla. Ennen tätä istutetut kukat säilyvät, mutta niiltä puuttuu päivämäärä.

Niityn korkeus säädetään `resize()`-funktion rivillä `H=Math.max(168,Math.min(288,Math.round(w*0.56)))`.

Purkin värit ovat SVG-gradienteissa `liqGrad` (täyttyvä) ja `liqGradFull` (täysi).
