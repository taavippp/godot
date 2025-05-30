---
title: Projekt Laskur
layout: default
parent: 2D mäng
nav_order: 1
---


# Projekt Laskur

-	TOC
{:toc}

## Uus projekt

Loo uus projekt nimega `Laskur`, mis kasutab taaskord Compatibility renderdajat. Kasutame [peatüki sissejuhatuses](https://taavippp.github.io/godot/2d-mang/) allalaetud mänguvaradest `sprites` kaustas asuvat `tilemap.png` spraidifaili. Liiguta need kaustad oma projekti kausta. Godot peaks need failid automaatselt importima, kui redaktorisse naased.

## Peategelase stseen

Esimese asjana loome mängu peategelase stseeni. Peategelane suudab ringi joosta, hüpata ja oma relva lasta. Stseeni algne struktuur on järgnev:

```mermaid
%%{init: {'theme':'neutral'}}%%
flowchart LR;
	root["CharacterBody2D (juursõlm)"];
	sprite["AnimatedSprite2D"];
	shape["CollisionShape2D"];

	root --> sprite;
	root --> shape;
```

Uurime lähemalt neid 3 uut sõlme:

### CharacterBody2D

Meie mäng kasutab Godot sisse ehitatud füüsikamootorit. See süsteem on arendaja jaoks lihtsustatud **füüsikakeha sõlmedega**. CharacterBody2D on üks neist ning seda sõlme liigutatakse läbi koodi.

Lisaks on olemas ka **RigidBody2D** ja **StaticBody2D**. RigidBody2D liigub kasutades impulsside süsteemi (üks pidev impulss on näiteks gravitatsioon), seda sõlme kasutatakse veidi täpsema füüsika simulatsiooni loomisel. StaticBody2D ei liigu, seega seda sõlme kasutatakse näiteks maapinna jaoks.

### CollisionShape2D

Füüsikakehadel on vaja kuju, et nad üksteist mõjutada saaksid. Siin tuleb appi CollisionShape2D sõlm, mis on ühine lahendus kõikidel 2D füüsikakehadel kuju loomiseks. Keha kuju saab olla ristkülik, ring jne.

### AnimatedSprite2D

Selle sõlmega on lihtne oma spraidi animatsioonid kokku panna kaadrihaaval ja neid redigeerida. Eksisteerib ka sõlm Sprite2D, millel pole juurdeehitatud animatsiooni võimekust.

## Animatsioon

Selleks, et teada, kui suur ja milline CollisionShape tulema peaks, paneme enne paika spraidi. Loo inspektoris AnimatedSprite2D -> Animation -> Sprite Frames jaoks uus SpriteFrames ressurss, vajutades `<empty>` tekstiga lahtrisse.

![SpriteFrames loomine](./pildid/projekt-laskur/spriteframes-loomine.png)

Peale SpriteFrames ressurssi loomist ava see, klõpsates uuesti sama lahtri peale. Alumisel ribal avaneb uus moodul. Vaikimisi on olemas animatsioon "default", aga me tahame juurde luua ka "run" ja "jump". Uue animatsiooni saad luua vasakus "Animations" menüüs, vajutades **rohelise plussiga paberi** ikooni peale. Loodud animatsiooni saad ümber nimetada selle peale klõpsates.

Peale animatsioonide loomist on vaja neile kaadrid juurde lisada. Vali animatsioon "default" ja leia parempoolses "Animation Frames" menüüs ruudustiku nupp `Add frames from sprite sheet`. Avaneb faili valimise aken, vali meie `tilemap.png`.

![Animatsioonide loomine SpriteFrames kaudu](./pildid/projekt-laskur/animatsioonide-loomine.png)

Avaneb uus aken, kus saad valida, mis kaadritest animatsioon koosneb. Esiteks peame seda akent konfigureerima, kasutades paremal pool olevaid sätteid. Pilt jaotub 10 horisontaalseks x 6 vertikaalseks = 60 kaadriks ja kaadritel on ühe piksline vahe ehk _separation_.

![Animatsiooni kaadrite valimine](./pildid/projekt-laskur/animatsiooni-kaadrid.png)

Meie peategelase animatsioonid koosnevad vaid kahest kaadrist viienda rea vasakul pool. "default" animatsioon kasutab vaid esimest neist, "run" kasutab mõlemat ja "jump" kasutab teist.

Vaikimisi on nende animatsioonide kaadrisagedus 5 FPS (kaadrit sekundis/*frames per second*). See tähendab, et kui "run" animatsioonil on kaks kaadrit, siis sekundi jooksul jõuab see animatsioon korduda 5 / 2 = 2.5 korda. Minu arvates tundus "run" animatsioon sedasi aeglane, seega määrasin, et see oleks `10 FPS`.

Meie tegelane tundub küll natuke hägune. Tegu on automaatse filtriga Godot poolt, kuid meie pikslikunsti jaoks see ei sobi. Paranda seda valides ülaribalt Project -> Project Settings. Seejärel kirjuta otsingusse `texture filter`, ava vasakust menüüst ainus otsingutulemus ja väärtuse `Linear` asemel olgu `Nearest`. Nüüd peaks pikslikunst kena ja terav välja nägema.

## Füüsika kuju

Nüüd, kus animatsioonid loodud, võiks tegeleda CollisionShape2D sõlmega. Loo inspektoris CollisionShape2D -> Shape -> RectangleShape2D ressurss. Määra selle suuruseks (10, 14) pikslit ja muuda Node2D -> Transform -> Position väärtuseks (0, 1). Nüüd on peategelasel korrektne füüsika kuju.

![Füüsika kuju detailid](./pildid/projekt-laskur/fuusika-kuju.png)

## Ressurssidest

**Ressurssid** on Godot viis andmete mugavalt hoidmiseks. Just kasutasime **SpriteFrames** ja **RectangleShape2D** ressursse. Järgmises alapeatükis kasutame oma mängu tasemete seadistamisel **TileSet** ressurssi. Ka stseenid on salvestatud PackedScene ressurssidena. Ressurssid on tihti salvestatud stseeni osana, aga kui soovid näiteks ühte ressurssi mitmes kohas kasutada, saad selle ka eraldi `.tres` lõpuga faili salvestada. Saad ka luua enda ressurssi klassi, laiendades Resource klassi, andes oma klassile nime `class_name` võtmesõnaga ja redaktori taaskäivitades.

Ressurssidega on ka **inspektoris** alati sama töövoog, ühe ja sama lahtri kaudu:

1.	tunned ressurssi ära, kui selle puudumisel on muutuja väärtuseks lahter tekstiga `<empty>`
2.	klõpsad selle lahtri peale ning valid sobiva ressurssi mida luua
3.	kui uuesti lahtrile klõpsad, avaneb ressurssi menüü, kus selle andmeid muuta saad
4.	paremklõpsu menüüst lahtri peal on võimalik seda ressurssi täpsemini hallata

## Tegevuste loomine

Paljud mängud pakuvad erinevaid skeeme tegelase liigutamiseks - klaviatuur, hiir, konsoolipult jne. Godot lihtsustab seda protsessi kasutades tegevuste süsteemi. Näiteks tegevuse "move_right" alla saab määrata klaviatuuri klahvi vajutuse ja konsoolipuldi liigutuse. Vaja on vaid kontrollida siis, kas tegevus "move_right" toimub.

Tegevusi saab luua ülaribalt Project -> Project Settings menüüs Input Map saki alt. Lahtrisse `Add New Action` saad kirjutada oma tegevuse nime ja selle kõrval olevast `Add` nupust selle lisada. Meil on tarvis järgnevaid tegevusi:

-	move_right
-	move_left
-	jump
-	shoot

Peale tegevuste deklareerimist, saab määrata, mis sisend mängija poolt selle tegevuse käivitab **paremas ääres olevast plussi** nupust. Saad lihtsalt vajutada sellele klahvile, mis sinu arust sobib ja siis `OK` vajutada.

Mina kasutan liikumiseks nooleklahve, hüppamiseks Z klahvi ja laskmiseks X klahvi. Peale nende määramist võid sätete akna sulgeda.

![Input map details](./pildid/projekt-laskur/input-map.png)

Nimetame juursõlme ümber `Player`iks. Seda saad teha, valides stseeni dokis juursõlme ning paremklõpsu menüüs `Rename` valikule vajutades või sõlmele klõpsates. Salvesta stseen, vajutades `CTRL + S`. Godot pakub faili salvestada nimega `player.tscn`. See sobib meile.

Järgmises alapeatükis õpime Godot füüsikamootorit tundma, kirjutades tegelase liigutamiseks skripti.

[Järgmine alapeatükk "Liikumine"](./liikumine)