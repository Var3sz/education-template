# Dolgozat: Teendőlista (Mini Todo) – Vue.js Options API

## Cél
Készíts egy *két komponensből álló* Vue alkalmazást, amellyel teendőket lehet kezelni:
felvétel, kész/aktív állapot váltás, törlés, kész elemek ürítése, statisztika.

A feladat célja: *methods, props, emit, computed* használata.

---

## Kötelező technológiák
- *Vue.js (Options API)*
- *Bootstrap 5*
- *Bootstrap Icons*

---

## Komponensek (pontosan 2)
1. *App.vue* – szülő komponens (lista + statisztika + új teendő felvétel)
2. *TodoItem.vue* – gyerek komponens (1 teendő megjelenítése + gombok)

---

## Adatszerkezet (kötelező)
Egy teendő objektum:
- id – egyedi azonosító
- text – teendő szövege
- done – boolean (kész-e)

---

## Funkcionális követelmények

### 1) App.vue (szülő komponens)

#### Kötelező data
- todos – teendők tömbje
- newText – új teendő szövege (input)
- error – hibaüzenet (string)

#### Kötelező methods
- addTodo() – új teendő hozzáadása *validációval*
- removeTodo(id) – teendő törlése
- toggleDone(id) – kész/aktív állapot váltása
- clearDone() – *minden kész* teendő törlése

#### Kötelező computed
- totalTodos – teendők száma
- doneCount – kész teendők száma
- activeCount – aktív teendők száma
- progressPercent – készültség százalékban (0–100, *egészre kerekítve*)

#### Kötelező Bootstrap UI
- Navbar/fejléc: „Mini Todo”
- Űrlap: 1 input (teendő szövege) + „Hozzáadás” gomb
- Statisztika **Bootstrap card**-ban (a 4 computed értékkel)
- Lista **list-group**-ban vagy **table**-ben
- Külön gomb: „Kész feladatok törlése” (clearDone)

---

### 2) TodoItem.vue (gyerek komponens)

#### Kötelező props
- todo – a teendő objektum

#### Kötelező emit események
- remove – paraméter: id
- toggle-done – paraméter: id

#### Kötelező methods
- onToggleDone()
- onRemove()

#### Kötelező Bootstrap Icons (ikonos gombok)
- kész/aktív váltás: bi-check-circle / bi-circle (állapottól függően)
- törlés: bi-trash

---

## Validáció (minden kötelező)
- A teendő szövege *nem lehet üres*.
- Nem lehet két *teljesen azonos* szövegű teendő a listában.
- Hiba esetén az error változóban jelenjen meg üzenet, és az UI-ban is látszódjon (pl. alert).

---

## Működési szabályok (kötelező)
- A gyerek komponens *nem módosíthatja közvetlenül* a todos tömböt.
- Minden módosítás (törlés, állapotváltás) *emit → szülő method* útvonalon történjen.
- A progressPercent számítása:
  - ha totalTodos == 0, akkor 0
  - különben Math.round(doneCount / totalTodos * 100)