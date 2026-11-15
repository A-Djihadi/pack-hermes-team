# Skills utiles

Un skill est une procédure réutilisable (un fichier `SKILL.md`, parfois avec des scripts) que l'agent charge quand la tâche s'y prête. Les skills s'installent par profil.

```bash
hermes skills list                        # skills installés (colonne Source : builtin / official / local / hub)
hermes skills search <mot-clé>            # chercher dans les registres
hermes skills inspect <skill>             # lire AVANT d'installer
hermes -p <profil> skills install <skill>
```

> Sécurité : un skill, c'est du contenu que ton agent va suivre et parfois exécuter. Lis-le (`inspect`) avant de l'installer, comme tu lirais un script.

## 1. Skills livrés avec Hermes

Ils apparaissent avec la source `builtin` ou `official` dans `hermes skills list` ; en général, rien à installer.

| Skill | Ce qu'il fait | Profils | Cas d'usage |
|---|---|---|---|
| `test-driven-development` | Impose le cycle red → green → refactor | coder, maintainer | Implémenter un ticket test d'abord |
| `systematic-debugging` | Débogage en 4 phases : comprendre avant de corriger | maintainer | Bug intermittent, CI rouge |
| `requesting-code-review` | Revue avant commit : scan sécurité, contrôles qualité | coder, architect | Avant d'ouvrir une PR |
| `github` | PR, issues, revues, dépôts via la CLI `gh` | coder, maintainer, architect | Ouvrir ou relire une PR |
| `spike` | Expériences jetables pour valider une idée | architect, coder | Tester une lib avant de l'adopter |
| `dogfood` | QA exploratoire d'une web-app, rapport de bugs | maintainer | Tester son app avant la mise en ligne |
| `grounded-citations` | Réponses et documents sourcés et vérifiables | reasoning, juriste, seo-expert | Toute affirmation à sourcer |
| `arxiv` | Recherche d'articles scientifiques | veille-tech, reasoning | Veille IA |
| `competitor-news-monitor` | Surveiller l'actualité d'entreprises nommées | veille-tech | Veille concurrentielle |
| `youtube-content` | Transcriptions YouTube → résumés, threads, articles | veille-tech, job-writer | Recycler une vidéo en post |
| `humanizer` | Retirer les tics d'écriture « IA » d'un texte | job-writer, veille-tech | Relire un post avant publication |
| `obsidian` | Lire et écrire dans un coffre Obsidian | reasoning | Second cerveau |
| `llm-wiki` | Base de connaissances markdown interliée | reasoning, chief | Documentation de projet |
| `hermes-agent` | Configurer et dépanner Hermes lui-même | tous | « Comment je fais X avec Hermes ? » |

## 2. Skills de méthode (hors Hermes)

Ces skills ne sont pas livrés avec Hermes : ils viennent de la communauté ou ont été écrits pour la team. Cherche-les avec `hermes skills search <nom>`, ou écris ta propre version (section 3).

| Skill | Ce qu'il fait | Profils | Cas d'usage |
|---|---|---|---|
| `grill-me` / `grilling` | Interview serrée pour durcir un plan ou une conception | architect, reasoning | Avant d'écrire une spec |
| `to-spec` | Transforme une conversation en spec | architect | Fin d'un atelier de conception |
| `to-tickets` | Découpe un plan ou une spec en tickets ordonnés | architect, chief | Préparer le travail de coder |
| `domain-modeling` | Construire et affiner le modèle de domaine | architect | Démarrage d'un projet |
| `codebase-design` | Vocabulaire commun pour concevoir des modules profonds | architect | Revue d'architecture |
| `improve-codebase-architecture` | Repérer les opportunités de refactor | architect, maintainer | Code devenu illisible |
| `code-review` | Relire les changements depuis un point fixe | architect, maintainer | Relecture de PR |
| `tdd` / `implement-spec` | Implémenter une spec étape par étape | coder | Ticket prêt à coder |
| `triage` / `diagnosing-bugs` / `retro` | Tri d'issues, diagnostic, rétrospective | maintainer | Hygiène de projet |
| `handoff` | Compacter une conversation en document de passation | tous | Passer le relais à un autre agent |
| `writing-for-agents` | Écrire des documents lisibles par des agents | architect, chief | Rédiger un brief ou un SOUL.md |
| `devsecops-github` | Checklist DevSecOps pour git, GitHub Actions, images | architect, maintainer | Toute CI ou image Docker |

## 3. Écrire tes propres skills

Le plus utile, c'est de laisser la team écrire ses propres skills. Quand un agent a trouvé la bonne façon de faire une tâche récurrente, demande-lui « enregistre ça comme skill ». Il crée un `SKILL.md` dans son profil, chargé quand c'est pertinent. Le curateur archive ceux qui ne servent plus (voir [pourquoi Hermes](../docs/pourquoi-hermes.md)).

Bonnes pratiques :

- un skill = une procédure, avec une description courte qui dit quand l'utiliser ;
- des règles impératives et leur raison, pas un journal de bord ;
- jamais de secret dans un skill.

Idées de skills perso à créer : la checklist de livraison de tes vidéos, ta procédure de déploiement, le format de ton rapport hebdo.
