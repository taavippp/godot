---
title: GDScript
layout: default
nav_order: 4
has_children: false
---

# GDScript

## Tühikud

Selles osas tutvume lähemalt GDScripti erinevate osadega. Selles peatükis oled juba natuke õppinud seda Godot'le loodud keelt, aga nüüd saad teada, mida täpsemalt GDScriptiga teha saab.

GDScript on süntaksi poolest kohati sarnane Python keelele - mõlemal keelel on iga rea taane oluline.
Esimene funktsioon siin koodiplokis annaks veateate, teine mitte.

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

| Andmetüüp | Tähendus                    |
|-----------|-----------------------------|
| null      | tühi/puuduv väärtus         |
| bool      | tõene või väär, ehk 1 või 0 |
| int       | täisarv                     |
| float     | ujukomaarv                  |
| String    | tekst                       |

Lisaks võtmesõna `void` kasutatakse, kui funktsioon ei peaks mingit väärtust tagastama.

## Funktsioonid, tsüklid, tingimuslaused

Funktsioone on võimalik deklareerida `func` võtmesõnaga.s
GDScriptis on võimalik luua nii `while` kui ka `for` tsükleid.
Tingimuslausete jaoks on võtmesõnad `if`, `elif`, `else` ja `match`.
`match` on sarnane teistest keeltest `switch` võtmesõnale, aga paindlikum.
Näiteks:

```gdscript
func _ready() -> void:
    var num_1: int = 10
    var num_2: int = 5
    
    # muutuja i eksisteerib siin ainult for-tsükli ajal
    for i in num_1:
        print(i)
    
    # siin loodud muutuja i ei ole for-tsükli omaga seotud
    var i: int = 0
    while i < num_2:
        print(i)
        i += 1
    
    if i == 4:
        print("neli")
    elif i == 5:
        print("viis")
    else:
        print("midagi muud")
    
    match num_1:
        5:
            print("viis")
        7:
            print("kümme")
        [8, 9, 10]:
            print("arv on kas 8, 9 või 10")
```

## Konteinerid

GDScriptis on erinevad konteinerid mitme ühte andmetüüpi väärtuste hoidmiseks.

-   Array
    -   massiiv
    -   Godot 4. versioonist alates on neile võimalik andmetüüpe määrata nii: `Array[tüüp]`, nt `Array[int]` või `Array[Node2D]`
-   Packed Array
    -   kuna tavaline massiiv on loodud igasuguseid andmetüüpe ja klasse sisaldama, siis suurte andmekogustega tegelemiseks on mõne andmetüübi jaoks olemas PackedArray, millega opereerimine on palju kiirem ja tõhusam
    -   PackedStringArray, PackedInt32Array jne
-   Dictionary
    -   sõnastik-konteiner, kus väärtustel on indeksite asemel võtmed
-   Signal
    -   ka signaali võib muutuja väärtuseks määrata (eelnevalt mainitud)
-   Callable
    -   ka funktsiooni võib muutuja väärtuseks määrata (eelnevalt mainitud)

## Võtmesõnad ja operaatorid

### Võtmesõnad

Lisaks teistele siin lehel mainitud võtmesõnadele eksisteerivad veel:

-   `break` & `continue`
    -   `break` on tsükli lõpetamiseks
    -   `continue` on tsükli iteratsiooni vahele jätmiseks
-   `is`
    -   tingimuslauses klassi kontrollimiseks
    -   näide: `a is Label`
-   `in`
    -   tingimuslauses kontrollimiseks, kas väärtus on stringis/massiivis/sõnastikus/sõlmes
-   `as`
    -   määra tundmatule väärtusele andmetüüp
    -   näide: `var tegelane := tegelase_stseen.instantiate() as CharacterBody2D`
    -   kui tundmatule väärtusele see andmetüüp ei sobi, on väärtus null
-   `self`
    -   viide praegusele klassi instantsile
-   `signal`
    -   signaali deklareerimiseks
    -   näide: `signal liikus_paremale`
-   `breakpoint`
    -   peatab programmi seal real, kus see on kirjas
    -   pead redaktorist nupule vajutama, kui tahad programmi tööd jätkata
-   `await`
    -   peatab skripti töö kuni saab signaali või kaasrutiin lõpeb
-   `assert`
    -   kui talle antud tingimus on vale, siis programm annab veateate
-   konstandid
    -   `PI`
    -   `TAU`
    -   `INF`
        -   lõpmatus
    -   `NAN`
        -   võimatu number

### Operaatorid

Tehete tegemiseks on saadaval operaatorid `+`, `-`, `*`, `/`, `%` ja `**`.
`%` annab tulemuseks jagamise jäägi. `**` on astendamise operaator.

Tehete operaatoritele saab lisada ette `=`, et määrata tehte tulemus muutuja väärtuseks.

Võrdluste tegemiseks on olemas `==`, `<`, `>`, `<=`, `>=` ja `!=` operaatorid.
On olemas võtmesõnad `not`, `and` ja `or`.

## Klassid

On kaks võtmesõna klasside jaoks, `class` ja `class_name`.

`class`iga deklareeritud klass on ligipääsetav vaid skriptifaili kaudu, mis tähendab, et ta on selle skripti alamklass.

`class_name` deklareerib uue klassi, mis on nähtav sõlmede loetelus.
Seda võtmesõna võib vaid kord ühes skriptifailis kasutada.

Näiteks on siis järgnev võimalik:

```gdscript
extends Node2D

class_name ShapeFactory

class Circle:
    var radius: float = 1

    func area() -> float:
        return this.radius ** 2 * PI

class Square:
    var side: float = 1

    func area() -> float:
        return this.side ** 2
```

Saad teises skriptifailis nendele klassidele niimoodi ligi:

```gdscript
var factory: ShapeFactory = ShapeFactory.new()
var circle: ShapeFactory.Circle = factory.Circle.new()
```

Tegelikult, kui klassi instantsi alles lood, siis võid lasta kompilaatoril ka andmetüüpi lihtsalt järeldada. Sedasi kirjutad vähem koodi.

```
var factory := ShapeFactory.new()
var circle := factory.Circle.new()
```

Pane tähele, et on kasutatud nii koolonit (:) kui ka võrdusmärki (=).

### Enumeraator ehk loenditüüp

Võtmesõnaga `enum` on võimalik deklareerida konstantsete väärtustega struktuur, kus konstantidel on automaatselt väärtused antud.  Vajadusel saad ka ise väärtuse määrata.

Tegutseme veel edasi ShapeFactory klassis:

```gdscript
# Nimetu loenditüüp
enum {
    CIRCLE,
    SQUARE,
    RECTANGLE,
    TRIANGLE
}

# Nimega loenditüüp
enum Direction {
    LEFT = -1,
    RIGHT = 1,
}
```

Muus skriptifailis saab nüüd nendele väärtustele nii ligi:

```gdscript
func _ready() -> void:
    var shape = ShapeFactory.RECTANGLE
    var direction:= ShapeFactory.Direction.RIGHT
```

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
    NaiteKlass.arv = 20
    print(a.arv) # 20
    print(b.arv) # 20
```

Staatilise funktsiooni jaoks ei pea klassi instantsi looma, saad selle lihtsalt välja kutsuda.

## *Singleton* muster läbi *autoload*-skriptide

Kui leiad, et skriptifail peaks olema teistest skriptifailidest globaalselt juurdepääsetav, aga ei taha luua eraldi klassi selleks, võid kasutada *autoload* funktsionaalsust. Sellega luuakse programmi avades *singleton*-tüüpi skripti instants, mis tähendab, et vaid üks globaalne koopia sellest eksisteerib.

Autoloadi saab luua Godot redaktoris ülaribalt nupult Project -> Project Settings. Siis avaneb sinu projekti konfigureerimise aken, kus on vaheleht `Globals`. Siin on võimalik teha olemasolev skript autoloadiks või luua uus, mis on koheselt autoload.

![Autoloadi loomine](./pildid/autoload.png)

Järgmises osas alustame uue peatükiga, kus loome Godot 2D füüsika mootoriga mängu.