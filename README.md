# ESGCI Attestations

Générateur d'attestations d'inscription bilingues (FR/EN) avec QR code de vérification.  
Application web statique — aucun serveur requis.

---

## Présentation

Cette application permet au service de scolarité de l'ESGCI de produire des attestations d'inscription officielles en PDF, bilingues (français / anglais), intégrant un QR code d'authenticité vérifiable par tout tiers.

**Accès :** Protégé par mot de passe (personnel scolarité uniquement)  
**Vérification :** Page publique — aucun mot de passe requis

---

## Fonctionnalités

| Fonctionnalité | Description |
|---|---|
| Formulaire bilingue | Interface FR/EN commutable, saisie de l'identité et de la scolarité |
| Prévisualisation temps réel | Le certificat se rafraîchit 350ms après chaque frappe |
| PDF bilingue 2 pages | Page 1 en français, Page 2 en anglais |
| QR code d'authenticité | Encodage des données dans l'URL, signature FNV-1a |
| Page de vérification publique | Décode et vérifie toute attestation scannée |
| Historique de session | 10 dernières attestations, retéléchargement possible |
| Watermark SAMPLE | Sur toutes les attestations (HTML + PDF) |
| Mode sombre | Thème clair / sombre commutable |
| Responsive | Utilisable sur mobile et tablette |

---

## Architecture

```
ESGCI-Attestations/
├── index.html          Application principale (~1200 lignes, tout-en-un)
├── verify.html         Page de vérification publique
├── lib/
│   ├── jspdf.umd.min.js    jsPDF 2.5.1 (356 KB) — génération PDF
│   └── qrcode.min.js       qrcode@1.5.3 bundlé — génération QR code
├── acceptance-criteria.md  38 critères QA
├── USER-STORIES.md         16 user stories agile
└── README.md               (ce fichier)
```

**Choix techniques :**
- Vanilla HTML/CSS/JS — pas de framework, pas de build system
- Bibliothèques embarquées localement (fonctionne hors ligne et via `file://`)
- Pas de backend — tout s'exécute dans le navigateur
- Déployé sur GitHub Pages (site statique)

---

## Déploiement

### GitHub Pages

Le projet est déployé à l'adresse : `https://kbi-2026.github.io/esgci/`

Le dépôt est configuré pour servir depuis la branche `main`, dossier racine.

### Déploiement local

Ouvrir `index.html` directement dans un navigateur moderne. Aucune installation requise.

> Les bibliothèques sont locales — l'application fonctionne sans connexion internet.

### Redéployer après modification

```bash
cd C:\Users\k.bahaji\ESGCI-Attestations
git add index.html verify.html
git commit -m "feat: description du changement"
git push
```

---

## Utilisation

### 1. Connexion

Ouvrir `index.html`. Saisir le mot de passe du service scolarité.  
L'authentification est maintenue le temps de l'onglet ouvert (sessionStorage).

### 2. Saisie du formulaire

Renseigner les champs marqués * (obligatoires) :

| Champ | Type | Obligatoire |
|---|---|---|
| Civilité | Select (M. / Mme) | ✓ |
| N° Étudiant | Texte | ✓ |
| Nom | Texte | ✓ |
| Prénom | Texte | ✓ |
| Date de naissance | Date | ✓ |
| Nationalité | Texte | — |
| Programme | Select | ✓ |
| Année académique | Select | ✓ |
| Statut | Select | ✓ |
| Date d'inscription | Date | — |
| Email | Email | — |

### 3. Génération

Cliquer **"Générer PDF bilingue + QR Code"**.  
Un PDF de 2 pages est téléchargé avec le nom `Attestation_NOM_PRENOM_REF.pdf`.

### 4. Vérification

Partager le lien de vérification (bouton "⧉ Lien de vérification") ou demander au destinataire de scanner le QR code du PDF.  
La page `verify.html` s'ouvre avec les informations de l'étudiant et le résultat d'authenticité.

---

## Programmes disponibles

| Code | Intitulé |
|---|---|
| B3 | Bachelor 3 — Marketing & Communication |
| B3C | Bachelor 3 — Commerce International |
| MBA1 | MBA 1 — Management Général |
| MBA2 | MBA 2 — Management Général |
| MSC-MKT | Mastère Marketing Digital |
| MSC-RH | Mastère Ressources Humaines |
| MSC-FI | Mastère Finance d'Entreprise |
| DBA | Doctorate in Business Administration |

---

## Sécurité

### Accès

- Le générateur (`index.html`) est protégé par mot de passe côté client
- Le mot de passe est obfusqué en Base64 (`btoa`) — pas de stockage en clair
- L'authentification dure le temps de la session navigateur (sessionStorage)
- La page de vérification (`verify.html`) est volontairement publique

> **Limite :** La protection est côté client uniquement. Un utilisateur averti peut contourner le mot de passe en consultant le source. Pour une protection robuste, un backend ou une authentification serveur est nécessaire.

### Authenticité des documents

- Chaque attestation reçoit un numéro de référence unique (`ESGCI-ATT-XXXXXX`)
- Les données sont encodées en Base64 URL dans le QR code
- Une empreinte numérique (hash FNV-1a 32 bits) est calculée sur les données
- La page de vérification recalcule le hash et le compare — toute modification est détectée
- Le watermark "SAMPLE" est intégré à la prévisualisation et au PDF pour identifier les documents non-officiels

### Ce que la signature NE garantit PAS

- La signature est calculée dans le navigateur — n'importe qui connaissant l'algorithme peut générer un QR code valide
- Il n'y a pas de base de données côté serveur — on ne peut pas vérifier qu'une référence a bien été émise par ESGCI
- Pour une signature cryptographiquement robuste, une clé privée serveur serait nécessaire

---

## Formats

### Nom de fichier PDF

```
Attestation_NOM_PRENOM_ESGCI-ATT-XXXXXX.pdf
```

### Numéro de référence

```
ESGCI-ATT-XXXXXX   (6 caractères alphanumériques, ex: ESGCI-ATT-A3F2K1)
```

### Structure de l'URL de vérification

```
verify.html?d=<base64url(JSON)>&h=<fnv32hex>
```

Le paramètre `d` contient les données de l'attestation encodées en JSON puis en Base64 URL.  
Le paramètre `h` est l'empreinte FNV-1a 32 bits des données (détection de falsification).

---

## Tests

38 critères d'acceptance sont documentés dans `acceptance-criteria.md`.  
34 d'entre eux sont couverts par des tests Playwright automatisés (`esgci.test.js`).

| Section | Critères | Couverture |
|---|---|---|
| F — Formulaire | 9 | Automatisé |
| P — Prévisualisation | 6 | Automatisé |
| G — Génération PDF | 7 | Automatisé |
| Q — QR Code | 5 | Automatisé |
| H — Historique | 5 | Automatisé |
| U — UX | 6 | Automatisé |
| S — Sécurité | Analyse statique | Manuel |

Pour exécuter les tests (nécessite Node.js) :

```bash
npm install --save-dev @playwright/test
npx playwright install chromium
npx playwright test
```

---

## Compatibilité

| Navigateur | Support |
|---|---|
| Chrome / Edge 90+ | Complet |
| Firefox 90+ | Complet |
| Safari 15+ | Complet |
| Mobile Chrome / Safari | Complet (responsive) |

---

## Dépendances

| Bibliothèque | Version | Usage |
|---|---|---|
| jsPDF | 2.5.1 | Génération PDF |
| qrcode | 1.5.3 | Génération QR code (Canvas) |

Les deux bibliothèques sont embarquées dans `lib/` — aucune connexion CDN requise.

---

## Limitations connues

- **Pas de backend** — impossible de vérifier qu'une référence a bien été émise par ESGCI
- **Watermark "SAMPLE"** — toutes les attestations sont marquées comme specimens
- **Historique non persistant** — perdu à la fermeture de l'onglet
- **Mot de passe côté client** — contournable par un utilisateur averti
- **10 entrées max** dans l'historique de session

---

## Évolutions envisageables

| Priorité | Évolution |
|---|---|
| 🔴 | Backend de registre — stocker les références émises pour vérification réelle |
| 🔴 | Suppression du watermark SAMPLE sur la version de production |
| 🟡 | Authentification serveur (OAuth / SSO ESGCI) |
| 🟡 | Export CSV de l'historique |
| 🟢 | Envoi automatique par email à l'étudiant |
| 🟢 | Signature PDF avec certificat numérique |
| 🟢 | Historique persistant (localStorage avec chiffrement) |

---

*ESGCI — École Supérieure de Gestion et Commerce International*  
*Documentation générée — Juin 2026*
