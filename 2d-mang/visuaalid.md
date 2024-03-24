---
title: Visuaalid
layout: default
parent: 2D mäng
nav_order: 3
---

# Visuaalid

Selles osas paneme tegelase animatsioonid tööle. Pärast võtame kasutusele TileMap sõlme, millega on mugav mängu tasemeid luua.

## Animatsioonid

Kui mäletad, lõime AnimatedSprite2D sõlme, millel juba panime animatsioonid paika. Nendeks olid: `climb`, `idle`, `jump` ja `run`. Nüüd on vaja see sõlm skriptifailis kasutusele võtta. Loo uus eksporditud muutuja `animated_sprite_2d` selle sõlme jaoks ja määra talle väärtus. Uus muutuja ilmub ilusti inspektoris, aga see, et tegelase füüsikaga ja visuaalidega seotud muutujad on koos, võib segadust tekitada. Kasutame annotatsiooni `export_group`, et grupeerida sarnase otstarbega ühe menüü alla. Selle annotatsiooni saab kasutusele võtta nii:

```gdscript
@export_group("Physics")

@export_range(100, 300, 10) var speed: int = 100
@export_range(1000, 2500, 25) var jump_force: int = 1000
@export_range(1, 25) var mass: int = 1
@export_range(100, 300, 10) var climb_speed: int = 150

@export_group("Nodes")

@export var animated_sprite_2d: AnimatedSprite2D
```

Nüüd on animatsioonidega sõlme võimalik koodis kasutada. Kui mäletad, siis kasutame `_physics_process` funktsiooni, sest seda kutsutakse välja maksimaalselt 60 korda sekundis. Animatsioonide puhul pole oluline, mitu korda sekundis nad uuenevad, seega võime kasutusele võtta `_process` funktsiooni. Kuna `_process` funktsioonis midagi muud peale animatsiooni loogika ei saa olema, siis ei pea animatsioonidega tegelemiseks eraldi funktsiooni looma.

### Ülesanne

Mõtle, mis kriteeriumite puhul mis animatsioon mängima peaks ja kirjuta vastav kood.
Selleks, et kontrollida, kas `velocity` x-koordinaat on 0, kasuta sisseehitatud funktsiooni `is_zero_approx` (kasutame seda, sest `velocity` kasutab ujuvkomaarve).
AnimatedSprite2D'l on funktsioon `play`, mis võtab argumendina animatsiooni nime.

Lahendusi on selle jaoks mitmeid, aga üks neist oleks järgmine:

```gdscript
func _process(delta: float) -> void:
	if (is_on_floor()):
		if (is_zero_approx(velocity.x)):
			animated_sprite_2d.play("idle")
		else:
			animated_sprite_2d.play("run")
	elif (is_climbing):
		animated_sprite_2d.play("climb")
	else:
		animated_sprite_2d.play("jump")
```

Kui käivitad mängu, tegelane teeb oma animatsioone küll, aga ta ei pööra ümber, kui liigud vasakule. Õnneks on AnimatedSprite2D'l olemas omadus `flip_h` (horisontaalne pööramine). Kirjuta juurde, et paremale liikumise puhul on `flip_h` väär ja vasakule liikumise puhul tõene.

```gdscript
func _process(delta: float) -> void:
    if (Input.is_action_pressed("movement_right")):
        animated_sprite_2d.flip_h = false
    elif (Input.is_action_pressed("movement_left")):
        animated_sprite_2d.flip_h = true
    # ...
```

Kui nüüd mängu käivitad, siis tegelane on animeeritud ja pöörab. Näeb päris vägev välja!

## TileMap sõlm

