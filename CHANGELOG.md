# Porger Survie — Journal du projet

Historique des corrections, ajouts et retraits depuis le début du
développement. Le numéro de version ne monte plus qu'à chaque lot de
travail réellement livré, pas à chaque test.

**Version actuelle : 1.0**

---

## Règle de numérotation

- **1.x** → un lot de corrections ou d'ajouts livré et vérifié
- **x.0** → refonte majeure ou jalon important
- Les tests intermédiaires ne changent pas le numéro

---

## Corrections de bugs

### Sauvegarde et scores

**La progression du hub n'était jamais envoyée au cloud.** La sauvegarde
en ligne ne partait qu'à la fin d'une partie. Tout ce qui était fait dans
le hub — acheter une armure, monter une compétence, poser une tourelle —
restait uniquement sur l'appareil, puis était écrasé au rechargement par
la version distante, plus ancienne. Corrigé par un envoi différé de
2,5 secondes après chaque modification, avec regroupement des
modifications rapprochées.

**Les scores refusés par le serveur disparaissaient en silence.** Le code
n'examinait jamais la réponse du serveur. Or `fetch` ne signale une erreur
que si le réseau est coupé : un refus d'accès passait pour un succès.
Combiné à l'expiration des jetons au bout d'une heure, cela faisait perdre
les scores des longues parties. Corrigé par un renouvellement automatique
du jeton, une file d'attente locale, et une alerte visible pour le joueur.

**La base de données rejetait les très bons scores.** Des contraintes
limitaient le niveau à 200, les kills à 50 000 et la vague à 500. Un
joueur ayant atteint le niveau 3531 voyait ses parties refusées sans
explication. Limites levées, seules les valeurs négatives sont interdites.

**La sauvegarde était perdue sans pseudo choisi.** L'envoi au cloud exigeait
un pseudo. Un joueur connecté sans en avoir choisi jouait donc sans aucune
sauvegarde. Un pseudo est désormais attribué automatiquement.

**Une progression locale plus avancée était écrasée** par la version du
cloud. Le local gagne maintenant lorsqu'il est plus avancé.

### Anti-triche

**Les parties longues et légitimes étaient rejetées.** Les seuils de
détection étaient fixes et calibrés sur des parties courtes. Le plafond
de gain d'argent, à 500 $ par vague plus 200 par kill, était dépassé par
tout bon joueur au-delà de la vague 30. Plus un joueur était bon, plus il
risquait l'exclusion. Les seuils suivent maintenant la progression.

**Les seuils exponentiels produisaient encore des faux positifs.** Seuls
les faits impossibles invalident désormais un score : sauter des vagues,
ou tuer plus de zombies qu'il n'en est apparu. Les gains élevés sont
signalés sans conséquence.

**Une partie invalide débloquait quand même la renaissance.** La meilleure
vague était mise à jour en local même pour une partie marquée invalide.

### Interface

**L'écran de fin empilait les récompenses à l'infini.** Le bloc de
récompenses était ajouté sans supprimer le précédent, et la fonction de
fin pouvait s'exécuter plusieurs fois. Sur une longue partie, des dizaines
de blocs repoussaient les boutons Rejouer et Hub hors de l'écran.

**Les menus ne mettaient pas le jeu en pause.** Seules la boutique, les
compétences et le menu pause étaient pris en compte. Le panneau
développeur, la fiche du robot, le filtre de butin et le formulaire de
support laissaient les zombies avancer.

**Les compteurs d'argent de la boutique ne se rafraîchissaient pas.**
Seul l'affichage principal était mis à jour, la boutique gardait une
valeur périmée.

**Le podium disparaissait quand la table était vide** au lieu d'inviter
à être le premier.

**Les réglages de son ne faisaient rien.** Les trois interrupteurs
enregistraient la préférence sans jamais toucher aux variables qui
contrôlent le son.

**Trois fonctions du panneau développeur n'existaient plus** —
invincibilité, tout tuer, soin complet — supprimées lors d'une réécriture.

**Le défilement de l'inventaire n'atteignait pas les parchemins.**

**L'écran d'équipement écrasait l'inventaire** en bas de page. Passé en
deux colonnes sur ordinateur.

### Zombies et animations

**Les profils gauche et droite étaient inversés.** Tous les zombies se
déplaçant horizontalement marchaient à reculons. Trouvé en analysant la
position de l'œil sur les images du sprite.

**Les proportions des sprites étaient fausses.** Le code utilisait des
cases de 20×22 pixels alors que le pack en fait 32×32, étirant les
zombies d'environ 10 % en hauteur.

**L'animation était saccadée sur quatre types sur cinq.** Tous
partageaient une animation décalant l'image de 320 pixels, alors que
chaque type a sa propre largeur. Ils sautaient donc des images de marche.

**L'orientation tremblait en diagonale.** Le sprite basculait à chaque
image affichée quand les déplacements horizontal et vertical étaient
proches. Corrigé par une marge de 40 %.

**Un contour carré entourait les nouveaux zombies**, dû à une ombre
portée qui ignore la transparence du sprite.

### Sécurité

**Injection de code par le pseudo.** Le pseudo des joueurs était inséré
directement dans le code des boutons de modération. Un pseudo malveillant
pouvait faire exécuter du code arbitraire dans le navigateur de
l'administrateur. Les boutons passent maintenant par l'identifiant
numérique du score.

**Aucun contrôle sur les caractères du pseudo.** Seule la longueur était
vérifiée. Validation ajoutée : lettres, chiffres, espaces, tirets et
underscores.

**Les fonctions de suppression étaient accessibles sans être connecté.**
En SQL, une comparaison avec une valeur nulle ne renvoie ni vrai ni faux,
ce qui laissait passer les visiteurs anonymes. Vérification explicite
ajoutée côté serveur.

### Divers

**La Rage annulait les améliorations achetées pendant son effet.** Elle
mémorisait la valeur totale des dégâts puis la restaurait, effaçant tout
achat fait entre-temps.

**Les vagues se lançaient en parallèle**, accumulant les zombies.

**La boucle de jeu plantait à chaque image** à cause d'un conflit de nom
de variable.

**Le bonus de renaissance était trop fort.** Linéaire à +10 % par
renaissance, sans limite. Passé en racine carrée avec un plafond à +40 %
pour les dégâts, les pièces gardant leur progression.

---

## Ajouts

### En ligne

- Classement mondial avec déduplication par joueur
- Connexion Google, publique et sans limite d'utilisateurs
- Sauvegarde de la progression dans le cloud
- Replay détaillé de chaque partie, vague par vague, avec les achats
- Système anti-triche avec journal et indice de suspicion statistique
- Renommage de pseudo qui emporte les anciens scores
- Formulaire de signalement de bugs avec capture d'écran, collable
  directement ou par glisser-déposer
- Boîte de réception pour l'administrateur, avec notifications
- Vérification automatique des mises à jour, avec bandeau d'alerte
- Aperçu enrichi lors du partage du lien

### Jeu

- Zombie soigneur, qui rend des points de vie à la horde
- Zombie tireur, qui attaque de loin sans s'approcher
- Robot allié dirigeable, débloqué à la vague 15, avec autodéfense et
  priorité de ciblage sur les soigneurs
- Second canon sur les tourelles, améliorable sur cinq niveaux, qui vise
  une cible différente du canon principal
- Apparence évolutive des tourelles selon leur niveau
- Sprites pixel art pour les zombies, à la place des emoji
- Filtre de butin par rareté, avec recyclage en pièces
- Recyclage en masse de l'inventaire
- Défis hebdomadaires, 50 répartis en quatre paliers
- Système de renaissance avec bonus permanents
- Amélioration possible d'un objet équipé sans le déséquiper

### Outils d'administration

- Panneau développeur avec réglages ajustables
- Mode suppression avec délai de 10 secondes annulable
- Dates d'inscription des joueurs
- Comptes développeurs multiples, identifiés par empreinte
- Obtention de tous les items pour comparer l'équilibrage

---

## Retraits et remplacements

- **Compteur de visites** retiré
- **Contact par e-mail** remplacé par le formulaire de signalement
- **Argent total cumulé** remplacé par le total gagné sur la partie
- **Robot en tant que compétence achetable** retiré : c'était une erreur
  de conception, il devenait améliorable à l'infini pour 0 $
- **Portée illimitée** plafonnée à 600, soit trois fois la portée de base
- **Vitesse de tir** plafonnée à 2,7 tirs par seconde
- **Nom du compte Google comme pseudo** abandonné au profit d'une
  numérotation simple

---

## Ce qui reste à faire

- Version mobile : le jeu se joue au clavier, il faudra des boutons
  tactiles
- Adresse de contact à renseigner
- Musique de fond libre de droits
- Système d'expérience pour les objets, en remplacement des parchemins
- Enchantements façon Minecraft, applicables aux objets et aux tourelles
- Mode autonome du robot, avec balayage radar
- Zombies déviants, qui visent la cible la plus proche
- Nom du projet à unifier entre le dépôt, le titre et l'application
