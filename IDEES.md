# Idées et décisions à traiter — Porger Survie

Liste tenue à jour au fil des discussions avec Erwan. Rien ici n'est codé tant
que ce n'est pas marqué FAIT. Les points sont classés par ordre de priorité
décidé avec lui.

---

## À faire ensuite

### 1. Équilibrage de la portée

Constat d'Erwan : au niveau 4 sur 8, la portée est déjà énorme.

Valeurs actuelles : base 200 px, +20 % par niveau (composé), 8 niveaux,
plafond dur à 600 px.

| Niveau | +20 % (actuel) | +15 % | +40 px fixe |
|---|---|---|---|
| 0 | 200 | 200 | 200 |
| 1 | 240 | 230 | 240 |
| 2 | 288 | 264 | 280 |
| 3 | 346 | 304 | 320 |
| 4 | 415 | 350 | 360 |
| 5 | 498 | 402 | 400 |
| 6 | 597 | 463 | 440 |
| 7 | 600 (plafond) | 532 | 480 |
| 8 | 600 (plafond) | 600 | 520 |

Problème réel : avec +20 %, le plafond de 600 est atteint dès le niveau 6,
donc les niveaux 7 et 8 sont payants mais ne donnent rien.

Trois options, à trancher par Erwan :

- A : garder +20 % et passer le maximum de 8 à 5 niveaux (portée finale 498)
- B : passer à +15 % par niveau en gardant 8 niveaux (montée plus douce,
  le plafond n'est atteint qu'au dernier niveau)
- C : passer à une valeur fixe, +40 px par niveau sur 8 niveaux
  (progression lisible, portée finale 520)

Repère d'échelle : un zombie normal fait 48 px de large. La portée de base de
200 px vaut donc environ 4 zombies de large. Le jeu n'a pas d'unité en mètres.

### 2. Objets de soin qui tombent au sol

État actuel du code, à confirmer avec Erwan avant de toucher quoi que ce soit :

- 💚 légendaire, rare : ramassé, il se stocke ; permet d'acheter « Soin
  complet » dans la boutique E pour 100 $ + 1 item
- 💜 épique, un peu moins rare : soigne 50 % des PV au ramassage, immédiatement

Erwan veut supprimer « le petit soin qui drop ». À clarifier : s'agit-il du
💜 épique (soin immédiat de 50 %) ? Le 💚 légendaire correspond déjà à ce
qu'il décrit comme « l'item épique rare qui donne le soin complet en boutique ».

### 3. Tuto

Un vrai environnement de tutoriel pour expliquer le contexte et les bases
(par exemple : avec les 20 $ de départ on peut déjà améliorer les dégâts).
Reporté volontairement, à faire plus tard.

---

## Reste de la feuille de route (issue de la passation)

- Mode autonome du robot : balayage radar visible, détection des cibles
  immobiles, achat par paliers en gemmes. Deux questions en attente :
  combien de paliers et à quel prix ; le clic manuel reste-t-il possible en
  autonome ou bascule-t-on avec un bouton
- Zombies déviants : visent la cible la plus proche plutôt que le joueur,
  rares pour ne pas neutraliser le robot
- Expérience sur les objets : les équipements gagnent de l'XP en tuant
  (3 XP en vague 1, 32 en vague 30), fusion d'objets pour donner de l'XP
  (commun 60, rare 240, épique 1100, légendaire 5000), coût par niveau
  80 × n^1.75, total 351 000 XP pour atteindre 30. Erwan veut un maximum
  supérieur à 30
- Enchantements façon Minecraft sur les objets et les tourelles : feu,
  éclair en chaîne (chaque cible touchée est mémorisée et ne peut plus être
  reprise dans la même chaîne), ralentissement, trois niveaux chacun. Les
  parchemins passeraient de l'amélioration de niveau à l'application
  d'enchantements
- Version mobile : boutons tactiles, le jeu se joue au clavier (E, R, Échap,
  1-5). Erwan teste avec le mode responsive de Safari
- Adresse de contact à renseigner dans CONTACT_EMAIL quand son Gmail sera prêt
- Musique de fond libre de droits (Incompetech, OpenGameArt)
- Unifier le nom du projet entre le dépôt (porger-survie), le titre
  (« Porger Survie ») et l'app Google (« Porger »)
- Concours : 5 € au premier du classement, récompenses des 2e et 3e non
  décidées. Le jeu tournant côté client, vérifier le replay du gagnant avant
  de payer

---

## Fait récemment

- Panneau développeur à deux blocs (partie en cours / hub) — publié en 1.1
- Robot allié déplacé dans les compétences (R), juste après la Tourelle :
  achat unique à 800 $ à partir de la vague 15, plus de touche T ni de
  fenêtre d'explication
- Correction : le jeu ne restait plus figé après l'achat du robot
- Correction : les zombies ne marchent plus sur place quand le jeu est en
  pause (E, R, Échap, placement et amélioration de tourelle passent
  maintenant tous par majPause)
