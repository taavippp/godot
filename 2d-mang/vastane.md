---
title: Vastane
layout: default
parent: 2D mäng
nav_order: 4
---

# Vastane

Lõpuks võtame kasutusele peategelase animatsioonid ning teeme talle ka mõned parandused. Pärast loome vastase, keda lasta saab.

## Peategelase lõpetamine

### Ülesanne 2

Meie peategelase AnimatedSprite2D sõlmel on 3 animatsiooni: "default", "run" ja "jump". AnimatedSprite2D väärtusega eksportmuutuja suudab animatsiooni mängida `play` funktsiooniga. Lisaks peab sprait vastavalt suunale end pöörama.

Tead, et funktsioon `is_on_floor()` tagastab, kas tegelane on maapinnaga kontaktis. On vaja ka tuvastada suuna muutuja abil, kas tegelane liigub. Selleks, et kontrollida, kas ujukomaarv on null, on olemas funktsioon `is_zero_approx(x: float)`.

Suuna muutmiseks on `AnimatedSprite2D` sõlmel selleks sarnaselt Sprite2D'le `flip_h` muutuja.

Mina panin enda lahenduses animeerimisega seotud koodiread eraldi funktsiooni.

[Ülesande lahendus](../lahendused/ulesanne-2)

## Peategelase lõpetamine, jätk

Nüüd parandame laskmisega seotud probleemid. Need olid järgnevad:

1.	kui tegelane ei liigu kuuli lastes, siis ka kuul ei liigu
2.	vasakule liikudes ja lastes luuakse kuul tegelase selja tagant

Mõtleme, miks need bugid tekkida võivad.

### Probleem 1

Kuul saab suuna peategelase käest. Suuna muutujat kasutatakse `velocity.x` arvutamiseks **korrutustehtes**. Peategelase suuna väärtus saab olla 0. Kui kiiruse muutujat korrutatakse nulliga, siis `velocity.x` on olenemata kiiruse muutuja väärtusest alati 0.

Selgub, et probleem on meie suuna muutujas. Kindlasti eksisteerib ka teisi lahendusi, aga minu valik on kasutada suuna jaoks kahte muutujat:

-	üks neist on mängija sisend (-1 ja 1 vahel, saab 0 olla) nimega `direction_input`
-	teine on tegelik suund (ei saa 0 olla) nimega `direction`

Loo siis juurde `direction_input` muutuja. Kui kutsud välja `Input.get_axis` funktsiooni, siis selle väärtus antakse nüüd sellele muutujale. Peale seda peame kohe oma `direction` muutujale ka väärtuse andma, mis põhineb `direction_input` põhjal.

```
... (muu kood)
var direction_input: float = 0.0
var direction: float = 1.0

func _process(delta: float) -> void:
	direction_input = Input.get_axis("move_left", "move_right")
	# kui toimub liikumine, võib tegelik suund muutuda
	if (not is_zero_approx(direction_input)):
		direction = sign(direction_input)
	... (muu kood)
```

Funktsioon `sign(x: float)` tagastab:

-	-1, kui `x` on negatiivne
-	0, kui `x` on 0 (meil seda juhtu pole, sest kasutame `not is_zero_approx(x)`)
-	1, kui `x` on positiivne

Nüüd peame mõtlema, kus kasutame mängija suuna sisendit ja tegeliku suuna muutujat.

Proovi aru saada igast juhust:

-	`velocity.x` arvutamisel kasutame `direction_input` muutujat
-	kuuli loomise signaali levitamisel kasutame `direction` muutujat
-	spraidi pööramise tingimuslauses kasutame `direction` muutujat
-	spraidi animatsiooni valikul kasutame `direction_input` muutujat

`direction_input` kasutame, kui on oluline teada, kas tegelane üldse liigubki. `direction` kasutame, kui meid huvitab puhtalt tema suund.

### Probleem 2

Tegelane laseb vasakule liikudes kuuli oma selja tagant. Kuuli luuakse vastavalt tegelase Marker2D asendile. Marker2D ei muuda suunda, kui tegelane seda muudab.

Kui paneme Marker2D ka suunda muutma, siis see bugi on läinud. Saame selle probleemi tegelikult ühe koodireaga parandada:

```gdscript
... (muu kood)
	# sätib markeri asendi õigeks olenevalt suunast
	bullet_marker.position.x = abs(bullet_marker.position.x) * direction
	if (Input.is_action_just_pressed("shoot")):
	... (muu kood)
```

Võtame markeri praeguse asendi absoluutväärtuse ja korrutame selle suunaga.

## Roomav vastane

Loome nüüd lõpuks vastase, keda võimalik lasta oleks.
