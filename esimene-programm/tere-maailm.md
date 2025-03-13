---
title: Tere maailm
layout: default
parent: Esimene programm
nav_order: 2
---

# Tere maailm

Selles osas tutvume lähemalt sõlmede ja signaalide süsteemiga ning kirjutame esimesed GDScripti (Godot enda programmeerimiskeel) koodiread.

## Esimene sõlm

Leia üles koht stseeni dokis, kus on kirjas *Create Root Node* ehk "loo juursõlm". Vajuta selle all olevale `2D Scene` nupule. See loob sinu stseeni Node2D sõlme, millel on algelised 2D funktsionaalsused ja mõned muud asjad olemas. Põhivaade lülitus ka automaatselt 2D režiimi. Heida pilk inspektori dokki.
Näed, et see jaguneb kolmeks põhiosaks:

1.  Node2D
2.  CanvasItem
3.  Node

Sellest struktuurist loed hiljem täpsemalt. Vajuta inspektoris Node2D all olevale `Transform` nupule, et näha Node2D unikaalseid omadusi. Näed, et sellel sõlmel on omadused nagu *position* ehk positsioon teljestikus ja *rotation* ehk pööre.

![Node2D omadused, nähtavad inspektori dokist.](./pildid/tere-maailm/node2d-inspektoris.png)

Liigu tagasi stseeni dokki ja vajuta oma loodud Node2D sõlme peal. Tema kohale tekkis paberilehe disainiga nupp, mis loob ja ühendab selle sõlme külge uue skriptifaili. Vajuta selle peale.

![Kuidas luua skriptifaili.](./pildid/tere-maailm/loo-voi-muuda-skripti.png)

Avaneb uus aken. Redaktor küsib, mis keeles tahad oma skriptifaili kirjutada, mis **klassi** pärija see skriptifail on ja kuhu failisüsteemis Godot skriptifaili salvestab. Kasutame ikka GDScript keelt ja skriptifail on Node2D pärija. Võid skriptifaili nime muuta näiteks `tere.gd`-ks. Vajuta `Create`, et see fail luua ja skripti kirjutamise vaatesse minna.

![Skriptifaili loomise detailid.](./pildid/tere-maailm/loo-skript.png)

## Esimesed koodiread

Skripti vaates peaks juba järgnev kood automaatselt seal olema.

```gdscript
extends Node2D

# Called when the node enters the scene tree for the first time.
func _ready():
	pass # Replace with function body.

# Called every frame. 'delta' is the elapsed time since the previous frame.
func _process(delta):
	pass
```

Võtmesõna `extends` täpsustab, mis klassi pärija su skript on. Iga skript Godot's on uus klass. `tere.gd` fail tähendab siis, et lõid klassi nimega "tere", mis on kättesaadav ainult siis, kui selle skripti mingi sõlme külge ühendad. See tähendab, et eelnevalt mainitud Node ja CanvasItem on klassid, mille pärija on Node2D klass.

```mermaid
%%{init: {'theme':'neutral'}}%%
flowchart TD;
	node["Node klass"];
	canvasitem["CanvasItem klass"];
	node2d["Node2D klass"];
	tere["Tere klass (tere.gd)"];
	node --> canvasitem;
	canvasitem --> node2d;
	node2d --> tere;
```

Sümboliga *#* algav rida tähistab kommentaari. See teeb terve rea kommentaariks, ehk seda ei loeta koodina, kuid arendaja saab siia kirjutada kasulikku informatsiooni koodi kohta.

Võtmesõna `func` kuulutab funktsiooni algust. Funktsioon `_ready()` on Node klassi sisse ehitatud funktsioon, mis käivitub siis, kui sõlm on valmis stseenis töötama. Tal on ees alakriips, et märgistada seda funktsiooni privaatsena. Muidu tegelikult GDScript-is **ei ole** võtmesõnu `private` ega `public`, ehk kõik funktsioonid on avalikud.

Käsk `pass` tähendab, et see koodirida ei tee mitte midagi.

Funktsioon `_process()` käivitub igal kaadril, mida programm renderdab. Tal on parameeter `delta`, mis tähistab aega sekundites eelmise kaadri möödumisest.

Kirjuta funktsioonis `_ready()` käsu `pass` asemele `print("Tere maailm!")`. Vajuta `CTRL + S` klahve, et salvestada oma tehtud töö. Tuleb ette stseeni salvestamise aken. `Save` nupu kohal võid stseeni nimeks panna näiteks `tere.tscn`. Vajuta siis `Save` nuppu või `Enter` klahvi, et stseen salvestada. Skriptifail on ka automaatselt salvestatud.

Nüüd käivitame oma projekti, sest tahame näha, kuidas "Tere maailm!" väljastatakse. Tööriistariba paremal pool on kolmnurga kujuline nupp `Run Project`. Sellele vajutades palub redaktor meil põhistseeni valida. Vajuta nupu `Select Current` peale.

![Projekti käivitamise nupp](./pildid/tere-maailm/projekti-kaivitamine.png)

Avaneb uus aken halli taustaga. Maailma tervitust siin ei ole. Selle leiad hoopis redaktori alumiselt ribalt `Output` osast.

![Output'i all on kirjutatud sõnum.](./pildid/tere-maailm/tere-maailm-konsoolis.png)

Tahaks ikka oma rakenduse aknas sõnumit näha. Praeguseks sulge see.

## Label sõlm

Selleks, et oma kirjutatud teksti projektis näha, peame ühte teist sõlme kasutama. Vajuta stseeni dokis Node2D peale ja kustuta see kas `Delete` klahviga või paremklõpsu menüüst leides `Delete Node(s)` valiku. Kinnita, et tahad seda kustutada.

Seekord vajuta plussmärgi-kujulisele nupule või klaviatuuril `CTRL + A`. Avaneb uus aken, mis pakub sulle kõiki sõlmi, mis saadaval on. Kirjuta ülemises osas `Search` otsingulahtrisse sõna "label". Pakutakse sõlme Label. Vajuta akna allosas olevat nuppu `Create`, et see luua.

Vajuta tööriistaribal nupu 2D peale, et tagasi liikuda 2D vaatesse. Uuel Label sõlmel pole enam `tere.gd` skriptifaili küljes. Seda teeme hiljem. Vali oma loodud sõlm stseeni dokis ja heida pilk taas inspektorisse ja näed kohe, et Label sõlmel on omadus Text, kuhu saad oma sõnumi kirjutada. Kirjuta tühja tekstikonteinerisse oma sõnum.

![Label sõlm näitab teksti redaktoris.](./pildid/tere-maailm/label-kuvab-teksti.png)

Kui nüüd projekti uuesti tööle paned, siis avanevas aknas ongi üleval vasakul nurgas näha sinu kirjutatud sõnum. Pane tähele, et kuna `tere.gd` pole Labeli külge ühendatud, siis Outputi ei saadetud enam sinu sõnum.

Järgmises osas teeme nii, et kuvatavat teksti saab muuta teiste sõlmede abil ja õpime signaale kasutama.