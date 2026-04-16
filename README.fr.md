# Camino de Santiago — Projet Data Science

> **Quels facteurs contextuels non-démographiques permettent d'expliquer et de prédire les mutations structurelles du Camino de Santiago entre 2003 et 2025 ?**

---

## Présentation

Ce projet analyse les **déterminants exogènes** des évolutions des flux de pèlerins sur le Camino de Santiago et les voies françaises, à partir d'un jeu de données multi-sources couvrant 2003–2025.

Contrairement aux nombreuses études existantes centrées sur le profil démographique ou les motivations des pèlerins, ce travail se concentre sur quatre axes peu ou pas explorés dans la littérature académique :

| Axe | Objet |
|-----|-------|
| **A — Climatosensibilité** | Impact des anomalies météo sur le choix de route et la fréquentation mensuelle |
| **B — Effet médiatique** | Corrélation entre films, livres, contenus YouTube/Vimeo, événements sportifs (trail) et vagues de pèlerins par pays |
| **C — Dispersion géographique** | Modélisation du transfert depuis le Camino Francés vers les routes secondaires (Espagne + voies françaises) |
| **D — Résilience post-crise** | Modèle de rebond différentiel par segment après le COVID, les Années Saintes et les chocs économiques |

---

## Périmètre

- **Couverture géographique** : Espagne (toutes routes officielles) + France (Via Podiensis, Turonensis, Lemovicensis, Tolosana + antennes régionales)
- **Période** : 2003–2025, granularité annuelle et mensuelle
- **Source principale** : Oficina del Peregrino, Santiago de Compostelle
- **Sources complémentaires** : AFCC (France), bureau des pèlerins de Saint-Jean-Pied-de-Port, données climatiques ERA5, Google Trends, résultats Trail du Saint-Jacques by UTMB, Wikiloc

---

## Approche méthodologique

Ce projet combine analyse quantitative de séries temporelles en panel et cadre interprétatif comportemental (NB02b). Les résultats quantitatifs sont systématiquement triangulés avec la recherche sur les motivations des pèlerins, la théorie anthropologique et les enquêtes institutionnelles qualitatives.

**Choix structurant — modélisation en panel route × année :**
L'unité d'analyse n'est pas l'année agrégée (n=21) mais la route × année (n=126, 6 routes × 21 ans). Ce choix résout le plafond statistique de la modélisation par parts de marché, dont les valeurs de R² étaient artificiellement élevées (jusqu'à 0.94) du fait d'une tendance linéaire commune. Les effets fixes route contrôlent les caractéristiques stables de chaque itinéraire.

---

## Résultats clés (NB03–NB04b)

### Ce que les données confirment

**Axe A — L'effet climatique existe et est positif sur les corridors espagnols.**
Une anomalie thermique estivale de +1°C sur un corridor est associée à +23% de volume de pèlerins sur cette route (p=0.010, n=126). Un été plus chaud qu'à la normale améliore les conditions de marche — moins de pluie, meilleure praticabilité. Ce signal était invisible à n=21 dans la modélisation par parts.

**Axe B — L'intérêt médiatique français est le signal système le plus robuste.**
`trends_FR_lag0` est significatif à p=0.000 sur les 6 routes du panel après contrôle des effets fixes. Chaque point d'intérêt supplémentaire sur Google Trends France est associé à +6,4% de volume sur l'ensemble du système. C'est un driver de demande global, pas spécifique à une route.

**`trends_PT` est un signal route-spécifique, pas systémique.**
Très prédictif pour le Camino Portugués et le Camino Portugués Costa individuellement, mais non significatif (p=0.159) quand on agrège les 6 routes principales. L'intérêt portugais est un driver ciblé, pas un moteur global.

**Axe C — L'accélération post-2018 est portée par l'intérêt médiatique français, pas par une rupture structurelle autonome.**
Lorsque `trends_FR_lag0` est contrôlé, la phase 2018+ perd sa significativité statistique. Ce qui semblait une mutation structurelle indépendante est en réalité la conséquence d'une montée continue de la visibilité médiatique en France.

**Axe D — Le rebond post-COVID a été uniforme sur tous les types de routes.**
Aucune différence significative de rythme de récupération entre routes accessibles, de défi et canoniques (p > 0.25 sur toutes les interactions DiD). Le rebond est fort (`post_covid` p=0.000) mais proportionnel — il reflète une libération globale de la demande différée, pas une réorientation comportementale.

**La culture trail est associée à une décélération du cluster défi physique.**
Volume de finishers du Trail du Saint-Jacques by UTMB et intérêt allemand prédisent négativement la croissance du cluster Norte/Primitivo/Vía de la Plata (Lasso R²=0.737). Hypothèse : le trail running capte une partie du profil "endurance" qui se tournait auparavant vers les routes difficiles.

### Ce que les données ne confirment pas

- L'hypothèse de récupération différentielle post-COVID selon le type de route
- `trends_PT` comme driver systémique (signal route-spécifique uniquement)
- La phase 2018+ comme rupture structurelle autonome (expliquée par l'intérêt médiatique)

### Limites méthodologiques

| Limite | Impact |
|--------|--------|
| n=126 observations panel | Modèles limités à 5 features max par spécification |
| Trail SJ disponible 2013–2024 uniquement | n≈60 pour les modèles trail-dépendants |
| Proxies ERA5 pour Primitivo, Inglés, Vía de la Plata | Précision réduite du signal climatique pour ces routes |
| Données SJPDP non disponibles | Proxy pèlerinage séquentiel approximatif |
| Camino Portugués Costa : n=9 obs utiles | Résultats indicatifs uniquement pour cette route |

---

## Valeur opérationnelle

Les résultats de ce projet sont conçus pour être directement utiles à :

- **Les collectivités territoriales** : Xunta de Galice, Régions Nouvelle-Aquitaine, Occitanie, Auvergne-Rhône-Alpes — pour anticiper les flux selon les conditions climatiques et les signaux médiatiques, et dimensionner les infrastructures d'accueil
- **Les associations jacquaires** : AFCC, Amis de Saint-Jacques (SJPDP) — pour adapter les ressources humaines et les campagnes de communication en fonction des signaux Google Trends par pays
- **Les organisateurs d'événements** : Trail du Saint-Jacques by UTMB — pour comprendre la relation de complémentarité et de substitution partielle entre pratique sportive et pèlerinage sur les routes de défi
- **Les acteurs du tourisme durable** — pour orienter les politiques de gestion des flux sur les routes patrimoniales, en tenant compte de la dynamique d'accessibilité (P.Costa, Inglés) comme moteur principal de diversification

---

## Structure du projet

La documentation technique complète est disponible dans [README.md](README.md) (en anglais).

---

## Auteur

Florian — Data Analyst & Data Scientist (MINES Paris PSL)
Passionné par le pèlerinage de Saint-Jacques de Compostelle depuis les années 1990.

---

## Licence

MIT License
