# Règles de travail — Porger Survie

À lire avant toute intervention sur ce dépôt.

## Méthode imposée par Erwan

Chaque tâche suit trois temps, dans cet ordre, sans en sauter un.

**1. Plan avant de coder.** Avant d'écrire la moindre ligne, annoncer ce qui va
être modifié : les fonctions touchées, le comportement visé, les valeurs
chiffrées quand il y en a, et ce qui risque de casser ailleurs. Si la demande
est ambiguë — Erwan dicte à la voix — reformuler ce qu'on a compris et attendre
confirmation plutôt que deviner. Le plan tient en quelques lignes, il ne s'agit
pas d'un document.

**2. Coder, publier sur test.html, tester soi-même.** Jamais index.html
directement. Après publication, tester la version en ligne avec l'extension
Claude in Chrome, dans le Chrome d'Erwan, connecté à son compte. Erwan n'est
sollicité qu'en dernier recours.

**3. Revue après livraison.** Une fois la livraison faite et testée, relire son
propre diff : correction, régressions possibles, fragilités, sécurité,
maintenabilité. Signaler ce qu'on trouve même si rien ne casse à l'écran, et
dire franchement ce qui n'a pas pu être vérifié.

## Publication

- `test.html` d'abord, validation d'Erwan, puis recopie telle quelle sur
  `index.html`. Les deux fichiers doivent porter les mêmes améliorations.
- Ne jamais supprimer `test.html`.
- Vérifier la syntaxe JS avant chaque envoi : extraire les blocs `<script>` et
  passer `node --check`.
- L'écriture sur GitHub est bloquée depuis le cloud Cowork : la publication
  passe par l'API GitHub exécutée depuis le Mac d'Erwan, dossier connecté
  `/Users/erwan/Desktop/Programmation/🏰 TOWER DEFENSE 🧟`.
- Attendre 60 à 90 secondes que GitHub Pages déploie, puis vérifier que le SHA
  du dernier déploiement correspond au commit.
- `VERSION_JEU` et le champ `version` de `version.json` doivent rester
  identiques. Incrémenter par lot livré, jamais par test.

## Messages à Erwan

Quand il doit faire une manipulation : l'URL de test en tête du message, puis
les étapes numérotées, sans commentaire autour. Lui rappeler Cmd + Option + R
sur Safari. Ne jamais lui attribuer une erreur quand le problème vient du code.

## Suivi

`IDEES.md` tient la liste des idées, décisions en attente et feuille de route.
Le relire avant de proposer la suite, et le mettre à jour quand Erwan donne une
nouvelle idée — il demande explicitement qu'on note à sa place.
`CHANGELOG.md` tient le journal des bugs corrigés, ajouts et retraits.
