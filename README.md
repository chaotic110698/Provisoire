# 🎡 Roue de Fortune

Roue de fortune interactive, jouable au doigt sur mobile comme à la souris sur ordinateur.
**Tout tient dans un seul fichier : [`index.html`](index.html).** Aucune dépendance, aucun
CDN, aucun serveur — il suffit d'ouvrir le fichier dans un navigateur.

## Utilisation

Ouvrez `index.html` (double-clic, ou déposez-le dans un onglet). Vous pouvez aussi
l'héberger tel quel sur n'importe quel hébergement statique : GitHub Pages, Netlify,
une clé USB, un partage réseau…

## Fonctionnalités

**Onglet Roue**
- Roue SVG générée à partir des seuls résultats cochés, une couleur par catégorie.
- **Parts pondérées** : chaque résultat porte un poids de 1 à 99, la part occupe un angle
  proportionnel et sort d'autant plus souvent.
- Rotation animée (~3 s) avec accélération puis longue décélération, pilotée en
  `requestAnimationFrame` sur un simple `transform` (composité par le GPU).
- Aiguille fixe au sommet ; le secteur désigné correspond toujours au résultat annoncé.
- Titre + description du résultat, confettis, **historique des 50 derniers tirages**
  (horodatés, exportables en CSV) et statistiques de fréquence.
- **Mode présentation** plein écran, pour projeter la roue devant un groupe
  (<kbd>Échap</kbd> pour sortir).
- **Au-delà de 40 résultats**, les parts portent un numéro et une légende s'affiche à côté.
- Lancement au clic sur « Lancer », sur le moyeu central, ou avec la touche <kbd>Entrée</kbd>.

**Onglet Gestion**
- Création rapide : nom, description, catégorie (existante ou nouvelle), poids, couleur.
- **Réglages de la roue** : *tirage sans remise* (le résultat sorti est décoché
  automatiquement) et *éviter le dernier résultat tiré*.
- Catégories pliables, renommables, recolorables, supprimables, **réordonnables** —
  l'ordre de la liste est celui des parts sur la roue.
- **Recherche** insensible aux accents, **tout cocher / tout décocher** sur ce qui est affiché.
- Édition d'un résultat dans une modale (nom, description, catégorie, poids).
- **Annulation** proposée après chaque suppression, réinitialisation ou import.
- Import / export JSON (fichier ou copier-coller), réinitialisation complète.

**Plusieurs roues**
- Autant de roues nommées que voulu (« Repas du soir », « Corvées », « Ordre de passage »…),
  chacune avec ses catégories, résultats, réglages, historique et statistiques.
- Création, renommage, duplication et suppression depuis la barre latérale.

**Packs thématiques**
- L'export laisse choisir les catégories à emporter. Toutes cochées, c'est la roue
  complète (réglages compris) ; une sélection partielle produit un **pack** : un jeu de
  catégories réutilisable, à fusionner dans n'importe quelle roue.
- Chaque catégorie porte un bouton « pack » qui l'exporte seule, en un clic.
- À l'import, l'application annonce ce qu'elle a reconnu (« Pack « Films du soir » :
  1 catégorie, 12 résultats ») avant que vous ne choisissiez. Fusionner est l'action
  par défaut : les catégories de même nom sont réutilisées, les doublons ignorés, et
  les réglages de la roue d'accueil sont conservés — un pack n'en transporte pas.

**Partage**
- Le bouton « Partager » encode la roue entière dans le fragment de l'URL. Envoyez le lien :
  la personne qui l'ouvre se voit proposer d'ouvrir la roue ou de la fusionner avec la sienne.
  Toujours sans compte ni serveur.

**Présentation**
- 4 styles : **Casino** (rouge/noir/or, effet 3D), **Feutré** (bordeaux, serif, texture),
  **Blanc classique** (minimaliste, contrasté) et **Velours** (noir profond, rouge velours
  et laiton, grain de tissu et lustre, display Didot).
- Mode nuit décliné pour chacun des quatre styles ; par défaut il suit la préférence
  système du navigateur. Velours assume son monde sombre dans les deux modes : le mode
  nuit y baisse les lumières au lieu d'inverser la palette.
- **Français et anglais**, détectés depuis la langue du navigateur et modifiables à tout moment.
- Barre latérale permanente sur écran large, tiroir escamotable sous 640 px.
- Feuille de style d'impression : la liste des résultats ou le résultat du tirage sortent
  proprement sur papier, sans les commandes.

## Données

Tout est conservé en `localStorage` (clé `roue-fortune-v2`) : roues, catégories, résultats,
poids, cases cochées, réglages, style, mode nuit, langue, sons, historique et statistiques.
Rien ne quitte le navigateur — il n'y a ni backend ni requête réseau. Une sauvegarde issue
de la version précédente (`roue-fortune-v1`) est reprise automatiquement sous forme de
première roue nommée.

Si le stockage est indisponible (navigation privée, quota, réglages restrictifs),
l'application reste pleinement utilisable et le signale par un bandeau, plutôt que de
perdre silencieusement les modifications.

Format d'échange (bouton « Exporter ») :

```json
{
  "app": "roue-de-fortune",
  "version": 2,
  "kind": "wheel",
  "name": "Ma roue",
  "options": { "removeOnDraw": false, "avoidRepeat": false },
  "categories": [{ "id": "c1", "name": "Boissons", "color": "#d64545" }],
  "results": [{ "id": "r1", "name": "Café", "desc": "…", "cat": "c1", "active": true, "weight": 1 }]
}
```

Un pack a exactement le même format, avec `"kind": "pack"`, un sous-ensemble de
catégories et pas de clé `options`. Les deux se rechargent par le même bouton
« Importer ».

À l'import, « Remplacer » écrase la roue active, « Fusionner » ajoute ce qui manque
(les catégories de même nom sont réutilisées, les doublons ignorés). Les deux sont annulables.

## Accessibilité

Onglets ARIA navigables aux flèches, piège à focus et fermeture par <kbd>Échap</kbd>
dans les modales, zone de résultat en `aria-live`, libellés sur toutes les icônes,
cibles tactiles d'au moins 44 × 44 px, et respect de `prefers-reduced-motion`
(animations et confettis désactivés).

## Développement

Le fichier est organisé en sections numérotées et commentées : jetons de style et thèmes,
structure, onglet Roue, onglet Gestion, modale, ajouts (profils, poids, tri, présentation),
impression, puis le JavaScript (utilitaires, traductions, état, thèmes et navigation, roue,
gestion, modale, import/export/partage, effets, démarrage).

Modifier une couleur de thème revient à changer quelques variables CSS dans le bloc
`[data-skin="…"]` correspondant. Ajouter une langue revient à ajouter une entrée au
dictionnaire `I18N` : le balisage porte des attributs `data-i18n`, `data-i18n-ph` et
`data-i18n-al` appliqués par `applyLang()`.
