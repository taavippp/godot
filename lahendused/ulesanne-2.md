---
title: Ülesanne 2
layout: default
nav_exclude: true
---

# Ülesanne 2

Ülesandeks oli spraidi sõlme AnimatedSprite2D animatsioonid lõpuks kasutusele võtta ning seda ka pöörata, olenevalt suuna muutujast. Selle jaoks on loodud uus funktsioon `_animate_sprite()`.

```gdscript
extends CharacterBody2D

signal shot_projectile(
	spawn_position: Vector2,
	direction: float
)

@export_range(0, 500, 10) var max_speed: float = 200.0
@export_range(0, 1000, 10) var jump_strength: float = 400.0

@export var bullet_marker: Marker2D
@export var sprite: AnimatedSprite2D

var gravity: float = 25.0
var direction: float = 1.0

func _process(delta: float) -> void:
	direction = Input.get_axis("move_left", "move_right")
	velocity.x = direction * max_speed
	velocity.y += gravity
	
	if (is_on_floor() and Input.is_action_just_pressed("jump")):
		velocity.y = -jump_strength
	
	if (Input.is_action_just_pressed("shoot")):
		shot_projectile.emit(
			bullet_marker.global_position,
			direction
		)
	
	_animate_sprite()
	
	move_and_slide()

func _animate_sprite():
	# pöörab spraiti, kui suund on vasakule
	sprite.flip_h = (direction < 0.0)
	
	# kui tegelane on maas ja ei liigu, siis "default" animatsioon
	# kui tegelane on maas ja liigub, siis "run" animatsioon
	# kui tegelane ei ole maas (ehk õhus), siis "jump" animatsioon
	if (is_on_floor()):
		if (is_zero_approx(direction)):
			sprite.play("default")
		else:
			sprite.play("run")
	else:
		sprite.play("jump")
```

[Tagasi õpetuse lehele](../2d-mang/vastane)