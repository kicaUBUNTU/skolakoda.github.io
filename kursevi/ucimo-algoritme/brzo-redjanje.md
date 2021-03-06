---
title: Brzo ređanje (quicksort)
layout: lekcija-algoritmi
permalink: /brzo-redjanje
---

![](https://upload.wikimedia.org/wikipedia/commons/6/6a/Sorting_quicksort_anim.gif)

**“Quicksort” je do sada najbrži poznati algoritam za sortiranje. I ovo je rekurzivni algoritam zasnovan na strategiji “podeli pa vladaj”.**

Algoritam se sastoji od sledećih koraka:

1. Odabir jednog člana niza, tzv. *pivot*-a.
2. Raspodeliti članove niza tako da sve elemente manje od pivota stavimo ispred njega (podniz 1), a veće iza njega (podniz 2). Nakon te raspodele pivot se nalazi na svojoj konačnoj poziciji.
3. Rekurzivno sortirati svaki podniz na isti način.

## Pozadina

Osnovna ideja kod *selection sort* algoritma je da se jedan element postavi na svoje mesto, a zatim da se isti metod rekurzivno primeni na niz koji je za jedan kraći od polaznog. S obzirom da je pripremna akcija zahtevala O(n) operacija, dobijena je jednačina `T (n) = T (n − 1) + O(n)`, čije rešenje je `O(n^2)`. Sa druge strane, kod *merge sort* algoritma sortiranje se svodi na sortiranje dva podniza polaznog niza dvostruko manje dimenzije. S obzirom da korak objedinjavanja dva sortirana niza zahteva O(n) operacija, dobija se jednačina `T (n) = 2T (n/2) + O(n)`, čije je rešenje O(n log n).

**Dakle, značajno je efikasnije da se problem dimenzije `n` svodi na dva problema dimenzije `n/2` nego na jedan problem dimenzije `n-1`** — ovo je osnovna ideja takozvanih podeli i vladaj algoritama u koje spada i *quick sort*.

## Algoritam

![](https://upload.wikimedia.org/wikipedia/commons/8/84/Lomuto_animated.gif)

`Quick sort` algoritam pokušava da postigne bolju efikasnost, modifikujući osnovnu ideju *selection sort* algoritma tako što umesto minimuma (ili maksimuma), u svakom koraku na svoje mesto dovede neki element (obično nazivan **pivot**) koji je relativno blizu sredine niza. Međutim, da bi nakon toga, problem mogao biti sveden na sortiranje dva dvostruko manja podniza, potrebno je prilikom dovođenja pivota na svoje mesto grupisati sve elemente manje od njega levo od njega, a sve elemente veće od njega desno od njega. Dakle, ključni korak *quick sort* algoritma je tzv. korak particionisanja koji nakon izbora nekog pivotirajućeg elementa podrazumeva da se niz organizuje da prvo sadrži elemente manje od pivota, zatim pivotirajući element, i na kraju elemente veće od pivota.

`Quick sort` algoritam može se implementirati na sledeći način. Poziv `qsort_(a, l, d)` sortira deo niza `a[l, d]`.

```c
void qsort_(int a[], int l, int d) {
  if (l < d) {
    razmeni(a, l, izbor_pivota(a, l, d));
    int p = particionisanje(a, l, d);
    qsort_(a, l, p - 1);
    qsort_(a, p + 1, d);
  }
}
```

Funkcija `qsort` se onda jednostavno implementira

```c
void qsort(int a[], int n) {
  qsort_(a, 0, n-1);
}
```

Funkcija `izbor_pivota` odabire za pivot neki element niza `a[l, d]` i vraća njegov indeks (u nizu a). Pozivom funkcije razmeni pivot se postavlja na poziciju `l`. Funkcija particionisanje vrši particionisanje niza (pretpostavljajući da se pre particionisanja pivot nalazi na poziciji `l`) i vraća poziciju na kojoj se nalazi pivot nakon particionisanja. Funkcija se poziva samo za nizove koji imaju više od jednog elementa te joj je preduslov da je `l` manje ili jednako `d`. Postuslov funkcije particionisanje je da je (multi)skup elemenata niza a nepromenjen nakon njenog poziva, međutim njihov redosled je takav da su svi elementi niza `a[l, p-1]` manji ili jednaki elementu `a[p]`, dok su svi elementi niza `a[p+1, d]` veći ili jednaki od elementa `a[p]`.

Napomenimo da je *quick sort* algoritam koji u praksi daje najbolje rezultate kod sortiranja dugačkih nizova. Međutim, važno je napomenuti da za sortiranje kraćih nizova naivni algoritmi (npr. *insertion sort*) mogu da se pokažu praktičnijim. Većina realnih implementacija *quick sort* algoritma koristi hibridni pristup — izlaz iz rekurzije se vrši kod nizova koji imaju nekoliko desetina elemenata i na njih se primenjuje *insertion sort*.

Implementacije particionisanja. Jedna od mogućih implementacija koraka particionisanja je sledeća:

```c
int particionisanje(int a[], int l, int d) {
  int p = l, j;
  for (j = l+1; j <= d; j++)
    if (a[j] < a[l])
      razmeni(a, ++p, j);

  razmeni(a, l, p);
  return p;
}
```

Invarijanta petlje je da je (multi)skup elemenata u nizu a nepromenjen, kao i da se u nizu a na poziciji `l` nalazi pivot, da su elementi `a[l+1, p]` manji od pivota, dok su elementi `a[p+1, j-1]` su veći ili jednaki od pivota. Nakon završetka petlje, `j` ima vrednost `d+1`, te su elementi `a[p+1, d]` veći ili jednaki od pivota. Kako bi se ostvario postuslov funkcije particionisanje vrši se još razmena pivota i elementa na poziciji `p` — time pivot dolazi na svoje mesto (na poziciju `p`).

Drugi način implementacije particionisanja je zasnovan na Dijkstrinom algoritmu „trobojke“ (*Dutch National Flag Problem*). U ovom slučaju, radi se malo više od onoga što postuslov striktno zahteva — niz se permutuje tako da prvo idu svi elementi striktno manji od pivota, zatim sva pojavljivanja pivota i na kraju svi elementi striktno veći od pivota.

```c
int particionisanje(int a[], int l, int d) {
  int pn = l-1, pp = d+1, pivot = a[l], t = l;
  while (t < pp) {
    if (a[t] < pivot)
      razmeni(a, t++, ++pn);
    else if (a[t] > pivot)
      razmeni(a, t, --pp);
    else
      t++;
  }
  return pn+1;
}
```

Invarijanta petlje je da se (multi)skup elemenata niza ne menja, da su svi elementi niza `a[l, pn]` manji od pivota, da su svi elementi niza `a[pn+1, t-1]` jednaki pivotu, i da su svi elementi niza `a[pp, d]` veći od pivota. Kada se petlja završi važi da je t jednako pp tako da su svi elementi niza `a[l, pn]` manji od pivota, niza `a[pn+1, pp-1]` jednaki pivotu, a niza `a[pp, d]` veći od pivota.

Treći mogući način implementacije koraka particionisanja je da se obilazi paralelno sa dva kraja i kada se na levom kraju nađe element koji je veći od pivota, a na desnoj neki koji je manji od pivota, da se izvrši njihova razmena.

```c
int particionisanje(int a[], int l, int d) {
  int l0 = l;

  while (l < d) {
    while (a[l] <= a[l0] && l < d)
      l++;
    while (a[d] >= a[l0] && l < d)
      d--;
    if (l < d)
      razmeni(a, l, d);
  }

  if (a[l] >= a[l0])
    l--;
  razmeni(a, l0, l);
  return l;
}
```

Invarijanta spoljašnje petlje je da je `l` manje ili jednako `d`, da je (multi)skup elemenata niza a nepromenjen (što je očigledno jer algoritam zasniva isključivo na razmenama), da se na poziciji `l0` nalazi pivot i da su elementi `a[l0, l-1]` manji od pivota (gde je `l0` početna vrednost promenljive `l`), a elementi `a[d+1, d0]` (gde je `d0` početna vrednost promenljive `d`) veći ili jednaki od pivota. Po završetku petlje je `l` jednako `d`. Element na toj poziciji još nije ispitan i njegovim ispitivanjem se osigurava da `l` bude takav da su elementi `a[l0, l]` manji ili jednak od pivota, a elementi niza `a[l+1, d0]` veći ili jednaki od pivota. Odavde, nakon zamene pozicija `l0` i `l` postiže se postuslov particionisanja.

S obzirom da se u svakom koraku petlji smanjuje pozitivna celobrojna vrednost `d - l`, a sve petlje se zaustavljaju u slučaju kada je `d - l` nula zaustavljanje sledi.

Naredna implementacija optimizuje prethodnu ideju, tako što izbegava korišćenje zamena.

```c
int particionisanje(int a[], int l, int d) {
  int pivot = a[l];
  while (l < d) {
    while (a[d] >= pivot && l < d)
      d--;
    if (l != d)
      a[l++] = a[d];

    while (a[l] <= pivot && l < d)
      l++;
    if (l != d)
      a[d--] = a[l];
  }
  a[l] = pivot;
  return l;
}
```

Invarijanta spoljašnje petlje je da je `l` manje ili jednako `d`, da je (multi)skup elemenata niza a van pozicije `l` jednak (multi)skupu elemenata polaznog niza bez jednog pojavljivanja pivota, da su elementi `a[l0, l-1]` manji od pivota (gde je `l0` početna vrednost promenljive `l`), a elementi `a[d+1, d0]` (gde je `d0` početna vrednost promenljive `d`) veći ili jednaki od pivota. Na sredini spoljne petlje, invarijanta se donekle naruši — svi uslovi ostaju da važe, osim što je (multi)skup elemenata `a` van pozicije `d` (umesto van pozicije `l`) jednak (multi) skupu elemenata polaznog niza bez jednog pojavljivanja pivota. Kada se petlja završi, `l` je jednako `d` i upisivanjem (sačuvane) vrednosti pivota na ovo mesto se postiže postuslov funkcije particionisanja. Zaustavljanje je analogno prethodnom slučaju.

**Izbor pivota.** Kao što je već rečeno, iako je poželjno da se pivot izabere tako da podeli niz na dve potpuno jednake polovine, to bi trajalo previše tako da se obično pribegava heurističkim rešenjima. Ukoliko se može pretpostaviti da su elementi niza slučajno raspoređeni (što se uvek može postići ukoliko se pre primene sortiranja niz permutuje na slučajan način), bilo koji element niza se može uzeti za pivot. Na primer,

```c
int izbor_pivota(int a[], int l, int d) {
  return l;
}
```

ili

```c
int izbor_pivota(int a[], int l, int d) {
  return slucajan_broj(l, d);
}
```

Bolje performanse se mogu postići ukoliko se npr. za pivot uzme srednji od tri slučajno izabrana elementa niza.

![](https://upload.wikimedia.org/wikipedia/commons/9/9c/Quicksort-example.gif)


Izvor: Predrag Janičić i Filip Marić, *PROGRAMIRANJE 2, Osnove programiranja kroz programski jezik C*, Matematički fakultet, Beograd, 2017.
