# Dolgozat: Bevásárlólista (Mini Shopping List) – Vue.js Options API

## Cél

Készíts egy **két komponensből álló** Vue alkalmazást, amellyel bevásárlólistát lehet kezelni:
felvétel, megvett/nem megvett állapot váltás, törlés, megvett elemek ürítése, statisztika.

A feladat célja: **methods, props, emit, computed** használata, valamint a **JavaScript tömbfüggvények** gyakorlása (*map, filter, find, some, reduce*).

---

# Kötelező technológiák

* **Vue.js (Options API)**
* **Bootstrap 5**
* **Bootstrap Icons**

---

# Komponensek (pontosan 2)

1. **App.vue** – szülő komponens (lista + statisztika + új termék felvétel)
2. **ShoppingItem.vue** – gyerek komponens (1 termék megjelenítése + gombok)

---

# Adatszerkezet (kötelező)

Egy bevásárlólista elem objektum:

```js
{
  id: Number,
  name: String,
  quantity: Number,
  bought: Boolean
}
```

* **id** – egyedi azonosító
* **name** – termék neve
* **quantity** – mennyiség (egész szám)
* **bought** – boolean (meg van-e véve)

---

# Funkcionális követelmények

## 1) App.vue (szülő komponens)

### Kötelező data

* **items** – bevásárlólista elemeinek tömbje
* **newName** – új termék neve (input)
* **newQuantity** – új termék mennyisége (input)
* **error** – hibaüzenet (string)

---

### Kötelező methods

* **addItem()** – új termék hozzáadása *validációval*
* **removeItem(id)** – termék törlése
* **toggleBought(id)** – megvett/nem megvett állapot váltása
* **clearBought()** – **minden megvett** termék törlése

---

### Kötelező computed

* **totalItems** – összes termék darabszáma
* **boughtCount** – megvett elemek száma
* **pendingCount** – még nem megvett elemek száma
* **totalQuantity** – összes mennyiség összege
* **boughtPercent** – megvett elemek aránya százalékban (0–100, **egészre kerekítve**)

---

### Kötelező Bootstrap UI

* Navbar / fejléc: **"Mini Shopping List"**
* Űrlap:

  * 1 input a termék nevének
  * 1 input a mennyiségnek
  * **"Hozzáadás"** gomb
* Statisztika **Bootstrap card**-ban (az 5 computed értékkel)
* Lista **list-group**-ban vagy **table**-ben
* Külön gomb: **"Megvett termékek törlése"** (`clearBought`)

---

# 2) ShoppingItem.vue (gyerek komponens)

### Kötelező props

* **item** – a bevásárlólista elem objektum

---

### Kötelező emit események

* **remove** – paraméter: `id`
* **toggle-bought** – paraméter: `id`

---

### Kötelező methods

* **onToggleBought()**
* **onRemove()**

---

### Kötelező Bootstrap Icons (ikonos gombok)

* megvett / nem megvett váltás

  * `bi-check-circle`
  * `bi-circle`

* törlés

  * `bi-trash`

---

# Validáció (minden kötelező)

* A termék neve **nem lehet üres**.
* A mennyiség csak **pozitív egész szám** lehet.
* Nem lehet két **teljesen azonos nevű** termék a listában.
* Hiba esetén az `error` változóban jelenjen meg üzenet, és az UI-ban is látszódjon
  (pl. **Bootstrap alert**).

---

# Működési szabályok (kötelező)

* A gyerek komponens **nem módosíthatja közvetlenül** az `items` tömböt.
* Minden módosítás (**törlés, állapotváltás**) **emit → szülő method** útvonalon történjen.

A `boughtPercent` számítása:

```js
if (totalItems === 0) {
  return 0
}

return Math.round(boughtCount / totalItems * 100)
```

# Elvárt működés – példa

1. A felhasználó beírja: **"Tej"**, mennyiség: **2** → hozzáadja a listához
2. A felhasználó beírja újra: **"Tej"**, mennyiség: **1** → **hibaüzenet jelenik meg**
3. A felhasználó rákattint egy elem állapotváltó gombjára → a termék **megvett** lesz

A statisztikában frissül:

* összes elem száma
* megvett elemek száma
* nem megvett elemek száma
* összes mennyiség
* megvett százalék

A **"Megvett termékek törlése"** gomb eltávolít minden megvett elemet.

---

# Minimum elvárás a beadásban

* 2 komponensből álló Vue alkalmazás
* helyes **props** és **emit** használat
* minden kötelező **methods** megvalósítva
* minden kötelező **computed** megvalósítva
* **Bootstrap 5 + Bootstrap Icons** használata
* validáció működése