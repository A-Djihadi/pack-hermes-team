# Pack Hermes team : ta team d'agents IA, prête à copier

> Mot-clé MASIA, épisode 2 : « Quel harnais pour ta team IA ? »

Ce repo est un modèle de team d'agents construite sur [Hermes Agent](https://hermes-agent.nousresearch.com/docs/), un projet open source de Nous Research. Tu y trouves :

- la fiche de rôle de chaque agent, avec un `SOUL.md` générique à copier ;
- une liste de skills utiles, rangés par rôle ;
- un guide d'installation pas à pas, pensé pour débuter ;
- une page « pourquoi Hermes » qui dit aussi ce que l'outil ne fait pas.

Tout est générique. Remplace les `<placeholders>` par tes propres infos. Le pack ne contient aucune clé ni donnée perso, et tu n'en mettras jamais dans un `SOUL.md`.

## Le principe

Tu parles à un seul agent, chief. Il découpe ta demande en tâches et les pose sur un tableau Kanban partagé. Chaque tâche part ensuite chez un agent spécialisé. Un agent, c'est un profil Hermes séparé, avec sa config, sa mémoire, ses skills et sa personnalité (`SOUL.md`).

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
| `chief` | Orchestrateur et seul point d'entrée : il découpe, dispatche, suit et synthétise | Pilotage | [chief](profils/chief.md) |
| `reasoning` | Compagnon d'analyse : réfléchir, planifier, répondre sans coder | Pilotage | [reasoning](profils/reasoning.md) |
| `architect` | Conçoit : specs, modèle de domaine, architecture, tickets, revue de code | Dev | [architect](profils/architect.md) |
| `coder` | Implémente une spec en TDD sur une branche et ouvre une PR | Dev | [coder](profils/coder.md) |
| `maintainer` | Tri des issues, bugs (reproduire d'abord), CI, dépendances, rétros | Dev | [maintainer](profils/maintainer.md) |
| `veille-tech` | Capte le verbatim réel des communautés et en tire des scripts courts | Contenu | [veille-tech](profils/veille-tech.md) |
| `monteur` | Transforme un script en vidéo verticale selon ta charte graphique | Contenu | [monteur](profils/monteur.md) |
| `seo-expert` | Audit SEO, GEO et SEA priorisé (impact × effort) | Acquisition | [seo-expert](profils/seo-expert.md) |
| `job-scout` | Cherche, dédoublonne et classe des offres d'emploi | Carrière | [job-scout](profils/job-scout.md) |
| `job-writer` | Rédige CV, lettres, articles et posts à partir de faits vérifiés | Carrière / contenu | [job-writer](profils/job-writer.md) |
| `juriste` | Assistant juridique et administratif (ne remplace pas un professionnel) | Admin | [juriste](profils/juriste.md) |

Inutile de créer les 11 d'un coup. Pour un projet perso, commence avec `chief`, `architect`, `coder` et `maintainer`. Tu ajouteras les autres quand tu en auras besoin.

## Prérequis

- Un ordinateur sous Linux, macOS (Apple Silicon), Windows ou WSL2. La [matrice officielle](https://hermes-agent.nousresearch.com/docs/getting-started/platform-support) donne le détail.
- Un accès à un modèle de langage : une clé API (OpenRouter, Anthropic, OpenAI, DeepSeek…) ou un abonnement compatible. Pour l'orchestrateur et l'architecte, prends un modèle solide en appel d'outils.
- `git` et, pour la partie dev, un compte GitHub avec la CLI [`gh`](https://cli.github.com/).
- 30 minutes.

## Démarrer

1. Suis le [guide d'installation](docs/installation.md).
2. Crée les profils à partir des [fiches](profils/).
3. Installe les [skills utiles](skills/README.md).
4. Lis [pourquoi Hermes](docs/pourquoi-hermes.md). Tu y verras comment la team s'améliore avec le temps.

## Structure du repo

```
README.md                 ce fichier
docs/installation.md      guide pas à pas
docs/pourquoi-hermes.md   mémoire persistante, boucle de rétroaction
profils/                  une fiche par rôle + SOUL.md à copier
skills/README.md          skills utiles par rôle
LICENSE                   texte de la licence CC-BY-4.0
```

## Avertissement

Ce pack décrit une façon d'organiser des agents, et les agents se trompent. Relis les PR. Ne laisse jamais un agent publier, payer ou signer à ta place. Garde tes secrets dans le fichier `.env` de Hermes, jamais dans un `SOUL.md` ni dans un repo.

## Licence

Licence : CC-BY-4.0 (attribution Djihadi Ahamdy). Le texte complet est dans [LICENSE](LICENSE).
