---
title: Ülesanne 1 lahendus
layout: minimal
nav_order: 99
---

# Ülesanne 1 lahendus

```gdscript
extends CharacterBody2D

@export var speed: int = 100
@export var sprite: Sprite2D

var direction: float = 1.0

func _ready() -> void:
	sprite.flip_h = direction < 0.0

func _process(delta: float) -> void:
	velocity.x = speed * direction
	
	move_and_slide()
```