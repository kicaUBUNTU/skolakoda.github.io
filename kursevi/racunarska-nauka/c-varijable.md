---
title: Šta su varijable?
layout: lekcija-racunarstvo
permalink: /c-varijable
image: /images/koncepti/varijable/varijabla.png
---

![]({{page.image}})

**Riječ «varijabla» dolazi iz engleske riječi «variable» što znači «promjenjiva vrijednost». Definicija varijable glasi: varijabla je memorijska lokacija čiji se sadržaj može mijenjati tokom izvođenja programa.**

Znači svaka varijabla u programu predstavlja neku memorijsku lokaciju i memorijsko zauzeće (zauzeće ovisi o tipu varijable) koje mi možemo mijenjati. U pravom svijetu varijablu bi najlakše mogli zamisliti kao jednu ladicu u velikom ormaru gdje se mogu staviti bilo kakve «stvari», pod uvjetom da su te stvari istog tipa. Ovo zahtijevanje da su «stvari» istog tipa proizlazi iz činjenice da prije nego što želimo koristiti neku varijablu moramo ju definirati kao varijablu određenog tipa.

Neki tipovi varijabli su brojevne varijable (cjelobrojne, decimalne…), znakovne (slova, interpunkcijski znakovi…) i ostale strukture izvedene od osnovnih (rezerviranih) tipova varijabli. Mnogi programski jezici dozvoljavaju definiranje vlastitih tipova varijabli (C/C++, VB, Pascal).

## Zapis varijable u memoriji računala

Format zapisa varijable u memoriji ovisi o tipu varijable, ali ono što se ne mijenja kod nijednog zapisa varijable (kao ni kod bilo kojeg drugog zapisa u memoriji) je to da se vrši zapisivanjem bajtova u memoriji. Npr. u C/C++-u reprezentacija varijable tipa `int` (cijeli broj) je zapis od 4 bajta (u nizu). Znači kada želimo koristiti varijablu tipa `int` u C/C++-u, u memoriji se zauzme memorijska lokacija (određeni početni bajt) i njena sljedeća 3 bajta (ukupno 4).

## Operacije sa varijablama

Ovisno o tipu varijable i programskom jeziku možemo izvršavati različite operacije nad varijablama. Npr. nad varijablama brojevnog tipa možemo izvršavati aritmetičke operacije (zbrajanje, oduzimanje, množenje…).

Programski jezici poput C++ dozvoljavaju vlastito definiranje operacija nad (vlastitim) tipovima varijabli što nam donosi veliku slobodu u pisanju programa.

## Zaključak

Varijable su neizostavan dio svakog programskog jezika i služe kao pomoć u samom procesu programiranja (točno poznavanje memorijske lokacije je zamijenjeno poznavanjem definicije varijable).


Izvor: [tutorijali.net](//tutorijali.net/)
