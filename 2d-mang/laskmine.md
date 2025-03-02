---
title: Laskmine
layout: default
parent: 2D mäng
nav_order: 3
---

# Laskmine

Selleks, et meie tegelane vastaseid lasta suudaks, peame looma talle eraldi stseeni sellest kuulist, mida ta pidevalt tulistama hakkab.

## Kuul

Kuuli stseeni ehitus on järgmine:

```mermaid
%%{init: {'theme':'neutral'}}%%
flowchart LR;
	root["CharacterBody2D (juursõlm)"];
	sprite["Sprite2D"];
	shape1["CollisionShape2D"];
	area["Area2D"];
	shape2["CollisionShape2D"];

	root --> sprite;
	root --> shape1;
	root --> area;
	area --> shape2;
```
Kasutame siin kaht uut sõlme:

### Sprite2D

Sprite2D on sarnane AnimatedSprite2D sõlmele, aga ei sisalda sisse ehitatud võimalust animatsioone luua. Kui tahad Sprite2D (ja teisi sõlmi, millel pole animatsiooni võimekust sisse ehitatud) animatsiooni jaoks kasutada, pead sellega koos kasutama AnimationPlayer sõlme.