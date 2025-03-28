---
title: Ülesanne 5
layout: default
nav_exclude: true
---

# Ülesanne 5

Ülesanne oli vastaste halduri stseenis kirjutada välja funktsioon, mis reageerib Timer sõlme `timeout` signaalile.
Funktsioon pidi valima suvalise vastase ja suvalise Marker2D sõlme. Vastane asetati Marker2D sõlme juurde. Lisaks pidi vastase surma korral edasi levitama uut signaali `spawned_enemy_died`.

```gdscript
extends Node2D

signal spawned_enemy_died

@export var enemy_scenes: Array[PackedScene] = []

var spawn_markers: Array[Marker2D] = []

func _ready() -> void:
	for child in get_children():
		if child is not Marker2D:
			continue
		spawn_markers.append(child)

# loob suvalise vastase ja asetab ta suvalise markeri asukohta
# vastase surma korral (died signaal) levitab enda spawned_enemy_died signaali
func _on_spawn_timer_timeout() -> void:
	var marker: Marker2D = spawn_markers.pick_random()
	var enemy_scene: PackedScene = enemy_scenes.pick_random()
	var enemy: Entity = enemy_scene.instantiate()
	enemy.global_position = marker.global_position
	enemy.died.connect(spawned_enemy_died.emit)
	add_child(enemy)
```

[Tagasi õpetuse lehele](../laskur-2.0/helid#ülesanne-5)