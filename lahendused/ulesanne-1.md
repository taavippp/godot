---
title: Ülesanne 1
layout: default
nav_exclude: true
---

# Ülesanne 1

Ülesanne oli kirjutada algne skript peategelase poolt lastava kuuli jaoks, toetudes olemasolevatele teadmistele.

```gdscript
extends CharacterBody2D

@export var speed: int = 100
@export var sprite: Sprite2D

var direction: float = 1.0

func _ready() -> void:
	# kui suund on alla 0, siis kuul liigub vasakule ja pöörame spraiti
	sprite.flip_h = direction < 0.0

func _process(delta: float) -> void:
	velocity.x = speed * direction
	
	move_and_slide()
```

[Tagasi õpetuse lehele](../2d-mang/laskmine)