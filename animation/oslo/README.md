# Scénario Oslo (ENGM) – animation ProB

Deux systèmes Point Merge (VALPU à l'ouest, INSUV à l'est), points communs NOSLA (piste 01R) et OGRAS (piste 01L), mode mixte. 10 avions (A1–A7, A9, A10), fin du scénario à t = 3562.
Constantes : MergeBuffer 10, FinalDuration 270, RwySep 60, DepDepSep 90, MinLat 8.

Les instants viennent de la somme des `tick_tock` de l'History (moins 4 s, décalage constant de la première page), vérifiés sur les paramètres des événements et sur les captures d'état.

## Avions (secondes)

| Avion | Cat. | Entrée | TMA | Branche | Entrée branche | MP | Merge | CP (piste) | CP | Seuil | Touchdown | Décollage (piste) |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| A9  | Medium | ESEBA | 0    | LEG_EN | 290  | INSUV | 597  | OGRAS (01L) | 696  | 1007 | 1016 | 1126 (01L) |
| A1  | Heavy  | RIPAM | 0    | LEG_WS | 514  | VALPU | 805  | NOSLA (01R) | 901  | 1190 | 1225 | 1281 (01L) |
| A3  | Medium | RIPAM | 157  | LEG_WS | 671  | VALPU | 981  | NOSLA (01R) | 1077 | 1396 | 1396 | 1665 (01R) |
| A5  | Light  | RIPAM | 288  | LEG_WS | 802  | VALPU | 1175 | NOSLA (01R) | 1271 | 1551 | 1565 | 1747 (01L, déduit) |
| A4  | Heavy  | BELGU | 0    | LEG_WN | 1016 | VALPU | 1325 | NOSLA (01R) | 1421 | 2017 | 2048 | 2145 (01L) |
| A2  | Medium | LUNIP | 1016 | LEG_ES | 1474 | INSUV | 1857 | NOSLA (01R) | 1937 | 2207 | 2207 | 2768 (01R) |
| A10 | Heavy  | ADOPI | 1126 | LEG_WN | 1565 | VALPU | 2056 | NOSLA (01R) | 2152 | 2476 | 2662 | 2809 (01L) |
| A7  | Heavy  | RIPAM | 1848 | LEG_WS | 2327 | VALPU | 2781 | NOSLA (01R) | 2877 | 3157 | 3223 | 3562 (01L) |
| A6  | Medium | RIPAM | 2005 | LEG_WS | 2490 | VALPU | 2953 | NOSLA (01R) | 3049 | 3345 | 3352 | 3460 (01L) |

Instants des Direct_To : A9 290, A1 514, A3 690, A5 884, A4 1016, A2 1565, A10 1747, A7 2490, A6 2662 (Move_On_Leg de A6 à 2637).

## États critiques

| État | t | Événement | Valeurs |
|---|---|---|---|
| CS1  | 0    | Entry_in_TMA (A3)  | 0 < 0 + 157 : entrée à RIPAM retardée à 157 (A1 Heavy) ; idem A6 à 1848 → 2005 |
| CS2  | 640, 778 | Entry_Leg (A3, A5) | 640 < 514 + 157 → 671 ; 778 < 671 + 131 = 802 |
| CS3/CS4 | 2637 | Direct_To (A6) refusé, Move_On_Leg | 2637 + 291 = 2928 < 2781 + 157 + 10 = 2948 ; accepté à 2662 |
| CS7  | 1126, 1281 | Landing_Verification (A3, A5) | 1126 + 270 = 1396 ≥ 1190 + 157 ; 1281 + 270 = 1551 ≥ 1396 + 131 |
| CS8  | 1016, 1225, 1396, 2662, 3223, 3352 | Touchdown | piste Unavailable, feux ON |
| CS9  | 1565 | Start_takingoff (A3) refusé | A5 touche la piste 01R à 1565 (à vérifier sur capture) |
| CS10 | 1665 | Start_takingoff (A3) | 1665 ≥ 1565 + 60 et 2048 ≥ 1665 + 60 |
| CS11 | 1565 | Direct_To (A10) refusé | VALPU libre, mais 1565 + 405 = 1970 < 1937 + 60 + 10 = 2007 à NOSLA ; accepté à 1747 (calculé, pas de capture) |
| CS12 | ~3511 | Start_takingoff (A7) refusé | 3511 < 3460 + 90 = 3550 (A6 a décollé de 01L à 3460) ; A7 décolle à 3562 |

CS5 et CS6 ne sont pas atteints dans ce scénario.

## Contenu

- `fig_scenario_oslo.png` : frise temporelle du scénario (comme pour Dublin).
- `steps/` : les 46 captures de l'animation, numérotées comme dans le fichier d'origine (14 et 38 absentes).
- `history/` : les 10 pages de l'History (de la plus ancienne à la plus récente).
- `critical_states/` : les captures des états critiques.
