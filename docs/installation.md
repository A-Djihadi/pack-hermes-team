# Installation pas à pas

Ce guide s'adresse aux débutants. Les commandes viennent de la [documentation officielle](https://hermes-agent.nousresearch.com/docs/getting-started/installation). En cas de doute, c'est la doc qui a raison, et `hermes <commande> --help` donne les options exactes de ta version.

## Étape 0 : prérequis

- Linux, macOS (Apple Silicon), Windows ou WSL2.
- Une clé API d'un fournisseur de modèles (ou un abonnement compatible).
- `git` installé. Pour la partie dev : un compte GitHub et la CLI `gh` (`gh auth login`).

## Récupère le pack

Clone le dépôt pour avoir les fiches et les `SOUL.md` sous la main :

```bash
git clone https://github.com/A-Djihadi/pack-hermes-team.git
cd pack-hermes-team
```

Le dépôt est privé : ton compte GitHub doit y avoir accès. Si git demande une authentification, lance `gh auth login`.

## Étape 1 : installer Hermes

Sous Linux, macOS ou WSL2 :

```bash
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
source ~/.bashrc   # ou ~/.zshrc
```

Sous Windows, dans PowerShell :

```powershell
iex (irm https://hermes-agent.nousresearch.com/install.ps1)
```

Si tu préfères une app, le [site Hermes](https://hermes-agent.nousresearch.com/) propose des paquets Desktop pour Windows et macOS.

> Bonne pratique : lis un script avant de l'exécuter (`curl -fsSL <url> | less`).

## Étape 2 : configurer le modèle et vérifier

```bash
hermes setup     # assistant complet (modèle, outils…)
hermes model     # ou : choisir seulement fournisseur + modèle
hermes doctor    # vérifie que tout est en ordre
hermes           # premier chat : dis bonjour
```

Tes clés API vont dans le fichier `.env` de Hermes, que `hermes setup` et `hermes model` gèrent pour toi. Ne les mets jamais dans un `SOUL.md` ni dans un repo.

## Étape 3 : créer les profils de la team

Un profil, c'est un agent isolé. Commence par le kit de départ :

```bash
hermes profile create chief      --description "Orchestrateur : découpe les demandes et les dispatche sur le Kanban."
hermes profile create architect  --description "Conçoit : specs, architecture, tickets, revue de code."
hermes profile create coder      --description "Implémente une spec en TDD et ouvre une PR."
hermes profile create maintainer --description "Bugs, tri d'issues, CI, dépendances, rétros."
hermes profile list
```

Le Kanban lit la description pour savoir à qui confier quoi. Tu peux la changer plus tard avec `hermes profile describe <nom> --text "..."`. Par défaut, un nouveau profil reprend les clés et le modèle du profil courant (voir `hermes profile create --help`).

## Étape 4 : donner sa personnalité à chaque agent (`SOUL.md`)

Chaque fiche de [`profils/`](../profils/) contient un bloc SOUL.md à copier. Colle-le dans le `SOUL.md` du profil, puis remplace les `<placeholders>` :

| Système | Emplacement |
|---|---|
| Linux / macOS / WSL2 | `~/.hermes/profiles/<nom>/SOUL.md` |
| Windows | `%LOCALAPPDATA%\hermes\profiles\<nom>\SOUL.md` |

Ouvre la fiche, copie le bloc entre les balises `SOUL:START` et `SOUL:END` (sans les lignes ```` ``` ````), colle-le dans le fichier avec ton éditeur, puis personnalise.

Tu peux aussi choisir un modèle par profil. Chaque profil devient une commande, et ceci règle son modèle :

```bash
hermes -p architect model
hermes -p coder model
```

Mets un modèle fort en raisonnement sur `chief` et `architect`. Un modèle plus économique suffit pour `coder`, `maintainer` et les tâches répétitives.

## Étape 5 : installer les skills utiles

Voir [`skills/README.md`](../skills/README.md). Commandes de base :

```bash
hermes skills list                 # ce qui est déjà installé
hermes skills search <mot-clé>     # chercher dans les registres
hermes skills inspect <skill>      # lire avant d'installer
hermes -p coder skills install <skill>
```

Les skills sont rattachés à un profil. Installe chacun dans le profil qui s'en sert.

## Étape 6 : lancer le Kanban

```bash
hermes kanban init      # crée le tableau (idempotent)
hermes gateway run      # le dispatcher tourne dans la gateway (premier plan)
```

Laisse la gateway tourner dans un terminal (ou installe-la en service avec `hermes gateway install`). Elle lance automatiquement le profil assigné quand une tâche est prête.

## Étape 7 : premier projet perso

1. Crée un dossier pour ton projet et initialise git :

   ```bash
   mkdir mon-projet && cd mon-projet && git init -b main
   ```

2. Parle à ton orchestrateur :

   ```bash
   chief
   ```

   > « Je veux une petite API de todo en Python dans `<chemin-du-projet>`. Fais concevoir par architect, implémenter par coder, puis relire. »

3. Suis l'avancement :

   ```bash
   hermes kanban list
   hermes kanban show <id-de-tache>
   hermes kanban tail <id-de-tache>   # suivre les événements d'une tâche
   ```

4. Tu peux aussi créer une tâche à la main :

   ```bash
   hermes kanban create "Spec de l'API todo" --assignee architect --workspace dir:<chemin-du-projet> --body "Contexte, critères d'acceptation, contraintes."
   ```

5. Relis la PR toi-même avant de fusionner.

## Dépannage

| Problème | Piste |
|---|---|
| `hermes: command not found` | Recharge ton shell ou vérifie le `PATH` |
| Clé API manquante | `hermes model` |
| Quelque chose cloche | `hermes doctor` |
| Une tâche reste en `ready` | Gateway lancée ? Assigné = nom de profil existant (`hermes profile list`) ? |
| « Je lui ai dit de retenir et il a oublié » | Vérifie le `MEMORY.md` du bon profil ; voir [pourquoi Hermes](pourquoi-hermes.md) |

## Sécurité (à lire)

- Secrets uniquement dans `.env`. Ajoute `.env*` au `.gitignore` de tes projets.
- Active la protection de branche sur `main` (PR obligatoire) : tes agents passent par des PR, pas par des push directs.
- Ne donne pas à un agent des accès dont il n'a pas besoin (paiement, publication, envoi d'e-mails…).
