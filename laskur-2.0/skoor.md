---
title: Skoor
layout: default
parent: Laskur 2.0
nav_order: 3
---

# Skoor

Selles alapeatükis lõpetame töö Laskuri projekti kallal. Selleks on vaja küllalt tööd veel teha:

-	skoori süsteem
	-	skoor tõuseb, kui tapad vastase
	-	kui saad rekordi, salvestatakse see faili
-	graafiline kasutajaliides
	-	nii praegust skoori kui ka rekordit näidatakse
	-	kui peategelane sureb, näidatakse sellest teavitavat sõnumit
		-	kui said rekordi, antakse sellest teada

## Skoori süsteem

Peame põhistseeni skriptile palju muudatusi tegema. Alustame skoori lugemise süsteemiga.

Me tahame, et skooripunktid tõuseks, kui vastane sureb ehk levitab `died()` signaali. Eelmises alapeatükis lõime EnemyManager stseeni, mis levitab enda `spawned_enemy_died()` signaali, kui tema vastase surmast teada saab. Seega saame lihtsalt **EnemyManageri signaali põhistseeni skriptiga ühendada**, et skooripunkte juurde lisada.

Lisa põhistseeni skripti järgnevad read juurde:

```
var score: int = 0

... (muu kood)

func _on_enemy_manager_spawned_enemy_died() -> void:
	score += 1
```

Nüüd, kus lihtne skoori lugemine on olemas, võiks kuidagi meelde jätta, mis mängija rekord on. Selleks loo juurde `high_score` muutuja, mille vaikeväärtus on ka 0. Loogiline oleks teha võrdlust praeguse skoori ja rekordi vahel siis, kui peategelane sureb. Selleks saame tema `died()` signaali ühendada skriptiga, mis tekitab `_on_player_died()` funktsiooni. Praegu saab selle funktsiooni sisuks lihtsalt lihtne `print()` käsk, kui mängija rekordi püstitab.

```gdscript
... (muu kood)

func _on_player_died() -> void:
	if (score <= high_score):
		return
	print("Said rekordi! Oled tõeline mängur.")
```

## Failihaldus

Nüüd, kus põhistseen skoori ja rekordeid suudab pidada, peaks parima skoori faili salvestama. Selleks kasutame FileAccess klassi. Tegeleme ennem faili kirjutamisega ehk rekordskoori salvestamisega.

### Ülesanne 6

Selleks, et iseseisvamalt Godot' õppida kasutama, pead oskama Godot' dokumentatsiooni sirvida. Õnneks on Godot ametlik dokumentatsioon **sisse ehitatud redaktorisse** ja kättesaadav ka **veebi kaudu** <https://docs.godotengine.org/en/stable/>. Redaktorisse sisse ehitatud dokumentatsioon on kättesaadav päris mitmel moel:

-	`F1` klahv
-	ülaribalt `Help -> Search Help...`
-	kui oled skripti redaktoris, on selle ülaosas paremal `Search Help` nupp
-	inspektoris luubi ikooniga nupp

![Kollaaž erinevatest dokumentatsiooni avamise viisidest](./pildid/skoor/dokumentatsiooni-avamise-viisid.png)

Avanenud aknas kirjuta otsinguribasse, mis sõlme/resurssi/klassi kohta uurida tahad ja vajuta `Open` või `Enter` klahvi.

Ülesanne on avastada, kuidas FileAccess klassiga faili avada ja põhistseeni skriptiga rekordskoor faili salvestada. Võiks kontrollida ka, et fail ikka olemas on.

[Ülesande lahendus](../lahendused/ulesanne-6)

## Failihaldus, jätk

Nüüd, kus salvestame rekordskoori faili, võiks seda mängu käivitades failist ka lugeda. Teeme seda muidugi `_ready()` funktsioonis. Koodiread tulevad sarnased ülesande lahendusele, aga kasutame `FileAccess.WRITE` argumendi asemel `FileAccess.READ`. Kui fail eksisteerib, siis võtame kogu selle sisu, muudame ümber täisarvu andmetüübiks ja määrame selle `high_score` väärtuseks.

```gdscript
# loe faili et high_score saada
func _ready() -> void:
	var file = FileAccess.open(HIGH_SCORE_FILE_PATH, FileAccess.READ)
	if (is_instance_valid(file)):
		high_score = int(file.get_as_text())
	print("Rekord on ", high_score)
```

Kui nüüd mängu käivitad, siis `Output`is kirjutatakse, mis mängu rekordskoor on. Skoori süsteem töötab!

## Kasutajaliides

Nüüd, kus mäng suudab skoori pidada ja parimat lausa salvestada, võiks mängijale ka näidata, mis tema tulemused on.

Kui praegu põhistseeni lisad näiteks Label sõlme ja stseeni käivitad, ei püsi see ekraanil ühe koha peal.