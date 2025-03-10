---
title: Ülesanne 3
layout: default
nav_exclude: true
---

# Ülesanne 4

Ülesandeks oli luua Area2D laiendav klass Hitbox, mis automaatselt Entity klassi isenditel elupunkte vähendab.

```gdscript
class_name Hitbox extends Area2D

signal hit_entity

func _ready() -> void:
	body_entered.connect(_hitbox_body_entered)

func _hitbox_body_entered(body: Node2D) -> void:
	if (body is not Entity):
		return
	var entity := body as Entity
	entity.take_damage()
	hit_entity.emit()
```

[Tagasi õpetuse lehele](../laskur-2.0/klassid#ülesanne-4)