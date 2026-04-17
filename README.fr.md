# Camino de Santiago — Projet Data Science

> **Quels facteurs contextuels non-démographiques permettent d'expliquer et de prédire les mutations structurelles du Camino de Santiago entre 2003 et 2025 ?**

---

## Présentation

Ce projet analyse les **déterminants exogènes** des évolutions des flux de pèlerins sur le Camino de Santiago et les voies françaises, à partir d'un jeu de données multi-sources couvrant 2003–2025.

| Axe | Objet |
|-----|-------|
| **A — Climatosensibilité** | Impact des anomalies météo sur le choix de route et la fréquentation mensuelle |
| **B — Effet médiatique** | Corrélation entre films, livres, contenus YouTube/Vimeo, événements sportifs (trail) et vagues de pèlerins par pays |
| **C — Dispersion géographique** | Modélisation du transfert depuis le Camino Francés vers les routes secondaires |
| **D — Résilience post-crise** | Modèle de rebond différentiel par segment après le COVID, les Années Saintes et les chocs économiques |

---

## Périmètre

- **Couverture géographique** : Espagne (toutes routes officielles) + France (Via Podiensis, Turonensis, Lemovicensis, Tolosana + antennes régionales)
- **Période** : 2003–2025, granularité annuelle et mensuelle
- **Source principale** : Oficina del Peregrino, Santiago de Compostelle
- **Sources complémentaires** : AFCC, SJPDP, données climatiques ERA5, Google Trends, Trail du Saint-Jacques by UTMB, Wikiloc

---

## Réponse à la question de recherche

> *« Quels facteurs contextuels non-démographiques expliquent les mutations structurelles du Camino de Santiago entre 2003 et 2025 ? »*

**Le facteur dominant est l'intérêt médiatique français** — un signal de demande continu et mesurable qui pilote le volume global du système et explique l'accélération post-2018 sans nécessiter de rupture structurelle exogène.

Le **climat** opère au niveau des corridors de route (+23 % de volume par +1°C d'anomalie estivale). La **culture trail** se substitue partiellement à la demande des routes de défi physique. Les **chocs structurels** (COVID, Années Saintes) amplifient la trajectoire existante sans la rediriger.

---

## Cinq résultats clés

**1. Axe A confirmé dans le panel** — Une anomalie thermique estivale de +1°C sur un corridor est associée à +23 % de pèlerins sur cette route (p=0,010, n=126). Ce signal était invisible à n=21 dans la modélisation par parts.

**2. Le rebond post-COVID a été uniforme** — Aucune différence significative de rythme de récupération entre routes accessibles, de défi et canoniques. Le rebond reflète une libération globale de demande différée, pas une réorientation comportementale.

**3. L'intérêt médiatique français est le signal système le plus robuste** (p=0,000) — Chaque point d'intérêt supplémentaire sur Google Trends France est associé à +6,4 % de volume sur l'ensemble des 6 routes du panel.

**4. L'intérêt portugais est un signal route-spécifique** — Très prédictif pour le Camino Portugués et le P.Costa individuellement, mais non significatif (p=0,159) quand on agrège les 6 routes. C'est un driver ciblé, pas systémique.

**5. L'accélération post-2018 est portée par l'intérêt médiatique français** — Lorsque `trends_FR_lag0` est contrôlé, la phase 2018+ perd sa significativité. Ce qui semblait une rupture structurelle autonome est la conséquence d'une tendance médiatique continue.

---

## Trois phases structurelles

| Phase | Période | Caractéristiques |
|-------|---------|-----------------|
| **1 — Stable** | 2004–2009 | Francés domine à 77–85 %. Profil A (traditionnel) dominant. Diversité 0,28–0,39. |
| **2 — Diversification** | 2010–2017 | Entrée du Profil B (laïc de quête) à grande échelle. Toutes les voies secondaires croissent. Diversité 0,39→0,62. |
| **3 — Recomposition** | 2018–2024 | Émergence du P.Costa comme route de masse. Parité hommes/femmes franchie en 2018. Diversité stabilisée à ~0,70. |

---

## Recommandations opérationnelles

**Xunta de Galice / Collectivités**
- Utiliser les prévisions de température estivale par corridor comme indicateur avancé de fréquentation (F1 : +1°C → +23 % attendus)
- En cas de choc type COVID, planifier un rebond proportionnel sur toutes les routes — pas de réallocation différentielle vers les voies accessibles (F2)

**AFCC / SJPDP**
- Surveiller Google Trends FR comme signal d'alerte précoce à lag=0 : c'est le driver système le plus fiable
- Les campagnes ciblant le Profil B (laïc, féminin, 35–55 ans) dans les médias français ont un impact systémique sur l'ensemble du réseau

**Trail du Saint-Jacques by UTMB**
- La culture trail est associée à une décélération du cluster de défi (Norte/Primitivo/Vía de la Plata) : substitution partielle confirmée
- Positionnement recommandé : passerelle vers le Camino pour les coureurs trail du Profil C, plutôt que concurrent des voies de défi

**Institutions académiques / Partenaires data**
- Priorités de collecte : données OPO (aviation Porto) pour tester l'hypothèse low-cost P.Costa ; données SJPDP pour quantifier le pèlerinage séquentiel ; nationalités post-2018 (Oficina) ; données 2025 pour décomposer le shift COVID

---

## Limites méthodologiques

| Limite | Impact |
|--------|--------|
| n=126 observations panel | Modèles limités à 5 features max |
| Trail SJ 2013–2024 uniquement | n≈60 pour les modèles trail |
| Proxies ERA5 pour 3 routes | Précision réduite du signal climatique |
| Camino Portugués Costa n=9 | Résultats indicatifs uniquement |
| Données SJPDP non disponibles | Proxy pèlerinage séquentiel approximatif |
| Nationalités post-2018 absentes | Dynamiques démographiques récentes non observables |

---

## Structure du projet

La documentation technique complète est disponible dans [README.md](README.md) (en anglais).

---

## Valeur opérationnelle

Les résultats sont directement utilisables par :

- **Les collectivités territoriales** : Xunta de Galice, Régions Nouvelle-Aquitaine, Occitanie, Auvergne-Rhône-Alpes — anticipation des flux, dimensionnement des infrastructures
- **Les associations jacquaires** : AFCC, Amis de Saint-Jacques (SJPDP) — adaptation des ressources, campagnes de communication
- **Les organisateurs d'événements** : Trail du Saint-Jacques by UTMB — compréhension des synergies et effets de substitution
- **Les acteurs du tourisme durable** — gestion des flux sur les routes patrimoniales

---

## Auteur

Florian — Data Analyst & Data Scientist (MINES Paris PSL)
Passionné par le pèlerinage de Saint-Jacques de Compostelle depuis les années 1990.

---

## Licence

MIT License
