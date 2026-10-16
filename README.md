# voc.md — mise en ligne sur GitHub Pages

## Contenu du dossier

| Fichier | Rôle |
|---|---|
| `index.html` | La page d'accueil (HTML/CSS/JS autonome, aucune dépendance sauf Google Fonts) |
| `voc.md` | Le skill complet, lu par les agents à l'adresse `https://voc.md/voc.md` |
| `downloads/voc-md-skill.zip` | Le même skill packagé pour Claude (dossier `voc-md/` + `SKILL.md`) |
| `llms.txt` | Point d'entrée standard pour les agents |
| `assets/chatgpt.png`, `assets/claude.png` | Logos des CTA (64×64) |
| `favicon.svg` | Favicon |
| `robots.txt`, `sitemap.xml` | Indexation |
| `CNAME` | Déclare le domaine personnalisé `voc.md` à GitHub Pages |
| `.nojekyll` | Désactive Jekyll — **indispensable** : sans lui, GitHub Pages transforme `voc.md` en page HTML et l'URL `/voc.md` ne sert plus le markdown brut |

## À compléter avant la mise en ligne

Deux liens sont des emplacements vides dans `index.html` (chercher `TODO-`) :

| Emplacement | À remplacer par |
|---|---|
| `TODO-GITHUB-REPO-URL` (2 occurrences) | L'URL du repo GitHub public, ex. `https://github.com/<organisation>/voc.md` — le lien « Contribute » renvoie vers les issues et pull requests |
| `TODO-TERMS-URL` | Page de conditions d'utilisation (à rédiger, ou lien vers celles de Screeb) |

## Mise en ligne

1. **Créer le repo.** Sur GitHub, créer un repo **public** (GitHub Pages est gratuit sur les repos publics avec un compte gratuit), par exemple `voc.md` sous l'organisation Screeb.
2. **Déposer les fichiers** à la racine du repo, en gardant les dossiers `assets/` et `downloads/`.
   - Par l'interface web : « Add file › Upload files », glisser tout le contenu du dossier.
   - Attention : le Finder macOS masque les fichiers commençant par un point. Si `.nojekyll` n'apparaît pas dans le repo après l'upload, le créer avec « Add file › Create new file », nom `.nojekyll`, contenu vide.
3. **Activer Pages.** Settings › Pages › Build and deployment › Source : « Deploy from a branch », branche `main`, dossier `/ (root)`. Enregistrer.
4. **Brancher le domaine.** Settings › Pages › Custom domain : `voc.md` (le fichier `CNAME` le pré-remplit normalement). Puis, chez le registrar du domaine, créer :
   - 4 enregistrements A sur l'apex `voc.md` : `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
   - 4 enregistrements AAAA (IPv6) : `2606:50c0:8000::153`, `2606:50c0:8001::153`, `2606:50c0:8002::153`, `2606:50c0:8003::153`
   - optionnel : un CNAME `www` → `<organisation>.github.io`
5. **Activer HTTPS.** Une fois le DNS propagé (de quelques minutes à quelques heures), cocher « Enforce HTTPS » dans Settings › Pages.
6. **Sécuriser le domaine.** Dans les settings de l'organisation GitHub › Pages › « Add a domain », vérifier `voc.md` (enregistrement TXT). Ça empêche quelqu'un d'autre de le rattacher à son propre repo.

## Vérifications après mise en ligne

- `https://voc.md/` affiche la page.
- `curl -I https://voc.md/voc.md` renvoie un statut 200 (et pas une redirection vers une page HTML) : c'est le test qui prouve que `.nojekyll` est bien pris en compte.
- `curl https://voc.md/voc.md` renvoie le markdown brut.
- `https://voc.md/downloads/voc-md-skill.zip` se télécharge.
- Les boutons « Open in ChatGPT » et « Open in Claude » ouvrent bien l'outil avec le prompt.

## Ce qu'on perd par rapport à Vercel

GitHub Pages ne permet ni en-têtes personnalisés ni réécritures selon l'agent. Un agent qui appelle `https://voc.md/` reçoit donc le HTML. Ce n'est pas bloquant : le prompt court pointe directement vers `https://voc.md/voc.md`, et le prompt complet est aussi dans la page.

## Les CTA

- **Open in ChatGPT** : `https://chatgpt.com/?hints=search&q=…` — ChatGPT ouvre et envoie le prompt automatiquement.
- **Open in Claude** : `https://claude.ai/new?q=…` — Claude ouvre une nouvelle conversation avec le prompt pré-rempli.
- Ces deux paramètres ne sont pas garantis contractuellement : à tester avant la mise en ligne, puis de temps en temps.

## Mettre à jour le prompt

Le prompt complet existe en trois exemplaires, à modifier ensemble :

1. `voc.md`
2. le bloc « The full prompt » de `index.html` (balise `<pre id="full-prompt">`)
3. `SKILL.md` dans `downloads/voc-md-skill.zip` (même contenu, précédé de l'en-tête `name` / `description`) — refaire le ZIP avec le dossier `voc-md/` à sa racine

Le prompt court (« Read https://voc.md/voc.md… ») existe en trois exemplaires : le bloc `#short-prompt` de `index.html` et les URL encodées des boutons « Open in ChatGPT » et « Open in Claude ».

Avantage de GitHub : chaque modification est versionnée, et le lien « Contribute » permet à d'autres de proposer des améliorations de la méthode par pull request.
