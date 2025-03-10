---
title: Laskur 2.0
layout: default
nav_order: 6
has_children: true
---

See peatükk on töös!
{: .todo }

Planeeritud struktuur:

-   Projekti organiseerimine +
-   Ühine entity klass, mitu vastast +
    -   Entitytel on elusid, kui nad haiget saavad, lähevad nad läbipaistvaks +
    -   Üks entity juurde kes lendab ja üles-alla läheb
    -   GameCamera skript, mis järgib mängijat selle asemel et see mängija laps-sõlm on
-   Hitbox klass +
    -   Kõik entityd saavad nüüd haiget teha +
-   Tweenid, helid
    -   Tween viga saamise animeerimiseks +
    -   Tween uue entity liigutamiseks
    -   Helid mängijal
-   Skoor ja selle salvestamine
    -   Sõlm, mis vastaseid haldab ja annab teada kui üks hävineb
    -   Lihtne FileAccess

# Laskur 2.0

Arendame edasi peatükis "2D mäng" loodud projekti "Laskur". Arendades edasi eksisteerivat lihtsat projekti, saad sügavamalt tuttavaks Godot'ga. Loome juurde uusi vastaseid, peame skoori ja kasutame helisid.

Õpid selles peatükis juurde:
-	oma sõlme klassi kirjutamine
-	AnimationPlayer sõlm ja Tween animatsiooni süsteem
-	helide kasutamine
-	failide haldus
-	**muud sõlmed?**

## Mänguvarad

Helid kuskile

Äkki paneks spraidid ja helid ühte Drive kausta lihtsaks ligipääsuks
Või zip fail siia reposse kaasa lihtsalt

Jõudu tööle!