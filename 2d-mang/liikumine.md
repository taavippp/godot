---
title: Liikumine
layout: default
parent: 2D mäng
nav_order: 2
---

# Liikumine

{: .todo }
See peatükk on töös!

-	Tee kindlaks kuidas ma tahan sprite pöörata

## Liikumise skript

Loo peategelase jaoks skript nimega `peategelane.gd`. Tegelasel on vaja liikumiseks teada tema maksimumkiirust ja liikumissuunda. Seega esimesed read, mis võiks skripti juurde kirjutada, on:

```gdscript
@export_range(0.0, 500.0) var max_speed: float = 200.0
var direction: float = 1.0
```

`@export_range` annotatsioon on sarnane tavalise `@export` annotatsiooniga, aga võimaldab muutujale piiride määramist. Kuna meil on kiirus ja liikumissuund eraldi muutujad, siis kiirus ei tohi alla 0 minna, sest tegelane hakkaks vastassuunas liikuma. Suund ei pea eksportmuutuja olema, kuna see muutub pidevalt tegelase liikudes. Lisaks jääb suuna väärtus -1 ja 1 vahele.

Kustuta `_ready` funktsioon. Kirjutame `_process` funktsiooni tegelase algelise liikumise loogika. Esiteks peame nüüd tegelase liikumissuuna määrama. Seda saame teha funktsiooniga `Input.get_axis(negative_action, positive_action)`. Kuna `action` tähendab tegevust, siis argumentideks saavad tegevuste nimed. Meie `negative_action` olgu `move_left` ja `positive_action` olgu `move_right`. Negatiivne tegevus tähendab siin seda, et tegelane liigub X-telje negatiivsel suunal ja positiivne tegevus vastupidist.

Tegelase liikumine sõltub suunast ja kiirusest, seega nüüd määrame CharacterBody2D `velocity.x` väärtuseks `direction * max_speed`. Lisaks, et tegelane peale `velocity` määramist lõpuks liikuma hakkaks, peame `_process` lõppu lisama käsu `move_and_slide()`. Võid nüüd oma mängu tööle panna ülariba nupust `Run Current Scene (F6)` ja veenduda, et tegelase liikumist on akna ülaosas näha, kui nooleklahve vajutad.

Lisa peategelasele juurde kaamera sõlm `Camera2D` ja määra selle `Zoom` väärtuseks `(4, 4)`. Niimoodi näeme pikslikunsti paremini.

## Kollisioonide süsteem

Said eelmises alapeatükis teada, et füüsika kehad töötavad CollisionShape2D sõlmede abil. Mäng kontrollib, kas kaks keha on kokku põrganud omavahel ja kohandab neid ringi, et nad enam ei põrkaks kokku. Füüsika kehad saavad olla erinevatel kihtidel, et nad üldse kokku ei põrkaks. Neil on muutujad `Collision Layer` ja `Collision Mask` - *layer*i alla saab märkida need kihid, kus füüsika keha on ja *mask*i alla need kihid, millega see füüsika keha peaks kokku põrgata suutma.

Meie projektis kasutame nelja erinevat füüsika kihti:

1.	*map*
2.	*player*
3.	*enemy*
4.	*projectile*

Füüsika kihtidele saame nimed anda Project Settings -> General -> Layer Names -> 2D Physics alt.

Peategelane peaks olema siis *player* kihil ja tema mask peaks tuvastama *map* ja *projectile* kihte. Neid saad määrata CollisionObject2D -> Collision alt.

## Hüppamine

Nüüd õpetame oma tegelase hüppama. Selleks, et ta hüpata saaks, peame talle algul gravitatsiooni tutvustama. Loo uus muutuja nimega `gravity`, mille väärtus on näiteks 25. Gravitatsioon on meie mängu maailmas kiirenev positiivne liikumine Y-teljel. Seega lisa enne `move_and_slide` käsku rida, kus määrad `velocity.y` väärtuse. Iga kaader lisatakse sellele gravitatsiooni jõud uuesti juurde, seega kirjutatav rida olgu `velocity.y += gravity`.

Hüppamiseks on vaja uut eksportmuutujat nimega `jump_strength`. See võiks olla näiteks 0 - 1000 vahemikus ja vaikimisi väärtuseks panin 400. Selleks, et tegelane hüppaks, peab ta puutuma maapinda ja hüppamise tegevus peab just olema toimunud. Hüppamine on gravitatsiooni vastand - ühekordne negatiivne Y-telje jõud.

Meie tegelane hüppab siis järgnevate koodiridadega:

```gdscript
if (is_on_floor() and Input.is_action_just_pressed("jump")):
	velocity.y = -jump_strength
```

Kui nüüd selle stseeni käivitad, tegelane liigub ja kukub igavesti, sest tal pole maapinda all. Enne maapinna loomist kasutame ära need animatsioonid, mis me lõime.

## Animatsioonid tööle

Meie AnimatedSprite2D sõlmel on 3 animatsiooni: "default", "run" ja "jump". Tead nüüd, et funktsioon `is_on_floor()` tagastab tõeväärtuse. On vaja ka kontrollida suuna muutuja abil, kas tegelane liigub. Kui tegelane ei liigu, on suund 0.

Lisaks peame muutma tegelase spraidi suunda, et mängijal oleks visuaalne tagasiside oma liikumisest. `AnimatedSprite2D` sõlmel on selleks `flip_h` muutuja.

```gdscript
extends CharacterBody2D

@export_range(0, 500, 10) var max_speed: float = 200.0
@export_range(0, 1000, 10) var jump_strength: float = 400.0

@export var sprite: AnimatedSprite2D

var gravity: float = 25.0
var direction: float = 0.0

func _physics_process(delta: float) -> void:
	direction = Input.get_axis("move_left", "move_right")
	velocity.x = direction * max_speed
	velocity.y += gravity
	if (is_on_floor() and Input.is_action_just_pressed("jump")):
		velocity.y = -jump_strength
	
	if (is_on_floor()): # kas tegelane on maas
		if (is_zero_approx(direction)): # kas suund on 0
			sprite.play("default")
		else:
			sprite.play("run")
	else:
		sprite.play("jump")
	
	move_and_slide()
```

Video pooleli 14:20 sprite flipimise juures
