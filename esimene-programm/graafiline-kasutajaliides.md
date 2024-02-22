---
title: Graafiline kasutajaliides
layout: default
parent: Esimene programm
nav_order: 3
---

# Graafiline kasutajaliides

## Sõlmed

Selles osas on eesmärk eelnevas osas tutvustatud Label sõlm panna tervitama kasutaja sisestatud nime. Selleks kasutame paari uut sõlme ja õpime veidi Godot graafilise kasutajaliidese süsteemi.
Kustuta praegune Label sõlm ja määra juursõlmeks hoopis Control. Lisa Control laps-sõlmedeks Label, TextEdit ja Button.

![Stseeni struktuur.](./pildid/graafiline-kasutajaliides/stseeni-struktuur.png)

Stseen peaks olema sedasi üles ehitatud.
{: .text-center .fs-3}

Control sõlm on Godot graafilise kasutajaliidese sõlmede alustala. Label, TextEdit ja Button on Control sõlme pärijad.
Label sõlme eesmärk ongi teksti kuvamine. Leidub veel sõlm RichTextLabel, mis suudab Rich Text formaadis teksti kuvada.
TextEdit sõlme otstarve on nimest tuletatav - kasutaja saab sinna sisse teksti kirjutada.
Ka Button sõlme ülesanne on selge, see on lihtsalt nupp, mida saab hiirega vajutada.

Tahame teha nii, et teksti sisestamise riba ja nupp oleksid programmi allpool ja tervitust kuvatakse keskel.

## Ankrud

Graafilise kasutajaliidese sõlmed arvestavad erinevate mängu akna suurustega kasutades ankrute süsteemi. On neli ankrut - ülemine, alumine, vasak, parem. Ankru väärtus on tavaliselt 0 ja 1 vahel. Kui vasaku ankru väärtus on 0, tähendab see, et sõlme vasak külg alustab ekraani vasakust küljest. Kui see oleks 0.5, algaks sõlme vasak külg ekraani keskelt. Kui see oleks 1, algaks sõlme vasak külg ekraani paremast küljest.

Vali Control sõlm tema peale klikkides ja inspektoris pane Control -> Layout -> Anchors Preset muutuja väärtuseks mall `Full Rect`. See teeb Control sõlme mängu akna suuruseks ja sätib ankrute väärtused järgmiseks:

-   vasak ankur: 0 (sõlm algab ekraani vasakust äärest)
-   ülemine ankur: 0 (sõlm algab ekraani ülemisest äärest)
-   parem ankur: 1 (sõlm lõpeb ekraani paremas ääres)
-   alumine ankur: 1 (sõlm lõpeb ekraani alumises ääres)

Kuna tahame tervet mängu akna ala kasutada, siis Control sõlmele see sobib. Tahtsime, et Label oleks ekraani keskel. Selleks otsi üles Control -> Layout -> Layout Mode muutuja ja tee kindlaks, et tema väärtus oleks `Anchors`. Seejärel ilmub Anchors Preset muutuja taas selle alla. Milline väärtus sealt sobiks Labeli ekraani keskele sobitamiseks?

Vastus on loomulikult `Center` - see hoiab Label sõlme nii horisontaalselt kui ka versikaalselt keskel. Kontrollimiseks võid Labelile jälle teksti `Tere maailm!` kirjutada. Pane tähele, et teksti juurde kirjutades ja ära kustutades püsib sõlm ikka ilusti ekraani keskel, kuigi tema suurus muutub. Tee sama Button sõlmega, aga aseta see hoopis paremasse alumisse nurka (väärtus `Bottom Right`). Tee ka nii, et see kuvaks teksti "Tervita". Saad seda teha samamoodi, nagu Label sõlmega.

TextEdit sõlmel pane muutuja Placeholder Text väärtuseks `Keda tahad tervitada?`. Kui kasutaja teksti sinna kirjutanud pole, siis seda sõnumit kuvatakse. Seejärel tegeleme TextEditi ankrutega. Pane temal Anchors Preset väärtuseks `Custom`. See paljastab palju uusi muutujaid täpsemalt ankrutega tegelemiseks. Tahame, et TextEdit oleks alumises ääres ja läheks nupuni välja. Seda saad teha järgmiste väärtustega:

-   Anchor Points -> Left: 0 (vasak ankur)
-   Anchor Points -> Top: 1 (ülemine ankur)
-   Anchor Points -> Right: 0.9 (parem ankur)
-   Anchor Points -> Bottom: 1 (alumine ankur)

Meie asendusteksti pole aga näha. Selleks pead Anchor Offsets -> Top väärtuseks panema näiteks -35 pikslit. Põhivaade peaks nüüd selline välja nägema:

![Stseeni lõplik välimus.](./pildid/graafiline-kasutajaliides/stseeni-visuaal.png)

## Kood

### Muutujad

Vali Control juursõlm ja ühenda talle taas Tere.gd külge. Skripti ühendamise aken peaks mainima, et fail juba eksisteerib ning et seda taaskasutatakse. Vajuta nupu `Load` peale.

![Skripti ühendamise aken kirjutab, et skriptifail on juba olemas.](./pildid/graafiline-kasutajaliides/fail-eksisteerib.png)

Praegu peaks skriptifail olema selline:

```gdscript
extends Node2D

# Called when the node enters the scene tree for the first time.
func _ready():
	print("Tere maailm!")

# Called every frame. 'delta' is the elapsed time since the previous frame.
func _process(delta):
	pass
```

Kohe esimesel real on viga. Kuna nüüd on juursõlm Control tüüpi, siis peaks kirjas olema hoopis `extends Control`. Järgmiseks on meil vaja ligi pääseda skriptist Label, TextEdit ja Button sõlmedele. Selleks on Godot's kaks viisi:

1.	`@export` annotatsiooniga muutujad
2.	`@onready` annotatsiooniga muutujad

Esiteks tutvume sellega, kuidas deklareerida muutujaid.
Üldine muutuja süntaks on järgmine: `var muutuja_nimi: muutuja_klass = väärtus`. Muutujatel ei pea klassi kirja panema, aga seda tehakse sellel kursusel, kuna see on hea praktika. Muutujatel ei pea ka väärtuseid kohe määrama. Lisaks on olemas võtmesõna `const` konstandi deklareerimiseks samal viisil. Konstantidel peab olema väärtus koodis kohe olemas. Funktsiooni tagastatava väärtuse andmetüüpi on ka mõistlik deklareerida järgneva süntaksiga: `func funktsiooni_nimi(argument: argumendi_klass) -> tagastatav_klass:`.

Annotatsioonid on @-märgiga algavad märgistused. Need eelnevad muutuja deklaratsioonile ja annavad sellele muutujale erinevaid omadusi juurde. Näiteks `@export` annotatsioon teeb muutuja inspektoris nähtavaks ja saad sealt talle väärtuse määrata. `@onready` annotatsioon määrab muutujale väärtuse alles siis, kui skriptiga ühendatud sõlme `_ready` funktsioon on käivitunud ehk tema ja ta laps-sõlmed on stseenide puusse sisestatud.

Label sõlmele saaks skriptist siis mõlemat viisi sedasi ligi:

```gdscript
# get_node funktsioon hakkab skriptiga ühendatud sõlme laste seast sellise nimega sõlme otsima
# nimi peab ühtima stseeni dokis oleva nimega
# $ märk on lühend get_node funktsiooni jaoks.
@onready var label_onready_a: Label = get_node("Label")
@onready var label_onready_b: Label = $Label

# pead inspektori dokis talle ise väärtuse määrama
@export var label: Label
```

Parem praktika on kasutada `@export` annotatsiooni, kuna selleks ei pea kirjutama koodis välja sõlme nime, mis võib hiljem muutuda. Seega kasutame sellel kursusel `@export`iga muutujaid. Proovi nüüd samas vormis `@export var label: Label` muutujale deklareerida TextEdit ja Button sõlmede muutujad. Skripti algus peaks nüüd selline välja nägema:

```gdscript
@export var label: Label
@export var text_edit: TextEdit
@export var button: Button
```

Lisaks on inspektori dokis ilmunud sektsioon nimega Tere.gd, kus on skripti `@export` annotatsiooniga avalikustatud muutujad. Pane tähele, et kuigi kirjutasid koodis `text_edit`, kuvatakse seda inspektoris kui Text Edit. Redaktor üritab muutuja nimed inspektoris loetavamaks teha. Vajutades nupule `Assign...` avaneb uus aken, kus on kujutatud kõik sinu stseenis sobivad sõlmed, mis selle muutuja väärtuseks võivad sobida. Meil on sobivaid sõlmi iga muutuja jaoks vaid üks. Vali õige sõlm ja kas topelt-kliki selle peal või vajuta all olevat `OK` nuppu.

![@export annotatsiooniga muutujad on inspektoris nähtavad.](./pildid/graafiline-kasutajaliides/eksport-muutujad.png)

Muutuja erinevate omadustega saad toimetada kasutades järgnevat süntaksi: `muutuja.omadus` Näiteks Label sõlme muutujale Text saad ligi kirjutades `label.text`. Proovi nüüd `_ready` funktsioonis `print` käsu asemel Labeli kuvatavaks tekstiks panna "Tere Godot!". Tulemus peaks olema selline:

```gdscript
func _ready() -> void:
	label.text = "Tere Godot!"
```

Kui nüüd rakenduse käivitad, kuvatakse teksti "Tere Godot!". See tähendab, et oled kogu koodi korrektselt kirjutanud. Lisasime `_ready` funktsioonile ka tagastava andmetüübi, milleks on `void`, sest see funktsioon ei tagasta midagi.

Siinkohal ei pidanud palju arutlema Text muutuja andmetüübi üle, aga mis siis, kui sa ei tea, milline muutuja väärtus võib olla? Kasutada saaksid redaktoriga kaasnevat sõlmede dokumentatsiooni. Sellele saad ligi skripti kirjutamise alast üleval paremal olevast nupust `Search Help`. Siin saad otsida erinevate sõlmede ja nende muutujate järgi endale vajalikku informatsiooni. Enamikel sõlmedel on ingliskeelsed selgitused head ja aitavad iseseisvalt hakkama saada.
Lisaks järgmises osas võtame täpsemalt GDScripti, tema andmetüübid ja muud detailid läbi.

### Signaalid

Nüüd peame välja nuputama, kuidas skriptis teha nii, et Buttoni nupuvajutuse peale läheb TextEditisse kirjutatud tekst Label sõlme. Vali inspektoris Button sõlm ja seejärel liigu inspektori dokist sõlme/`Node` dokki.

![Sõlme doki nupu asukoht.](./pildid/graafiline-kasutajaliides/solme-dokk.png)

Kohe peaks näha olema Button sõlme erinevad signaalid, mida ta välja saadab. Meid huvitab signaal `pressed`, mis reageeribki Buttoni vajutuse peale. Signaali mingi sõlme skriptiga ühendamiseks on ka kaks viisi:

1.	kirjutad selle koodis välja
2.	teed seda graafilise kasutajaliidesega

Kursuse autori isiklik eelistus on esimene variant, kuna niikuinii peab signaaliga toimetamiseks koodi kirjutama. Õpime mõlemat tegema, aga edaspidi koodinäidetes on esimest varianti kasutatud. Esimese meetodiga näeks `_ready` funktsioon välja selline:

```gdscript
func _ready() -> void:
	button.pressed.connect(_on_button_pressed) # uus rida
	label.text = "Tere Godot!"

func _on_button_pressed() -> void:
	pass
```

Godot kolmandas versioonis ei saanud signaalidele ligi sedasi ja pidi jälle stringe kasutama, aga neljandas versioonis on see mugavamaks ja kindlamaks tehtud. Signaalidel on funktsioon `connect`, mille argumendiks on koodinäite lõpus deklareeritud `_on_button_pressed` funktsioon. Nii funktsioonidele kui ka signaalidele saab sedasi viidata neljandas versioonis. `connect` funktsioon teeb nii, et kui Buttonit vajutatakse, see laseb välja `pressed` signaali, mis on eelnevalt juba Control sõlmega ühendatud ja signaali peale kutsub Control sõlm välja oma funktsiooni `_on_button_pressed`.

Teine viis on sealsamas sõlme dokis topelt-klikkida `pressed` signaali peale. Seejärel avaneb aken, mis küsib detaile signaali sõlmega ühendamise kohta. Oluline on, et ta ühendaks Control sõlmega. Kõik muu võid jätta samaks. Seejärel vajuta nupule `Connect`. Skriptis on nüüd genereeritud algne `_on_button_pressed` funktsioon, aga signaali ühendusest skriptis endas tõendit ei ole.

`_process` funktsiooni pole meil selles skriptis vaja, kuna ta ei tee igal kaadril midagi uut. Proovi nüüd `_on_button_pressed` funktsioon panna Outputi mingit sõnumit väljastama `print` käsuga, et kindlaks teha, et nupuvajutuse peale tõesti saab Control sõlm signaali kätte ja reageerib sellele.

Viimaseks asjaks pead nii tegema, et Label sõlm võtab TextEdit sõlme teksti ja kuvab seda. Seejärel tee nii, et TextEdit sõlmel pole enam teksti ning ta taas kuvab oma instruktsiooni.

Kogu skript peaks lõpuks olema järgnev:

```gdscript
extends Control

@export var label: Label
@export var text_edit: TextEdit
@export var button: Button

# Called when the node enters the scene tree for the first time.
func _ready() -> void:
	button.pressed.connect(_on_button_pressed)
	label.text = "Tere Godot!"

func _on_button_pressed() -> void:
	label.text = "Tere " + text_edit.text
	text_edit.text = ""
```

Nüüd on esimene projekt valmis. Käivita see, sisesta teksti ja vajuta nupule. Kui akna keskel kuvatav sõnum muutub, oled hakkama saanud! Lisaks peaks akna suurust muutes kõik sõlmed ilusti kaasa liikuma. Button on TextEditi sõlmest eespool, aga nende alad kattuvad väga väikse aknaga, sest Buttoni teksti suurus ei muutu.

![Akna suuruse muutmine töötab, kui ta selline saab välja näha.](./pildid/graafiline-kasutajaliides/akna-suuruse-muutmine-tootab.png)

Järgnevas peatükis lood 2D mängu ja õpid Godot 2D füüsika sõlmi ja paljut muud kasutama.