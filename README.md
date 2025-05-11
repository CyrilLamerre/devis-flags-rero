# Devis en ligne - Drapeaux Publicitaires

Application web moderne pour générer des devis personnalisés de drapeaux publicitaires avec sélection produits, formulaire client, génération PDF, et suivi en back-office.

## 🗂️ État des lieux

### 1. Dépôt GitHub

- **URL**: https://github.com/CyrilLamerre/devis-flags
- **Branche par défaut**: main
- **Branches actives**: main (structure + front complet)
- **Licence**: MIT
- **Actions CI**: `.github/workflows/build.yml` → lint + npm run build

#### 1.1 Arborescence principale

```
/
├─ netlify/
│   └─ functions/
│       ├─ creerDevis.ts      // POST – crée devis + lignes
│       └─ trackDevis.ts      // GET – +1 ouverture, change statut
├─ public/
│   ├─ template.pdf           // placeholder (sera remplacé par Page Designer)
│   └─ favicon.svg
├─ src/
│   ├─ components/            // Catalogue, Accessoires, Panier, etc.
│   ├─ pages/                 // Home, DevisToken, Admin
│   └─ lib/
│       ├─ airtable.ts        // wrapper fetch → Airtable REST
│       └─ auth.ts            // Basic auth admin
├─ .env.sample                // clés à dupliquer en local
├─ tailwind.config.js
├─ vite.config.ts             // proxy Netlify functions en dev
└─ package.json               // React 18 + Vite + Tailwind
```

### 2. Secrets & variables d'environnement

| Nom (GitHub + Netlify) | Valeur prod | Rôle |
|------------------------|-------------|------|
| AIRTABLE_API_KEY | patRnhqXi65AoDxa1.15d16ae73655300c16e79a7e550673fcb434f7102c18175c384394e7ff4b441c | PAT "personal access token" |
| AIRTABLE_BASE_ID | appj44XfwtsHTfLNT | ID base "Flags" |
| ADMIN_PASSWORD | FlagAdmin2025! (à changer avant mise en prod) | Accès back-office /admin |

### 3. Airtable – schéma définitif

#### 3.1 Tables & principaux champs

| Table | Champs clés |
|-------|------------|
| Produits | Catégorie · Format · Nom complet · Dimensions · Description · Fournisseur (link) · Photo · Prix TTC (€) · Poids (kg) |
| Fournisseurs | Ref fournisseur (clé) · Nom fournisseur · Prix achat (€) · Délai (j) · MOQ · Produits (link) |
| Devis | N° devis · Jeton · Coordonnées client · Total TTC (€) · PDF (attachment) · Statut (nouveau/rouvert/téléchargé) · Ouvertures |
| Lignes de devis | Devis (link) · Produit (link) · Quantité · Prix unit TTC (lookup) · Sous-total (€) |

#### 3.2 Automatisation Page Designer

- **Bloc Page Designer**: à finaliser (layout A4)
- **Automation**: Trigger "Record created (Devis)" → Script "Generate PDF & attach to field PDF"

### 4. Front / UX

| Écran | URL | Contenu |
|-------|-----|---------|
| Catalogue | / | Choix modèle · formats (+ quantités) · accessoires |
| Modale client | — | Validation formulaire (Nom, Email, Tel, etc.) |
| Page lien magique | /devis/:token | Loader → polling PDF (3 s) → bouton Télécharger |
| Back-office | /admin | Tableau Devis (statut, ouvertures, CSV export) |

#### Palette & typographie
- Police : Montserrat
- Primaire : #fca502
- Texte foncé : #1a1a1f
- Fond gris clair : #e5e5e5

### 5. Netlify – à faire

1. Create a new site → Import from GitHub (devis-flags)
2. Build cmd `npm run build` – Publish dir `dist`
3. Add env vars : AIRTABLE_API_KEY, AIRTABLE_BASE_ID, ADMIN_PASSWORD
4. Fichier _redirects (déjà dans public/) pour le routing SPA :
   ```
   /*    /index.html   200
   ```
5. Custom domain (optionnel) : devis-flags.netlify.app ou autre

### 6. Retouches prévues / TODO rapides

| Priorité | Item |
|----------|------|
| Haute | Finaliser Page Designer + activer Automation (génère PDF) |
| Haute | Reporter les 3 secrets dans Netlify |
| Moyenne | Uploader les photos produits (colonne Photo – Produits) |
| Basse | Remplir Prix TTC (€) pour chaque produit |
| Basse | Personnaliser favicon & meta-tags SEO |

### 7. Accès & tests

| Ressource | Identifiant |
|-----------|------------|
| Admin back-office | user : admin / pwd : FlagAdmin2025! |
| Exemple de jeton | (sera dispo dès qu'un devis est créé depuis le front) |

## ➡️ Étapes suivantes conseillées

1. Créer le site Netlify & lancer un build
2. Tester le parcours utilisateur ; vérifier que les enregistrements apparaissent bien dans Airtable
3. Brancher le PDF : dès que l'automatisation ajoute le PDF dans le champ PDF, la page lien magique activera immédiatement le téléchargement et mettra Statut = rouvert

## 🚀 Déploiement

Tout est prêt côté repo / code ; la balle est désormais dans le camp du dev Cursor pour :
- finir la partie Netlify (5 mn)
- brancher Page Designer (10 mn)
- pousser les photos / prix dans Airtable

Une fois ces deux mini-tâches terminées, la V1 sera exploitable en prod. 🎉
