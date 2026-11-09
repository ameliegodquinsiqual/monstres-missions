# Monstres & Missions

Appli familiale pour téléphone : chaque enfant a son petit monstre, coche ses missions du jour, gagne des points, garde sa flamme allumée jour après jour et échange ses points contre des récompenses. Les parents valident, donnent des bonus ou malus de comportement et gèrent tout depuis l'espace Parents.

- Hébergement : **GitHub Pages** (gratuit)
- Synchro entre téléphones : **Firebase Firestore** (gratuit pour un usage familial)
- S'installe sur l'écran d'accueil et s'ouvre même sans connexion

Sans Firebase configuré, l'appli marche quand même, en mode local : les données restent sur un seul téléphone.

---

## 1. Mettre l'appli sur GitHub Pages

1. Sur GitHub, créez un nouveau dépôt, par exemple `monstres-missions`. Il doit être **public** pour GitHub Pages gratuit (le code est public, vos données ne le sont pas : elles sont dans Firebase).
2. Dans le dépôt : **Add file › Upload files**, glissez **tous les fichiers de ce dossier**, puis **Commit changes**.
3. **Settings › Pages** : dans *Build and deployment*, choisissez *Deploy from a branch*, branche `main`, dossier `/ (root)`, puis **Save**.
4. Après une ou deux minutes, l'appli est en ligne à l'adresse `https://VOTRE-PSEUDO.github.io/monstres-missions/`.

## 2. Créer la base Firebase (environ 5 minutes)

1. Allez sur [console.firebase.google.com](https://console.firebase.google.com) et connectez-vous avec un compte Google.
2. **Créer un projet** (ou *Ajouter un projet*) : donnez un nom, par exemple `monstres-missions`. Google Analytics n'est pas utile, vous pouvez le désactiver.
3. Dans le menu de gauche : **Créer › Firestore Database › Créer une base de données**.
   - Si on vous demande une édition, prenez l'édition **Standard**.
   - Emplacement : une région en Europe, par exemple `europe-west9 (Paris)`. Ce choix est définitif.
   - Mode : **production**.

## 3. Publier les règles de sécurité

1. Dans **Firestore Database**, ouvrez l'onglet **Règles**.
2. Remplacez tout le contenu par celui du fichier `firestore.rules` de ce dépôt.
3. Cliquez sur **Publier**.

## 4. Brancher l'appli sur Firebase

1. Dans la console Firebase : roue dentée **› Paramètres du projet**, section **Vos applications**, cliquez sur l'icône **Web** `</>`.
2. Donnez un surnom (par exemple `appli`), ne cochez pas *Firebase Hosting*, puis **Enregistrer l'application**.
3. Firebase affiche un bloc `const firebaseConfig = { apiKey: "...", ... }`. Gardez cette page ouverte.
4. Sur GitHub, ouvrez le fichier `config.js`, cliquez sur le crayon (**Edit**), et recopiez chaque valeur entre les guillemets correspondants (`apiKey`, `authDomain`, `projectId`, `storageBucket`, `messagingSenderId`, `appId`).
5. **Commit changes**. Une minute plus tard, rechargez l'appli : elle est synchronisée.

## 5. Première utilisation

1. Sur votre téléphone, ouvrez l'adresse GitHub Pages et touchez **Créer notre famille**.
2. Suivez les 4 étapes : nom de la famille, code parent, enfants, missions et récompenses de départ.
3. Pour l'autre parent : **Parents › Réglages › Ajouter un téléphone**. Il scanne le QR code avec l'appareil photo de son téléphone, ou vous lui envoyez le lien.

### Installer sur l'écran d'accueil

- **iPhone** (Safari) : bouton Partager › **Sur l'écran d'accueil**.
- **Android** (Chrome) : menu ⋮ › **Installer l'application** (ou *Ajouter à l'écran d'accueil*).

Sur iPhone, l'appli installée a sa propre mémoire, séparée de Safari. À sa première ouverture, choisissez **La famille existe déjà ?** et collez le lien ou le code famille (visible dans *Ajouter un téléphone* sur un téléphone déjà relié). Installer l'appli est conseillé sur iPhone : Safari peut effacer les données d'un site non ouvert pendant plusieurs jours.

---

## Comment marchent les points

- **Missions** : chacune rapporte des points. Cochée par un enfant, elle passe *en attente* jusqu'à ce qu'un parent la valide. En mode parent, elle est validée tout de suite. La validation peut se désactiver dans les réglages.
- **Flamme** : une journée où toutes les missions prévues sont faites ajoute un jour à la série et rapporte un bonus de +1 point par jour de suite (plafonné à 10 jours, réglable). Il y a des paliers en plus : 3 jours +5, 7 jours +15, 14 jours +30, 30 jours +60, 60 jours +100, 100 jours +150. Un jour sans mission prévue ne casse pas la série. Un jour incomplet la remet à zéro.
- **Comportement** : bonus ou malus donnés par un parent (code demandé).
- **Niveaux** : ils montent avec les points gagnés et le monstre évolue (joues, antennes, étoile, cape, couronne, étincelles). Les malus et les achats ne font jamais baisser le niveau.
- **Boutique** : les points s'échangent contre les récompenses que vous définissez, avec confirmation d'un parent.

## Sécurité et confidentialité

- Chaque famille est rangée sous un **code secret de 20 caractères** créé au hasard sur le premier téléphone. Les règles Firestore interdisent de lister les familles : sans le code, impossible de lire ou modifier vos données.
- Toute personne qui a votre lien famille peut voir et modifier vos données : ne le partagez qu'avec la famille.
- Le **code parent** à 4 chiffres empêche les enfants de valider eux-mêmes ou de changer les réglages. Ce n'est pas un mot de passe fort.
- Les valeurs de `config.js` ne sont pas secrètes (c'est normal avec Firebase). Pour aller plus loin, vous pouvez limiter la clé à votre site : dans la [console Google Cloud](https://console.cloud.google.com/apis/credentials) du projet, ouvrez la clé API › *Restrictions relatives aux applications* › *Sites Web*, et ajoutez `https://VOTRE-PSEUDO.github.io/*`.
- Le plan gratuit de Firebase (Spark) couvre largement une famille. Aucune carte bancaire n'est nécessaire.

## Fichiers

| Fichier | Rôle |
| --- | --- |
| `index.html` | L'appli complète |
| `config.js` | Votre configuration Firebase |
| `firestore.rules` | Règles de sécurité à coller dans Firebase |
| `manifest.webmanifest`, `sw.js` | Installation sur l'écran d'accueil et ouverture hors connexion |
| `icon-*.png`, `apple-touch-icon.png` | Icônes de l'appli |

Pour publier une nouvelle version d'`index.html`, remplacez le fichier sur GitHub. Si un téléphone garde l'ancienne version, fermez puis rouvrez l'appli.
