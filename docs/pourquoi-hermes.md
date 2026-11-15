# Pourquoi Hermes ?

Version courte et honnête. Chaque point renvoie à la documentation officielle ; si un point n'y figure pas, il n'est pas ici.

## 1. Une mémoire qui survit aux sessions

Hermes garde deux petits fichiers par profil ([doc](https://hermes-agent.nousresearch.com/docs/user-guide/features/memory)) :

| Fichier | Contenu | Limite |
|---|---|---|
| `MEMORY.md` | notes de l'agent : environnement, conventions, leçons apprises | ~2 200 caractères |
| `USER.md` | ton profil : préférences, style, attentes | ~1 375 caractères |

Ils sont injectés au début de chaque session. L'agent les tient lui-même à jour avec l'outil `memory`. Les conversations passées restent aussi consultables (recherche plein texte dans l'historique des sessions).

**Ce qu'il faut savoir :**

- La mémoire est **volontairement petite**. Quand elle est pleine, l'agent doit faire le tri ; elle ne grossit pas à l'infini.
- Elle est **figée en début de session** : ce qui est écrit pendant une session n'apparaît dans le prompt qu'à la session suivante. Démarre une nouvelle session (`/new`) aux moments naturels (tâche finie, changement de sujet).
- Elle est **par profil**. Ton `coder` et ton `architect` ne partagent pas leurs notes, et c'est voulu : ne lance jamais deux agents sur le même profil.
- Un modèle faible en appel d'outils peut **dire** « c'est noté » sans rien écrire. Vérifie le fichier si un souvenir manque.

## 2. Une boucle de rétroaction : l'agent apprend des procédures

Au-delà des faits, Hermes enregistre des **skills** : des procédures réutilisables (fichiers `SKILL.md`) chargées seulement quand elles servent. Quand l'agent découvre comment bien faire une tâche, ou que tu le corriges, il peut créer ou corriger le skill concerné. La prochaine fois, il part de cette version améliorée.

Pour éviter que ça dérive :

- un **curateur** en arrière-plan repère les skills créés par l'agent qui ne servent plus et les archive, sans jamais les supprimer ; les skills livrés avec Hermes ou installés depuis un registre n'y sont pas soumis ([doc](https://hermes-agent.nousresearch.com/docs/user-guide/features/curator)) ;
- tu peux exiger une **validation humaine** avant toute écriture de mémoire ou de skill (`write_approval`), et **verrouiller** un skill que tu veux garder intact (`hermes curator lock`).

Concrètement : tu corriges une fois, la team s'en souvient.

## 3. Une vraie team, pas un seul agent

- **Profils** : chaque agent a son propre dossier (config, clés, mémoire, skills, `SOUL.md`). Créer un profil `coder` donne une commande `coder` ([doc](https://hermes-agent.nousresearch.com/docs/user-guide/profiles)).
- **Kanban** : un tableau de tâches durable (SQLite) partagé entre profils. Une tâche a un assigné, des dépendances, des commentaires, une étape de revue (`request-review` / `request-changes`). Un dispatcher lance le bon profil quand une tâche est prête ([doc](https://hermes-agent.nousresearch.com/docs/user-guide/features/kanban)).
- **Indépendant du fournisseur** : chaque profil peut utiliser un modèle différent, par exemple un modèle fort pour concevoir et un modèle moins cher pour exécuter.

## Ce que Hermes ne fait pas pour toi

- Il ne garantit pas la qualité : un mauvais modèle reste un mauvais modèle.
- Il ne remplace pas ta relecture : les PR, les publications et les documents légaux restent sous ta validation.
- Il demande un peu de mise en place (profils, clés, gateway pour le dispatcher). Le [guide](installation.md) est là pour ça.
