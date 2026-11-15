# Installation pas à pas

Guide pour débutant. Les commandes viennent de la [documentation officielle](https://hermes-agent.nousresearch.com/docs/getting-started/installation). En cas de doute, la doc fait foi et `hermes <commande> --help` donne les options exactes de ta version.

## Étape 0 — Prérequis

- Linux, macOS (Apple Silicon), Windows ou WSL2.
- Une clé API d'un fournisseur de modèles (ou un abonnement compatible).
- `git` installé. Pour la partie dev : un compte GitHub et la CLI `gh` (`gh auth login`).

## Étape 1 — Installer Hermes

**Linux / macOS / WSL2**

```bash
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
source ~/.bashrc   # ou ~/.zshrc
```

**Windows (PowerShell)**

```powershell
iex (irm https://hermes-agent.nousresearch.com/install.ps1)
```

Tu préfères une app ? Des paquets Desktop (Windows / macOS) sont disponibles sur le [site Hermes](https://hermes-agent.nousresearch.com/).

> Bonne pratique : lis un script avant de l'exécuter (`curl -fsSL <url> | less`).

## Étape 2 — Configurer le modèle et vérifier

```bash
hermes setup     # assistant complet (modèle, outils…)
hermes model     # ou : choisir seulement fournisseur + modèle
hermes doctor    # vérifie que tout est en ordre
hermes           # premier chat : dis bonjour
```

Tes clés API vont dans le fichier `.env` de Hermes (géré par `hermes setup` / `hermes model`). **Jamais** dans un `SOUL.md`, ni dans un repo.

## Étape 3 — Créer les profils de la team

Un profil = un agent isolé. Commence par le kit de départ :

```bash
hermes profile create chief      --description "Orchestrateur : découpe les demandes et les dispatche sur le Kanban."
hermes profile create architect  --description "Conçoit : specs, architecture, tickets, revue de code."
hermes profile create coder      --description "Implémente une spec en TDD et ouvre une PR."
hermes profile create maintainer --description "Bugs, tri d'issues, CI, dépendances, rétros."
hermes profile list
```

La description sert au Kanban pour savoir à qui confier quoi (modifiable plus tard avec `hermes profile describe <nom> --text "..."`). Par défaut, un nouveau profil reprend les clés et le modèle du profil courant (voir `hermes profile create --help`).

## Étape 4 — Donner sa personnalité à chaque agent (`SOUL.md`)

Chaque fiche de [`profils/`](../profils/) contient un bloc **SOUL.md à copier**. Colle-le dans le `SOUL.md` du profil, puis remplace les `<placeholders>` :

| Système | Emplacement |
|---|---|
| Linux / macOS / WSL2 | `~/.hermes/profiles/<nom>/SOUL.md` |
| Windows | `%LOCALAPPDATA%\hermes\profiles\<nom>\SOUL.md` |

Ouvre la fiche, copie le bloc entre les balises `SOUL:START` et `SOUL:END` (sans les lignes ```` ``` ````), colle-le dans le fichier avec ton éditeur, puis personnalise.

**Modèle par profil (optionnel)** : chaque profil devient une commande. Pour choisir un modèle propre à un profil :

```bash
hermes -p architect model
hermes -p coder model
```

Conseil : un modèle fort en raisonnement pour `chief` et `architect`, un modèle plus économique pour `coder`, `maintainer` et les tâches répétitives.

## Étape 5 — Installer les skills utiles

Voir [`skills/README.md`](../skills/README.md). Commandes de base :

```bash
hermes skills list                 # ce qui est déjà installé
hermes skills search <mot-clé>     # chercher dans les registres
hermes skills inspect <skill>      # lire avant d'installer
hermes -p coder skills install <skill>
```

Les skills sont **par profil** : installe chaque skill dans le profil qui en a besoin.

## Étape 6 — Lancer le Kanban

```bash
hermes kanban init      # crée le tableau (idempotent)
hermes gateway run      # le dispatcher tourne dans la gateway (premier plan)
```

Laisse la gateway tourner dans un terminal (ou installe-la en service avec `hermes gateway install`). Elle lance automatiquement le profil assigné quand une tâche est prête.

## Étape 7 — Premier projet perso

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

5. **Relis la PR toi-même** avant de fusionner.

## Dépannage

| Problème | Piste |
|---|---|
| `hermes: command not found` | Recharge ton shell ou vérifie le `PATH` |
| Clé API manquante | `hermes model` |
| Quelque chose cloche | `hermes doctor` |
| Une tâche reste en `ready` | Gateway lancée ? Assigné = nom de profil **existant** (`hermes profile list`) ? |
| « Je lui ai dit de retenir et il a oublié » | Vérifie le `MEMORY.md` du **bon profil** ; voir [pourquoi Hermes](pourquoi-hermes.md) |

## Sécurité (à lire)

- Secrets uniquement dans `.env`. Ajoute `.env*` au `.gitignore` de tes projets.
- Active la protection de branche sur `main` (PR obligatoire) : tes agents passent par des PR, pas par des push directs.
- Ne donne pas à un agent des accès dont il n'a pas besoin (paiement, publication, envoi d'e-mails…).
