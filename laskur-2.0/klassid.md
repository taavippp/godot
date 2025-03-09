---
title: Klassid
layout: default
parent: Laskur 2.0
nav_order: 1
---

See peatükk on töös!
{: .todo }

-	Githubi repo link #klassid alla
-	sounds peab olema vist juba eelnevalt lisatud, kui ma yhte zip faili koik varad panen
-	Entity klassi loomine ylesanne?

# Klassid

Kuna tegu on eelmise peatüki projekti edasiarendusega, võid juba loodud Laskuri projekti taas avada. Kui pole Laskuri projekti loodud, soovitan eelmise peatüki läbi võtta või GitHubi repositooriumist see alla laadida.

Selle peatüki põhiosa on kirjutada oma klass nimega Entity (olemus), mis laiendab CharacterBody2D klassi. Selle klassi eesmärk on lihtsustada mängu erinevate tegelaste (Player, Crawler ning ühe teeme veel) loomist ning nendega töötamist. Lisaks on olemustel elupunktid (*health points*).

## Organiseerimine

Praegu peaks failide dokis väikene segadus olema. Enne oma klassi loomist korrastaks neid veidi.

Uusi kaustasid/faile saad failide doki kaudu luua **parema hiire klõpsuga**.

Stseeni fail ja vastav skripti fail võiksid samas kaustas olla. Olemuste jaoks võiks luua `entities` kausta, kus igal olemusel on veel oma kaust. Kuul ei ole olemus, sest kuulil ei tohi elupunkte olla. See tähendab, et kuul läheb oma eraldi kausta nimega `projectiles`. Loome veel ühe kausta nimega `classes`, kus hakkavad olema meie uute klasside skriptifailid.

Mina organiseerisin oma failid lõpuks niimoodi:

```
res://
├── classes
├── entities
│   ├── crawler
│   │   ├── crawler.tscn
│   │   └── crawler.gd
│   └── player
│       ├── player.tscn
│       └── player.gd
├── projectiles
│   ├── bullet.tscn
│   └── bullet.gd
├── sounds
│   ├── jump.wav
│   └── shoot.wav
├── sprites
│   └── tilemap.png
├── export_presets.cfg
├── icon.svg
├── level.tscn
├── main.gd
└── main.tscn
```

Fail `export_presets.cfg` tekkis, kui eelmises peatükis oma mängu eksportisime.

## Entity klass

Loo siis `classes` kausta uus skriptifail nimega `entity.gd`. Deklareeri uus klass nimega Entity kirjutades skripti algusesse `class_name Entity extends CharacterBody2D`.

Selle klassi loome selle eesmärgiga, et vältida koodis kordusi ja et kõik olemused järgiksid sama standardit. Meie Player olemusel ja Crawler olemusel on ühist järgmised omadused:

-	`speed` muutuja
-	`gravity` muutuja
-	`direction` muutuja

Lisaks kirjutame juurde veel täisarvulise eksportmuutuja `max_health`, mille väärtus olgu vahemikus 1 - 10 ja tavalise täisarvulise muutuja `health`, mis `_ready()` funktsioonis saab väärtuseks `max_health`. Meie tegelased peavad siis suutma oma elusid ka kaotada. Loome funktsiooni `take_damage()`, mille kutsumisel elupunktid langevad ühe võrra ning kui nad on nullis (või alla selle), siis see sõlm kustutab end.

Kood võiks umbes selline välja näha:

```gdscript
class_name Entity extends CharacterBody2D

@export_range(0, 500, 10) var speed: float = 200.0
@export_range(1, 10) var max_health: int = 1

var gravity: float = 25.0
var direction: float = 1.0
var health = 1

func _ready() -> void:
	health = max_health

func take_damage() -> void:
	health = health - 1
	if (health <= 0):
		queue_free()
		return
```

## Koodi kaudu animeerimine

Tahame, et kui olemus elupunkte kaotab, siis oleks seda visuaalselt korraks kuidagi näha. Selleks õpime animeerima kasutades Tween süsteemi. Tweenidega animeerimine töötab sujuvalt muutujate väärtuseid muutes. Tweenid hakkavad automaatselt tööle.

Meie kasutame oma animatsiooni jaoks `modulate` muutujat, mis on värvifilter sõlmede peal. See on vaikimisi valget värvi, mis tähendab, et sõlmede värvid ei ole muudetud. Meie muudame animatsiooni alguses selle läbipaistvaks ning animatsiooni jooksul taas valgeks, mis tekitab korraks haihtumise efekti.

Lisa `take_damage()` funktsiooni lõppu (peale elupunktide kontrolli) järgnevad koodiread:

```gdscript
	... (muu kood)
	var tween: Tween = create_tween() # loob Tweeni
	tween.tween_property(
		self, # stseeni juursõlm
		"modulate", # modulate muutuja
		Color.WHITE, # lõpuks on modulate värv valge
		0.5 # võtab pool sekundit
	).from(Color.TRANSPARENT) # alustame läbipaistvast värvist
```

Tegelikult saaks ka selleks animatsiooniks kasutada AnimationPlayer sõlme, aga see võtaks rohkem tööd ja selle kordamist.
{: .tip }

Video jätkab 05:11, paneme praegused olemused kasutama Entity klassi