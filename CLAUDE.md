# GardenMinder — CLAUDE.md

## Présentation du projet

Application PWA de gestion de jardin (arrosage, alertes, journal). Fichier unique : **`index.html`** (~6500 lignes). Pas de build, pas de framework, pas de dépendances npm — HTML/CSS/JS vanilla.

## Architecture du fichier `index.html`

| Lignes | Contenu |
|--------|---------|
| 1–26 | `<head>` : PWA manifest inline, Service Worker inline |
| 27–900 | `<style>` : tout le CSS (variables CSS, écrans, composants) |
| 930–2100 | `<body>` : HTML de tous les écrans |
| 2109–2256 | `PLANT_DB` : base de données des plantes |
| 2257–2344 | Constantes (sol, exposition, contenants) + état global |
| 2344–2500 | Navigation (`goTo`) + rendu écran principal |
| 2500–4960 | Logique métier : météo, arrosage, graphiques |
| 4962–5380 | Onglets : alertes, réglages, sol, mode avancé, reset |
| 5383–5924 | Journal de bord, gamification, partage social |
| 5939–6320 | Timelapse, mémoire vivante, badges |
| 6324–6526 | Auth, boot (`boot()`) |

## Écrans (id HTML)

- `screen-landing` — page d'accueil
- `screen-intro` — onboarding (3 slides)
- `screen-wizard` — configuration initiale (ville, plantes, sol)
- `screen-app` — app principale (onglets : accueil, journal, historique, alertes, réglages)
- `screen-plant` — fiche plante
- `screen-category` — sélection par catégorie
- `screen-trial-end` — fin de période d'essai
- `screen-auth` — connexion / inscription

## État global

```js
const state = { ... }  // ~ligne 2294
const LS_KEY = 'alertgarden_state_v4'  // clé localStorage
```

- `saveState()` / `loadState()` pour la persistance
- `isPremium()` / `trialDaysLeft()` pour le modèle freemium

## Conventions importantes

- **Ne jamais régénérer tout le fichier** — faire des modifications ciblées
- Variables CSS dans `:root` (ligne ~28) : utiliser les tokens existants (`--green-deep`, `--cream`, `--text-dark`, etc.)
- Polices : `--ff-display` (Playfair Display), `--ff-serif` (Fraunces), `--ff-body` (Outfit)
- Pas de commentaire superflu — le code se documente via les sections `// ── NOM ──`
- Pas de TypeScript, pas de bundler — JS vanilla ES6+

## Langue

Interface en **français**. Tout texte visible par l'utilisateur doit être en français.

## Ce qu'il ne faut pas toucher sans raison

- `CACHE_VERSION` (ligne ~22) — ne changer que si on veut invalider le cache SW
- `LS_KEY = 'alertgarden_state_v4'` — changer = perte des données utilisateur
- `PLANT_DB` — base de données figée, modifier avec précaution
- La fonction `boot()` (~ligne 6440) — point d'entrée de l'app

## Gardes-fous

- Ne jamais supprimer de fichiers sans lister exactement ce qui sera supprimé et demander confirmation.
- Ne jamais manipuler de secrets ou API keys.
- Avant de déclarer un livrable terminé, vérifier le résultat réel.
- Si incertain, proposer un test simple plutôt que de deviner.

## Workflow Claude Code

Pour chaque demande :
1. Localiser la section concernée avec grep ou Read(offset, limit)
2. Modifier uniquement les lignes nécessaires avec Edit
3. Commit + push sur la branche de travail
