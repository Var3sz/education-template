# Dolgozat: Könyvkölcsönző nyilvántartás (Mini Library) – Vue.js Options API

## Cél
Készíts egy **két komponensből álló** Vue alkalmazást, amellyel könyvkölcsönzéseket lehet kezelni:  
új könyv felvétele, kölcsönzött/visszahozott állapot váltása, törlés, visszahozott könyvek ürítése, statisztika és egyszerű szűrés.

A feladat célja: **methods, props, emit, computed** használata, valamint a **JavaScript tömbfüggvények** tudatos alkalmazása (*map, filter, find, some, reduce, sort*).

---

# Kötelező technológiák
- **Vue.js (Options API)**
- **Bootstrap 5**
- **Bootstrap Icons**

---

# Komponensek (pontosan 2)

1. **App.vue** – szülő komponens (lista + statisztika + új könyv felvétele + szűrés)  
2. **BookItem.vue** – gyerek komponens (1 könyv megjelenítése + gombok)

---

# Adatszerkezet (kötelező)

Egy könyv objektum:

```js
{
  id: Number,
  title: String,
  borrower: String,
  returned: Boolean
}
```

- **id** – egyedi azonosító  
- **title** – könyv címe  
- **borrower** – kölcsönző neve  
- **returned** – boolean (vissza lett-e hozva)

---

# Funkcionális követelmények

## 1) App.vue (szülő komponens)

### Kötelező data

- **books** – könyvek tömbje
- **newTitle** – új könyv címe (input)
- **newBorrower** – kölcsönző neve (input)
- **filterMode** – szűrés aktuális értéke
- **error** – hibaüzenet (string)

---

### Kötelező methods

- **addBook()** – új könyv hozzáadása *validációval*
- **removeBook(id)** – könyv törlése
- **toggleReturned(id)** – kölcsönzött / visszahozott állapot váltása
- **clearReturned()** – **minden visszahozott** könyv törlése
- **setFilter(mode)** – szűrés beállítása

---

### Kötelező computed

- **totalBooks** – összes könyv száma
- **returnedCount** – visszahozott könyvek száma
- **borrowedCount** – még kölcsönben lévő könyvek száma
- **returnedPercent** – visszahozott könyvek aránya százalékban (0–100, **egészre kerekítve**)
- **filteredBooks** – a szűrés alapján megjelenítendő lista
- **sortedBooks** – a megjelenítendő lista címszerint rendezve

---

### Kötelező Bootstrap UI

- Navbar / fejléc: **"Mini Library"**
- Űrlap:
  - 1 input a könyv címének
  - 1 input a kölcsönző nevének
  - **"Hozzáadás"** gomb
- Szűrő gombsor vagy select:
  - **Összes**
  - **Kölcsönben**
  - **Visszahozott**
- Statisztika **Bootstrap card**-ban
- Lista **list-group**-ban vagy **table**-ben
- Külön gomb: **"Visszahozott könyvek törlése"** (`clearReturned`)

---

# 2) BookItem.vue (gyerek komponens)

### Kötelező props

- **book** – a könyv objektum

---

### Kötelező emit események

- **remove** – paraméter: `id`
- **toggle-returned** – paraméter: `id`

---

### Kötelező methods

- **onToggleReturned()**
- **onRemove()**

---

### Kötelező Bootstrap Icons (ikonos gombok)

Állapotváltás ikonok:

- `bi-book`
- `bi-book-fill`

Törlés ikon:

- `bi-trash`

Az ikon kiválasztása az állapottól függjön:

- ha a könyv még kölcsönben van → `bi-book`
- ha vissza lett hozva → `bi-book-fill`

---

# Validáció (minden kötelező)

- A könyv címe **nem lehet üres**.
- A kölcsönző neve **nem lehet üres**.
- Nem lehet két olyan rekord a listában, ahol a **könyv címe és a kölcsönző neve egyszerre teljesen azonos**.
- Hiba esetén az `error` változóban jelenjen meg üzenet, és az UI-ban is látszódjon  
  (pl. **Bootstrap alert**).

---

# Működési szabályok (kötelező)

- A gyerek komponens **nem módosíthatja közvetlenül** a `books` tömböt.
- Minden módosítás (**törlés, állapotváltás**) **emit → szülő method** útvonalon történjen.

A `returnedPercent` számítása:

```js
if (totalBooks === 0) {
  return 0
}

return Math.round(returnedCount / totalBooks * 100)
```

- A listában mindig a **szűrt és rendezett** elemek jelenjenek meg.
- A rendezés **címszerint növekvő sorrendben** történjen.

# Elvárt működés – példa

1. A felhasználó beírja: **"A Pál utcai fiúk"**, kölcsönző: **"Anna"** → hozzáadja a listához  
2. A felhasználó beírja ugyanezt újra ugyanazzal a névvel → **hibaüzenet jelenik meg**  
3. A felhasználó másik könyvet is felvesz a listába  
4. A felhasználó a szűrőt **"Kölcsönben"** értékre állítja → csak azok látszanak, amelyek még nincsenek visszahozva  
5. A felhasználó egy könyvnél állapotot vált → a könyv **visszahozott** lesz  

A statisztikában frissül:

- összes könyv száma
- visszahozott könyvek száma
- kölcsönben lévő könyvek száma
- visszahozási arány

A **"Visszahozott könyvek törlése"** gomb eltávolít minden visszahozott könyvet.

---

# Minimum elvárás a beadásban

- 2 komponensből álló Vue alkalmazás
- helyes **props** és **emit** használat
- minden kötelező **methods** megvalósítva
- minden kötelező **computed** megvalósítva
- **Bootstrap 5 + Bootstrap Icons** használata
- validáció működése
- működő **szűrés és rendezés**