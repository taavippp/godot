---
title: GDScript
layout: default
nav_order: 3
has_children: false
---

# GDScript

## Tühikud

Selles osas tutvume lähemalt GDScripti erinevate osadega. [Tutvustuse peatükis](./tutvustus/index.md) natuke õppisid juba seda Godot'le loodud keelt, aga nüüd saad teada, mida täpsemalt GDScriptiga teha saab.

GDScript on süntaksi poolest kohati sarnane Python keelele - mõlemal keelel on iga rea taane oluline.
Esimene funktsioon siin koodiplokis viskaks veateate, teine mitte.

```gdscript
# Valesti taandatud kood
func _ready() -> void:
print("tere")

# Korrektselt taandatud kood
func _process(delta: float) -> void:
    print("Möödus " + delta + " sek")
```

Vaikimisi üritab Godot redaktor sulle õiget taanet pakkuda. Kui deklareerid näiteks funktsiooni, tingimuslause või tsükli (*if*/*while*/*for*), siis järgnev rida algab automaatselt tabulaatoriga.

## Andmetüübid

Godot's on järgnevad andmetüübid:

-   null
    -   tühi/puuduv väärtus
-   bool
    -   true/false ehk tõene/väär või 1/0 väärtus
-   int
    -   täisarv
-   float
    -   ujuvkomaarv
-   string
    -   tekst

## Konteinerid

GDScriptis on erinevad konteinerid mitme ühte andmetüüpi muutuja hoidmiseks.

-   Array
    -   massiiv
    -   Godot 4. versioonist alates on neile võimalik andmetüüpe määrata nii: `Array[tüüp]`, nt `Array[int]`
-   Packed Array
    -   kuna tavaline massiiv on loodud igasuguseid andmetüüpe ja klasse sisaldama, siis suurte andmekogustega tegelemiseks on mõne andmetüübi jaoks olemas PackedArray, millega opereerimine on palju kiirem ja tõhusam
    -   PackedStringArray, PackedInt32Array jne
-   Dictionary
    -   konteiner, kus väärtustel on indeksite asemel võtmed
-   Signal
    -   ka signaali võib muutuja väärtuseks määrata (eelnevalt mainitud)
-   Callable
    -   ka funktsiooni võib muutuja väärtuseks määrata (eelnevalt mainitud)

## Klassid

On kaks võtmesõna klasside jaoks, `class` ja `class_name`.
`class` on tavaline klass, mis on ligipääsetav ainult tema skriptiga ühendatud sõlme kaudu.
`class_name` deklareerib uue klassi, mis on nähtav sõlmede loetelus. Seda kasutatakse siis, kui üht ja sama klassi soovid taaskasutada. Seda võtmesõna võib vaid kord ühes skriptifailis kasutada.
Näiteks on siis järgnev võimalik:
```gdscript
extends Node2D

class_name MinuKlass

var taisarv: int = 5

class KlassMinuKlassis:
    var komaarv: float = 0.5

    func mis_on_komaarv() -> float:
        return self.komaarv
```

Saad teises skriptifailis nendele klassidele niimoodi ligi:

```gdscript
var minu_klass: MinuKlass = MinuKlass.new()
var klass_minu_klassis: MinuKlass.KlassMinuKlassis = MinuKlass.KlassMinuKlassis.new()
```

Tegelikult, kui klassi instantsi alles lood, siis võid lasta kompileerijal ka andmetüüpi lihtsalt järeldada. Sedasi kirjutad vähem koodi.

```
var minu_klass:= MinuKlass.new()
var klass_minu_klassis:= MinuKlass.KlassMinuKlassis.new()
```

Pane tähele, et on kasutatud nii : (koolon) kui ka = (võrdusmärk) kirjamärke.

## Staatilised muutujad ja funktsioonid

Võtmesõna `static` saab kasutada nii muutuja kui ka funktsiooni kirjutamisel.
Staatiline muutuja hoiab oma väärtust läbi klassi erinevate instantside, kuid seda saab ikka muuta.
Näide:

```gdscript
extends Node

class NaiteKlass:
    static var arv: int = 10

var a: NaiteKlass = NaiteKlass.new()
var b: NaiteKlass = NaiteKlass.new()

func _ready():
    print(a.arv) # 10
    print(b.arv) # 10
    a.arv = 15
    print(a.arv) # 15
    print(b.arv) # 15
```

Staatilise funktsiooni jaoks ei pea klassi instantsi looma, saad selle lihtsalt välja kutsuda.

{: .todo }
Tsüklid, tingimuslaused, võtmesõnad