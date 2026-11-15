# Pack Hermes Team — ta team d'agents IA, prête à copier

> Mot-clé **MASIA** — épisode 2 « Quel harnais pour ta team IA ? »

Ce repo est un **modèle (template)** de team d'agents construite sur [Hermes Agent](https://hermes-agent.nousresearch.com/docs/) (open source, Nous Research). Il contient :

- les **fiches de rôle** de chaque agent, avec un `SOUL.md` générique à copier ;
- une **liste de skills utiles** par rôle ;
- un **guide d'installation pas à pas** pour débutant ;
- une page honnête **« pourquoi Hermes »**.

Tout est générique : remplace les `<placeholders>` par tes propres infos. Aucune clé ni donnée perso n'est fournie, et tu n'en mettras jamais dans un `SOUL.md`.

## Le principe

Un seul point d'entrée, **chief**, reçoit tes demandes. Il les découpe en tâches et les dispatche sur un **tableau Kanban** partagé à des agents spécialisés. Chaque agent est un **profil Hermes** séparé : sa config, sa mémoire, ses skills et sa personnalité (`SOUL.md`) à lui.

```
toi ──► chief ──► Kanban ──┬─► architect ──► coder ──► maintainer   (dev)
                           ├─► reasoning                            (analyse)
                           ├─► veille-tech ──► monteur              (contenu)
                           ├─► seo-expert                           (acquisition)
                           ├─► job-scout ──► job-writer             (candidatures / rédaction)
                           └─► juriste                              (admin / légal)
```

## La team

| Profil | Rôle | Pôle | Fiche |
|---|---|---|---|
| `chief` | Orchestrateur, ton seul point d'entrée : découpe, dispatche, suit, synthétise | Pilotage | [chief](profils/chief.md) |
| `reasoning` | Compagnon d'analyse : réfléchir, planifier, répondre sans coder | Pilotage | [reasoning](profils/reasoning.md) |
| `architect` | Conçoit : specs, modèle de domaine, architecture, tickets, revue de code | Dev | [architect](profils/architect.md) |
| `coder` | Implémente une spec en TDD sur une branche et ouvre une PR | Dev | [coder](profils/coder.md) |
| `maintainer` | Tri des issues, bugs (reproduire d'abord), CI, dépendances, rétros | Dev | [maintainer](profils/maintainer.md) |
| `veille-tech` | Capte le verbatim réel des communautés et en tire des scripts courts | Contenu | [veille-tech](profils/veille-tech.md) |
| `monteur` | Transforme un script en vidéo verticale selon ta charte graphique | Contenu | [monteur](profils/monteur.md) |
| `seo-expert` | Audit SEO / GEO / SEA priorisé (impact × effort) | Acquisition | [seo-expert](profils/seo-expert.md) |
| `job-scout` | Cherche, dédoublonne et classe des offres d'emploi | Carrière | [job-scout](profils/job-scout.md) |
| `job-writer` | Rédige CV, lettres, articles et posts à partir de faits vérifiés | Carrière / contenu | [job-writer](profils/job-writer.md) |
| `juriste` | Assistant juridique et administratif (ne remplace pas un professionnel) | Admin | [juriste](profils/juriste.md) |

Tu n'as pas besoin des 11. **Kit de départ conseillé pour un projet perso : `chief` + `architect` + `coder` + `maintainer`.** Ajoute les autres quand le besoin arrive.

## Prérequis

- Un ordinateur sous Linux, macOS (Apple Silicon), Windows ou WSL2 (voir la [matrice officielle](https://hermes-agent.nousresearch.com/docs/getting-started/platform-support)).
- Un accès à un modèle de langage : clé API d'un fournisseur (OpenRouter, Anthropic, OpenAI, DeepSeek…) ou abonnement compatible. Un modèle **solide en appel d'outils** est fortement conseillé pour l'orchestrateur et l'architecte.
- `git` et, pour la partie dev, un compte GitHub avec la CLI [`gh`](https://cli.github.com/).
- 30 minutes.

## Démarrer

1. Suis le **[guide d'installation](docs/installation.md)**.
2. Crée les profils à partir des **[fiches](profils/)**.
3. Installe les **[skills utiles](skills/README.md)**.
4. Lis **[pourquoi Hermes](docs/pourquoi-hermes.md)** pour comprendre ce qui rend la team meilleure avec le temps.

## Structure du repo

```
README.md                 ce fichier
docs/installation.md      guide pas à pas
docs/pourquoi-hermes.md   mémoire persistante, boucle de rétroaction
profils/                  une fiche par rôle + SOUL.md à copier
skills/README.md          skills utiles par rôle
LICENSE                   licence (à valider)
```

## Avertissement

Ce pack décrit une façon d'organiser des agents. Les agents peuvent se tromper : relis les PR, ne laisse jamais un agent publier, payer ou signer à ta place, et garde tes secrets dans le fichier `.env` de Hermes, jamais dans un `SOUL.md` ni dans un repo.
