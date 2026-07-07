# 🏠 Mes Propriétés / My Properties

**Version 1.7.0**

Application **bilingue (FR/EN)** — bouton de langue dans l'en-tête, choix mémorisé. En anglais, l'app s'appelle *My Properties*.

Application web (PWA) de gestion de propriétés pour le Québec : taxes municipales et scolaires, dépenses d'entretien, entretien préventif, garanties et loyers — le tout dans **un seul fichier HTML**, sans serveur ni dépendance.

## ✨ Fonctionnalités

Les **Réglages** (sauvegarde, synchro nuage, effacement) s'ouvrent via l'icône d'engrenage dans l'en-tête, à côté du sélecteur d'année.

### Tableau de bord
- Vue d'ensemble par année : taxes totales, taxes restantes à payer, dépenses, loyers perçus
- Prochains versements de taxes avec statut (À venir / En retard) et bouton « Marquer payé »
- Entretien à prévoir (tâches dues ou à venir dans 30 jours)
- Garanties actives et baux à renouveler (alerte à 90 jours)
- Graphique des dépenses par catégorie (code couleur)
- Fiche annuelle par propriété : coût total → loyers perçus → **net**

### Propriétés
- Nom, adresse, type (maison, condo, duplex, triplex, chalet, terrain…)
- N° matricule / lot, date et prix d'achat, évaluation municipale, notes
- Chaque propriété a sa propre couleur d'accent dans toute l'app

### Taxes (municipales et scolaires)
- Un compte de taxes par propriété, par type et par année
- Échéancier de 1 à 6 versements, généré automatiquement en montants égaux (ajustables)
- Suivi payé / à venir / en retard, avec date de paiement

### Dépenses
- Date, montant, titre, fournisseur, description
- 10 catégories : Entretien, Réparation, Rénovation, Plomberie, Électricité, Toiture, Extérieur & terrain, Électroménagers, Frais & assurances, Autre
- Marqueur **« Dépense capitalisable »** pour distinguer les rénovations qui ajoutent de la valeur (utile au calcul du gain en capital)
- Pour les **Rénovations** : option « Rend un logement indisponible » avec dates de début/fin — le calendrier d'occupation est marqué automatiquement en indisponible pour ces mois
- Champ **« Garantie jusqu'au »** avec rappel au tableau de bord
- Filtres par propriété et catégorie, export **CSV** compatible Excel (fr-CA : séparateur `;`, virgule décimale)

### Parts (copropriété)
- Propriétaires et parts (%) définis sur chaque propriété (ex. 50/50 ou 60/40) — validation que le total fait 100 %
- Onglet **Parts** : sommaire annuel par propriétaire (loyers, coûts, net agrégés sur toutes les propriétés) et répartition détaillée par propriété (loyers / taxes / dépenses / net selon la part de chacun)

### Entretien planifié
- Tâches récurrentes (ramonage, thermopompe, échangeur d'air, gouttières…)
- Fréquences : mensuelle, aux 3/6 mois, annuelle, aux 2 ans, aux 5 ans
- Bouton « Fait ✓ » qui recalcule automatiquement la prochaine échéance

### Location
- Logements par propriété : loyer mensuel et notes
- Sous-onglet **Occupation** : baux séparés des logements, avec fiche complète du locataire (nom complet, téléphone, courriel, loyer du bail, dates, **départ réel** si le locataire quitte avant l'échéance, notes) et **historique conservé** quand le locataire change — les anciens baux restent consultables
- Grille de 12 mois par année pour cocher les loyers perçus
- Total perçu vs potentiel annuel, rappel de renouvellement de bail
- Sous-onglet **Occupation** : calendrier annuel logements × mois à trois états — occupé, libre, ou indisponible (rénovations, hachuré rouge) — pré-rempli depuis les dates de bail; un tap fait défiler occupé → indisponible → libre. Les mois définis par un bail ou une dépense de rénovation sont verrouillés (modifiez le bail ou la dépense pour les changer). Taux d'occupation (excluant les mois indisponibles) et logements libres par mois
- Sous-onglet **Promotion** : annonces prêtes à copier (Facebook Marketplace, Kijiji…) générées à partir du logement — titre, prix, disponibilité, description, inclusions — avec bouton « Copier l'annonce »

## 🚀 Installation

Aucune installation requise — c'est un fichier HTML autonome.

1. **En local** : ouvrez `mes-proprietes.html` dans votre navigateur.
2. **GitHub Pages** :
   - Créez un dépôt et déposez-y le fichier (renommez-le `index.html` si vous voulez une URL propre)
   - Activez Pages dans *Settings → Pages → Deploy from a branch*
   - Ouvrez l'URL sur votre téléphone et **ajoutez à l'écran d'accueil** pour l'utiliser comme une app

L'interface s'adapte automatiquement au **mode sombre** de votre appareil.

## 💾 Données et sauvegarde

- Les données sont enregistrées en **localStorage**, dans le navigateur de l'appareil. Rien ne quitte votre appareil par défaut.
- **Export / Import JSON** (Réglages) : faites des copies de secours régulières.
- **Synchro nuage optionnelle** via [JSONBin.io](https://jsonbin.io) pour retrouver vos données sur plusieurs appareils :
  1. Créez un compte gratuit sur jsonbin.io et copiez votre **clé maître** (X-Master-Key)
  2. Dans Réglages, collez la clé et cliquez « Envoyer vers le nuage » — le *bin* est **créé automatiquement** et son ID s'inscrit dans le champ
  3. Sur un autre appareil, entrez la clé **et** cet ID de bin, puis « Récupérer du nuage »
  4. Par la suite : « Envoyer » après vos modifications, « Récupérer » sur l'autre appareil

> ⚠️ La synchro est manuelle et fonctionne en « dernier écrit gagne ». Envoyez avant de changer d'appareil.

## 🗂️ Structure des données (JSON)

```json
{
  "properties": [{ "id", "name", "address", "type", "lot", "purchaseDate", "purchasePrice", "currentValue", "owners": [{ "name", "share" }], "notes" }],
  "taxes":      [{ "id", "propertyId", "year", "type": "municipale|scolaire", "total", "installments": [{ "dueDate", "amount", "paid", "paidDate" }], "notes" }],
  "expenses":   [{ "id", "propertyId", "date", "amount", "category", "title", "vendor", "warrantyEnd", "description", "capital" }],
  "recurring":  [{ "id", "propertyId", "title", "freq", "lastDone", "notes" }],
  "units":      [{ "id", "propertyId", "name", "monthlyRent", "payments": { "2026": ["…12 mois"] }, "occupancy": { "2026": ["…12 mois"] }, "ad": { "title", "price", "avail", "desc", "incl" }, "notes" }],
  "leases":     [{ "id", "unitId", "name", "phone", "email", "rent", "start", "end", "departed", "notes" }],
  "settings":   { "binKey", "binId" }
}
```

Le format est rétrocompatible : les collections manquantes sont créées automatiquement au chargement.

## 🛠️ Technique

- HTML / CSS / JavaScript vanilla, un seul fichier, zéro dépendance
- Polices : Fraunces (titres) + Public Sans (texte) via Google Fonts, avec repli système hors-ligne
- Manifeste PWA généré dynamiquement (installable sur l'écran d'accueil)
- Mode sombre automatique (`prefers-color-scheme`)
- Interface bilingue FR/EN (bascule dans l'en-tête) — montants, dates et export CSV adaptés à la langue (`fr-CA` / `en-CA`)

## 🗺️ Idées pour la suite

- Photos de factures et documents (ImgBB)
- Suivi hypothèque et assurances avec rappels de renouvellement
- Historique multi-années des taxes et dépenses (tendances)
- Recherche libre dans les dépenses
- Synchro nuage automatique après chaque modification

## 🔢 Version

La version courante est affichée au bas de l'onglet **Réglages** et incluse dans les exports JSON (`appVersion`). Pour la changer : modifiez la constante `APP_VERSION` au début du `<script>` (et la balise `<meta name="app-version">`).

## 📄 Licence

Projet personnel — utilisez, modifiez et partagez librement.
