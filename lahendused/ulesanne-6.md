---
title: Ülesanne 6
layout: default
nav_exclude: true
---

# Ülesanne 6

Ülesandeks oli õppida Godot' sisse ehitatud dokumentatsiooni kasutada ja uurida FileAccess klassi kohta. Seejärel oli vaja mängija rekordskoor faili salvestada. Rekordit võrreldakse saavutatud skooriga siis, kui mängija sureb.

Kui selle klassi dokumentatsiooni õigesti suutsid avada, siis failidega tegelemise lahendus oli kohe *Description* osas.

```gdscript
... (muu kood)

const HIGH_SCORE_FILE_PATH: String = "user://high_score.dat"

... (muu kood)

# kirjutame skoori faili, kui high_score ületatud
func _on_player_died() -> void:
	if (score <= high_score):
		return
	var file = FileAccess.open(HIGH_SCORE_FILE_PATH, FileAccess.WRITE)
	if (is_instance_valid(file)): # kontrollib, et fail eksisteerib
		file.store_string(String.num(score)) # kirjutab skoori faili
```

[Tagasi õpetuse lehele](../laskur-2.0/skoor#ülesanne-6)