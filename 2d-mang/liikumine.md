---
title: Liikumine
layout: default
parent: 2D mäng
nav_order: 2
---

# Liikumine

Loo peategelase jaoks skript nimega `peategelane.gd`. Tegelasel on vaja liikumiseks teada kiirust ja liikumissuunda. Seega esimesed read, mis võiks skripti juurde kirjutada, on:

```gdscript
@export_range(0.0, 500.0) var speed: float = 200.0
var direction: float = 1.0
```

`@export_range` annotatsioon on sarnane tavalise `@export` annotatsiooniga, aga võimaldab muutujale piiride määramist. Kuna meil on kiirus ja liikumissuund eraldi muutujad, siis kiirus ei tohi alla 0 minna, sest tegelane hakkaks vastassuunas liikuma. Suund ei pea eksportmuutuja olema, kuna see muutub pidevalt tegelase liikudes.

Kustuta `_ready` funktsioon. Kirjutame `_process` funktsiooni tegelase algelise liikumise loogika. Esiteks peame nüüd tegelase liikumissuuna määrama. Seda saame teha käsuga `Input.get_axis`. See võtab kaks argumenti ??????????

Video pooleli 06:30 skripti loomise juures
