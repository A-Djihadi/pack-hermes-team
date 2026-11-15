# Pourquoi Hermes ?

Version courte. Chaque point renvoie à la documentation officielle. Ce qui n'y figure pas n'est pas ici.

## 1. Une mémoire qui survit aux sessions

Hermes garde deux petits fichiers par profil ([doc](https://hermes-agent.nousresearch.com/docs/user-guide/features/memory)) :

| Fichier | Contenu | Limite |
|---|---|---|
| `MEMORY.md` | notes de l'agent : environnement, conventions, leçons apprises | ~2 200 caractères |
| `USER.md` | ton profil : préférences, style, attentes | ~1 375 caractères |

Hermes les injecte au début de chaque session, et l'agent les met à jour lui-même avec l'outil `memory`. Tu peux aussi fouiller les conversations passées : l'historique des sessions a une recherche plein texte.

Ce qu'il faut savoir :

- La mémoire est petite, exprès. Quand elle est pleine, l'agent fait le tri au lieu de l'agrandir.
- Elle est figée en début de session. Ce que l'agent écrit pendant une session n'arrive dans le prompt qu'à la suivante. Lance une nouvelle session (`/new`) quand une tâche se termine ou que tu changes de sujet.
- Elle est propre à chaque profil. Ton `coder` et ton `architect` ne partagent pas leurs notes, et c'est voulu. Ne lance donc jamais deux agents sur le même profil.
- Un modèle faible en appel d'outils peut répondre « c'est noté » sans rien écrire. Si un souvenir manque, ouvre le fichier.

## 2. Une boucle de rétroaction : l'agent apprend des procédures

Hermes ne retient pas que des faits. Il enregistre aussi des skills, des procédures réutilisables (fichiers `SKILL.md`) qu'il charge seulement quand elles servent. Quand l'agent trouve comment bien faire une tâche, ou quand tu le corriges, il peut créer ou corriger le skill concerné. La fois suivante, il repart de cette version.

Deux garde-fous limitent la dérive :

- un curateur tourne en arrière-plan. Il repère les skills créés par l'agent qui ne servent plus et les archive, sans les supprimer. Les skills livrés avec Hermes ou installés depuis un registre ne sont pas concernés ([doc](https://hermes-agent.nousresearch.com/docs/user-guide/features/curator)) ;
- tu peux exiger ta validation avant toute écriture de mémoire ou de skill (`write_approval`), et verrouiller un skill que tu veux garder tel quel (`hermes curator lock`).

En pratique, tu corriges une fois et la team s'en souvient.

## 3. Une vraie team, pas un seul agent

- Profils : chaque agent a son propre dossier (config, clés, mémoire, skills, `SOUL.md`). Un profil `coder` te donne une commande `coder` ([doc](https://hermes-agent.nousresearch.com/docs/user-guide/profiles)).
- Kanban : un tableau de tâches durable (SQLite), partagé entre profils. Une tâche a un assigné, des dépendances, des commentaires et une étape de revue (`request-review` / `request-changes`). Un dispatcher lance le bon profil quand une tâche est prête ([doc](https://hermes-agent.nousresearch.com/docs/user-guide/features/kanban)).
- Pas lié à un fournisseur : chaque profil peut tourner sur un modèle différent. Par exemple, un modèle fort pour concevoir et un moins cher pour exécuter.

## Ce que Hermes ne fait pas pour toi

- Il ne garantit pas la qualité. Un mauvais modèle reste un mauvais modèle.
- Il ne remplace pas ta relecture. Les PR, les publications et les documents légaux passent par toi.
- Il demande un peu de mise en place : profils, clés, gateway pour le dispatcher. Le [guide](installation.md) couvre tout ça.
