---
title: Helid
layout: default
parent: Laskur 2.0
nav_order: 2
---

See peatükk on töös!
{: .todo }

# Helid

Selles alapeatükis loome juurde veel ühe vastase, kes liigub Tweeniga loodud animatsiooni abiga, õpime Godot's helisid kasutama ja loome vastaseid haldava stseeni, mis vastaseid pidevalt juurde tekitab.

## Lendaja

Teeme juurde veel ühe vastase, kes lennata suudab. Loo uus stseen, kus juursõlmeks on Entity nimega `Flyer` (meie loodud klass). Järgime sama töövoogu, mis varem ja lisa juurde AnimatedSprite2D.

Sellel tegelasel on vaid **üks animatsioon**, mis **automaatselt** mängima hakkab. Kasutame ikka sama spraidilehte `tilemap.png`, millel oli **10 horisontaalset**, **6 vertikaalset** kaadrit ja **1-piksline kaadrite vahe**. Flyeri kaks kaadrit asuvad viimases reas, kollase putuka omade kõrval. Mina panin selle animatsiooni kaadrisageduseks 10 FPS.

![Flyeri animatsiooni detailid](./pildid/helid/flyer-animatsioon.png)

Lisa juurde CollisionShape2D, mille kujuks on RectangleShape2D ja Hitbox, mille CollisionShape2D kujuks on CircleShape2D. Määra neile suurused ise. Salvesta stseen `entities` kausta nagu teistegi olemustega. **Ära unusta sõlmede füüsikakihid** - juursõlm on *enemy* kihil ja põrkab kokku *level*iga, Hitbox peab ainult *player* kihti tuvastama.

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

Kui nüüd lisad paar lendajat põhistseeni, peaksid nad vaikselt üles-alla lendama. Nende liikumine peatub, kui nad maapinnaga kokku põrkavad, aga nad jätkavad liikumist, kui `velocity.y` taas negatiivse väärtuse saab.

## Peategelase helid

Meie mängus saab olema kaks erinevat heli ja mõlemad neist kuuluvad peategelasele.

-	`jump.wav` - hüppamise heli
-	`shoot.wav` - kuuli laskmise heli

Ava taas peategelase stseen ja lisa kaks `AudioStreamPlayer` sõlme. Mina nimetasin nad vastavalt ümber `JumpAudioStreamPlayer`iks ja `ShootAudioStreamPlayer`iks. Kui uurid ühte neist sõlmedest inspektorist, näed et tal on `Stream` omadus. Selle väärtuseks peame määrama enda heli. Seda saab mugavalt teha klõpsates `<empty>` lahtri kõrval olevale noolele (saab ka lahtri peal parem-klõpsuga) ning avanenud menüüst `Quick Load...` valida.

![Helifaili valimise instruktsioon](./pildid/helid/helifaili-valimine.png)

Avaneb väike aken, mis kuvab kõiki selleks väärtuseks sobivaid helifaile. Mina tegelesin ennem JumpAudioStreamPlayeriga, seega valisin `sounds/jump.wav`. Enne, kui teise heli juurde liigud, kontrolli oma heli volüümi. Mina leidsin, et see on vaikimisi liiga vali, seega langetasin `Volume dB` -10 detsibelli peale. Tee sama protsess läbi ka laskmise heliga. Laskmise helil võiks muuta ka `Max Polyphony` väärtuse 5 peale - see tähendab, et 5 laskmise heli võib korraga mängida.

Kui sõlmed on ette valmistatud, siis peame viimast korda `player.gd` skripti muutma. Meie heli sõlmede jaoks on vaja eksportmuutujaid ning on vaja leida õiged read, kus AudioStreamPlayeri `play()` funktsiooni kasutada.

```gdscript
	... (muu kood)
	if (is_on_floor() and Input.is_action_just_pressed("jump")):
		velocity.y = -jump_strength
		jump_audio.play() # hüppamise heli
	
	# sätib markeri asendi õigeks olenevalt suunast
	bullet_marker.position.x = abs(bullet_marker.position.x) * direction
	if (Input.is_action_just_pressed("shoot")):
		shot_projectile.emit(
			# kasutame global_position, sest position on suhteline juursõlmega
			bullet_marker.global_position,
			direction
		)
		shoot_audio.play() # laskmise heli
	... (muu kood)
```

## Vastaste haldur

Praegu tundub meie projekt ikka rohkem nagu prototüüp kui tõeline mäng. Seda saame parandada uue süsteemiga, mis vastaseid pidevalt põhistseenis juurde tekitama hakkab. Niimoodi on mängijal pidevalt vaja vastaseid hävitada ja mäng on kaasahaaravam.

Loo uus stseen, kus tavaline Node2D on juursõlm ja tema ainus laps-sõlm on Timer. Timer sõlme me pole veel kasutanud, aga tema funktsionaalsus on päris ilmne - see võtab aega ja mingi aja möödudes lõpetab oma töö.