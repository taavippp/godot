---
title: Ülesanne 3
layout: default
nav_exclude: true
---

# Ülesanne 3

Ülesandeks oli mängu (esimesele) vastase jaoks kirjutada skript. Vastane suudab ringi liikuda kahes suunas ning talle mõjub gravitatsioon, ehk õhus olles ta kukub alla. Kui ta puutub kokku seinaga, siis ta muudab suunda.

```gdscript
extends CharacterBody2D

@export var speed: int = 75
@export var sprite: AnimatedSprite2D

var direction: float = 1.0
var gravity: int = 25

func _physics_process(delta: float) -> void:
	if (is_on_wall()):
		direction = direction * -1.0
	velocity.x = speed * direction
	velocity.y += gravity
	sprite.flip_h = direction < 0.0
	move_and_slide()
```


[Tagasi õpetuse lehele](../2d-mang/vastane)