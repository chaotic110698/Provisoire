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
- Rotation animée (~3 s) avec accélération puis longue décélération, pilotée en
  `requestAnimationFrame` sur un simple `transform` (composité par le GPU).
- Aiguille fixe au sommet ; le secteur désigné correspond toujours au résultat annoncé.
- Titre + description du résultat, confettis, historique des 10 derniers tirages et
  statistiques de fréquence.
- Lancement au clic sur « Lancer », sur le moyeu central, ou avec la touche <kbd>Entrée</kbd>.

**Onglet Gestion**
- Création rapide : nom, description, catégorie (existante ou nouvelle), couleur.
- Catégories pliables, renommables, recolorables, supprimables (avec confirmation).
- Case à cocher par résultat, et case d'en-tête à trois états pour toute une catégorie.
- Édition d'un résultat dans une modale (nom, description, catégorie).
- Import / export JSON (fichier ou copier-coller), réinitialisation complète.

**Présentation**
- 3 styles : **Casino** (rouge/noir/or, effet 3D), **Feutré** (bordeaux, serif, texture),
  **Blanc classique** (minimaliste, contrasté).
- Mode nuit décliné pour chacun des trois styles ; par défaut il suit la préférence
  système du navigateur.
- Barre latérale permanente sur écran large, tiroir escamotable sous 640 px.

## Données

Tout est conservé en `localStorage` (clé `roue-fortune-v1`) : catégories, résultats,
cases cochées, style, mode nuit, sons, dernier résultat, historique et statistiques.
Rien ne quitte le navigateur — il n'y a ni backend ni requête réseau.

Format d'échange (bouton « Exporter ») :

```json
{
  "app": "roue-de-fortune",
  "version": 1,
  "categories": [{ "id": "c1", "name": "Boissons", "color": "#d64545" }],
  "results": [{ "id": "r1", "name": "Café", "desc": "…", "cat": "c1", "active": true }]
}
```

À l'import, « Remplacer » écrase la configuration, « Fusionner » ajoute ce qui manque
(les catégories de même nom sont réutilisées, les doublons ignorés).

## Accessibilité

Onglets ARIA navigables aux flèches, piège à focus et fermeture par <kbd>Échap</kbd>
dans les modales, zone de résultat en `aria-live`, libellés sur toutes les icônes,
cibles tactiles d'au moins 44 × 44 px, et respect de `prefers-reduced-motion`
(animations et confettis désactivés).

## Développement

Le fichier est organisé en sections numérotées et commentées : jetons de style et
thèmes, structure, onglet Roue, onglet Gestion, modale, puis le JavaScript
(utilitaires, état, thèmes, roue, gestion, modale, effets). Modifier une couleur de
thème revient à changer quelques variables CSS dans le bloc `[data-skin="…"]`
correspondant.
