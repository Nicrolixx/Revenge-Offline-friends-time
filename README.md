# Friend Status Notifier (plugin Revenge)

Surveille le statut Discord d'amis choisis. Quand l'un d'eux passe de **hors-ligne**
a un autre statut (en ligne / inactif / ne pas deranger), tu recois un **toast** dans
l'app. Le plugin memorise aussi la **derniere fois ou il a vu la personne en ligne**.

## Construire le plugin

Prerequis : Node.js 18+.

```bash
npm install
npm run build
```

Le dossier `dist/` contient alors `index.js` et `manifest.json`.

## Heberger et installer

Revenge charge un plugin depuis une **URL** pointant vers le dossier qui contient
`manifest.json`. Heberge le dossier `dist/` sur n'importe quel hebergement statique
(GitHub Pages, un repo raw GitHub, un petit serveur perso...), puis :

1. Revenge -> Reglages -> **Plugins** -> bouton **+**.
2. Colle l'URL du dossier (celle qui se termine par `.../dist/`).
3. Active le plugin.

## Configurer

1. Sur Discord, active le **Mode developpeur** (Reglages -> Avance) pour pouvoir
   copier l'ID d'un utilisateur (appui long sur le profil -> Copier l'ID).
2. Ouvre les reglages du plugin, colle l'ID de l'ami, appuie sur **Ajouter**.
3. Chaque ami surveille affiche son statut actuel et sa derniere connexion vue.
   **Appui long** sur une ligne pour le retirer.

## Limites importantes (a lire)

- **Arriere-plan** : un plugin s'execute a l'interieur de Discord. Il ne recoit les
  changements de statut que tant que la connexion (gateway) est active, c'est-a-dire
  surtout quand l'app est ouverte/au premier plan. Aucun plugin ne peut forcer le
  systeme a maintenir Discord vivant en arriere-plan. Sur Android tu peux ameliorer
  ca en desactivant l'optimisation batterie pour Discord/Revenge ; sur iOS c'est
  quasiment impossible de tourner longtemps en fond. Au relancement, le plugin
  resynchronise l'etat courant.
- **Notification** : ce que le plugin peut faire de facon fiable, c'est un **toast**
  in-app quand l'app est active. Une vraie notification push systeme n'est pas
  accessible depuis un plugin.
- **Derniere connexion** : Discord n'expose pas de "vu pour la derniere fois" pour
  les autres utilisateurs. La valeur affichee est le dernier moment ou **ce plugin**
  a observe la personne en ligne (depuis son installation), pas un historique reel.
- **Invisible** : une personne en mode invisible apparait hors-ligne pour toi ; elle
  est donc indetectable.

## Note de compatibilite

Le code utilise les imports `@vendetta/*` (couche de compatibilite que Revenge
fournit pour les plugins Vendetta). Si une version future de Revenge change son
format de build/chargement, la logique reste la meme ; seul le pipeline de build
(`build.mjs`) serait a aligner sur le template officiel a jour.
