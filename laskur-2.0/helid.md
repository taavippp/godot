---
title: Helid
layout: default
parent: Laskur 2.0
nav_order: 2
---

See peatükk on töös!
{: .todo }

# Helid

Selles alapeatükis loome juurde veel ühe vastase, kes liigub Tweeniga loodud animatsiooni abiga ja õpime Godot's helisid kasutama.

## Lendaja

Teeme juurde veel ühe vastase, kes lennata suudab. Loo uus stseen, kus juursõlmeks on Entity nimega `Flyer` (meie loodud klass). Järgime sama töövoogu, mis varem ja lisa juurde AnimatedSprite2D.

Sellel tegelasel on vaid **üks animatsioon**, mis **automaatselt** mängima hakkab. Kasutame ikka sama spraidilehte `tilemap.png`, millel oli **10 horisontaalset**, **6 vertikaalset** kaadrit ja **1-piksline kaadrite vahe**. Flyeri kaks kaadrit asuvad viimases reas, kollase putuka omade kõrval. Mina panin selle animatsiooni kaadrisageduseks 10 FPS.

![Flyeri animatsiooni detailid](./pildid/helid/flyer-animatsioon.png)

Lisa juurde CollisionShape2D, mille kujuks on RectangleShape2D suurusega (8, 12) ja Hitbox, mille CollisionShape2D kujuks on CircleShape2D raadiusega 6 pikslit. Salvesta stseen `entities` kausta nagu teistegi olemustega. **Ära unusta sõlmede füüsikakihid** - juursõlm on *enemy* kihil ja põrkab kokku *level*iga, Hitbox peab ainult *player* kihti tuvastama.

Nüüd oleks vaja ta liikuma panna. Loo juurde skript `flyer.gd`. Lendaja on kogu aeg õhus ja lendab vaikselt üles-alla. Kuigi ta kutsub `_process(delta)` funktsioonis välja ikka `move_and_slide()`, siis tema liikumise loogika läheb tegelikult `_ready()` funktsiooni. Skript tuleb välja selline:

```gdscript
extends Entity

const MAX_Y_VELOCITY: float = 24.0

@export var sprite: AnimatedSprite2D

func _ready() -> void:
	super() # Entity klassil on ka _ready funktsioon, super() kutsub seda
	sprite.flip_h = direction < 0.0
	var tween = create_tween()
	# paneb Tweeni animatsiooni korduma
	# vajaduse korral saab argumendiks arvu anda
	tween.set_loops()
	tween.tween_property(
		self,
		"velocity:y", # muudab velocity.y väärtust
		MAX_Y_VELOCITY,
		1.0 # võtab 1 sekund
	)
	# toimub peale eelmise tween_property lõppu
	tween.tween_property(
		self,
		"velocity:y",
		-(MAX_Y_VELOCITY),
		1.0
	)

func _process(delta: float) -> void:
	move_and_slide()
```

Nagu siit skriptist näha, on Tweenid lihtne ja võimas töövahend. Tavaline AnimatedSprite2D ega isegi AnimationPlayer sõlm ei suudaks `velocity` muutujat animeerida.

AnimationPlayer sõlm suudab küll muutujaid Tweenile sarnaselt animeerida, aga ainult **eksportmuutujaid**. AnimationPlayeri pluss on see, et see pakub graafilist kasutajaliidest animatsioonide loomiseks.
{: .tip }

## Peategelase helid



24:39