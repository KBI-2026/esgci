# User Stories — ESGCI Attestations

> **Projet :** Générateur d'attestations d'inscription bilingues
> **Version :** 1.0 — Juin 2026
> **Rôles :** Scolarité ESGCI · Étudiant / Tiers vérificateur

---

## Légende

| Symbole | Signification |
|---------|--------------|
| 🔴 Must Have | Fonctionnalité indispensable |
| 🟡 Should Have | Fonctionnalité importante |
| 🟢 Could Have | Fonctionnalité souhaitable |

---

## Epic 1 — Accès sécurisé

### US-01 · Authentification par mot de passe 🔴

**En tant que** responsable scolarité ESGCI
**Je veux** accéder au générateur via un mot de passe
**Afin de** réserver l'émission d'attestations au personnel habilité

**Critères d'acceptance**

| # | Critère |
|---|---------|
| 1 | Une page de garde s'affiche avant tout accès au générateur |
| 2 | La saisie du bon mot de passe donne accès à l'application |
| 3 | Un mot de passe incorrect affiche un message d'erreur pendant 2,5 secondes |
| 4 | Le champ mot de passe est vidé après chaque tentative échouée |
| 5 | La touche Entrée soumet le formulaire |
| 6 | L'authentification est mémorisée le temps de la session (fermer l'onglet = déconnexion) |
| 7 | La page de vérification `verify.html` reste accessible sans mot de passe |

---

## Epic 2 — Saisie des données étudiant

### US-02 · Renseigner l'identité d'un étudiant 🔴

**En tant que** responsable scolarité
**Je veux** saisir les informations d'identité d'un étudiant
**Afin de** personnaliser l'attestation avec ses données officielles

**Critères d'acceptance**

| # | Critère |
|---|---------|
| 1 | Je peux saisir : civilité (M. / Mme), n° étudiant, nom, prénom, date de naissance, nationalité |
| 2 | Les champs civilité, n° étudiant, nom, prénom, date de naissance sont obligatoires |
| 3 | Le champ nationalité est optionnel et vaut "Française" par défaut |
| 4 | Un astérisque rouge (*) identifie visuellement les champs obligatoires |
| 5 | Cliquer "Générer" sans remplir les champs requis affiche un message d'erreur |

---

### US-03 · Renseigner les informations de scolarité 🔴

**En tant que** responsable scolarité
**Je veux** saisir le programme et l'année académique de l'étudiant
**Afin que** l'attestation reflète fidèlement sa situation d'inscription

**Critères d'acceptance**

| # | Critère |
|---|---------|
| 1 | Je peux sélectionner parmi 8 programmes (Bachelor, MBA, Mastère, DBA) |
| 2 | Je peux sélectionner l'année académique (2023-2024 à 2026-2027), 2025-2026 présélectionné |
| 3 | Je peux choisir le statut : "Inscrit(e)" ou "En cours" |
| 4 | La date d'inscription et l'email sont optionnels |
| 5 | Si la date d'inscription est renseignée, elle apparaît dans l'attestation |
| 6 | Si la date d'inscription est vide, aucune ligne ne s'affiche dans l'attestation |

---

## Epic 3 — Prévisualisation

### US-04 · Visualiser l'attestation en temps réel 🔴

**En tant que** responsable scolarité
**Je veux** voir une prévisualisation de l'attestation au fur et à mesure de ma saisie
**Afin de** vérifier l'exactitude des informations avant de générer le PDF

**Critères d'acceptance**

| # | Critère |
|---|---------|
| 1 | La prévisualisation se rafraîchit automatiquement 350ms après chaque frappe |
| 2 | Tant que les champs requis ne sont pas remplis, un message d'invite est affiché |
| 3 | Dès que les 5 champs requis sont remplis, le certificat s'affiche |
| 4 | Le QR code apparaît dans la prévisualisation avec le badge "QR · VÉRIFIÉ" |
| 5 | Le watermark "SAMPLE" est visible en diagonale sur la prévisualisation |

---

### US-05 · Basculer entre la page FR et la page EN 🟡

**En tant que** responsable scolarité
**Je veux** prévisualiser les deux pages de l'attestation (française et anglaise)
**Afin de** vérifier le contenu bilingue avant impression

**Critères d'acceptance**

| # | Critère |
|---|---------|
| 1 | Deux onglets permettent de basculer : "🇫🇷 Français · Page 1" et "🇬🇧 English · Page 2" |
| 2 | L'onglet actif est mis en surbrillance |
| 3 | La prévisualisation anglaise affiche tous les textes traduits |
| 4 | Le contenu (nom, programme, etc.) est identique sur les deux pages |

---

## Epic 4 — Génération PDF

### US-06 · Générer une attestation d'inscription bilingue 🔴

**En tant que** responsable scolarité
**Je veux** générer un PDF officiel en un clic
**Afin de** remettre ou envoyer l'attestation à l'étudiant

**Critères d'acceptance**

| # | Critère |
|---|---------|
| 1 | Le bouton "Générer PDF bilingue + QR Code" lance la génération |
| 2 | Le PDF contient exactement 2 pages : page 1 en français, page 2 en anglais |
| 3 | Chaque page contient : en-tête ESGCI, nom complet, programme, n° étudiant, date de naissance, nationalité, année académique, statut, mention légale, QR code, zone de signature |
| 4 | Le nom du fichier suit le format `Attestation_NOM_PRENOM_REF.pdf` |
| 5 | Un numéro de référence unique est généré (format `ESGCI-ATT-XXXXXX`) |
| 6 | Un toast de confirmation s'affiche après la génération |
| 7 | Le bouton est désactivé pendant la génération pour éviter les doublons |

---

### US-07 · Retélécharger une attestation déjà générée 🟡

**En tant que** responsable scolarité
**Je veux** pouvoir retélécharger le PDF d'une attestation déjà générée
**Afin de** ne pas avoir à resaisir les informations en cas d'oubli ou d'erreur d'envoi

**Critères d'acceptance**

| # | Critère |
|---|---------|
| 1 | Le bouton "↓ Télécharger PDF" reste disponible après génération |
| 2 | Le clic retélécharge le même PDF (même référence, mêmes données) |
| 3 | Depuis l'historique, le bouton "↓ PDF" retélécharge chaque attestation de la session |

---

### US-08 · Identifier les attestations comme non-officielles (SAMPLE) 🔴

**En tant que** responsable de la sécurité documentaire
**Je veux** que toutes les attestations générées affichent un watermark "SAMPLE"
**Afin d'** empêcher toute utilisation frauduleuse des documents de démonstration

**Critères d'acceptance**

| # | Critère |
|---|---------|
| 1 | Le watermark "SAMPLE" apparaît en diagonale sur la prévisualisation HTML |
| 2 | Le watermark "SAMPLE" est imprimé en diagonale sur chaque page du PDF |
| 3 | Le watermark est semi-transparent pour ne pas masquer le contenu |
| 4 | Le watermark est présent sur la page française ET la page anglaise |

---

## Epic 5 — QR Code & Vérification

### US-09 · Générer un QR code d'authenticité 🔴

**En tant que** responsable scolarité
**Je veux** que chaque attestation contienne un QR code unique
**Afin de** permettre à un tiers de vérifier l'authenticité du document

**Critères d'acceptance**

| # | Critère |
|---|---------|
| 1 | Un QR code est automatiquement généré et intégré dans la prévisualisation |
| 2 | Le QR code est imprimé dans le PDF (page FR et page EN) |
| 3 | Scanner le QR code redirige vers la page de vérification avec les données encodées |
| 4 | Si le QR code est indisponible, un cachet "OFFICIEL" le remplace dans le PDF |

---

### US-10 · Partager le lien de vérification 🟡

**En tant que** responsable scolarité
**Je veux** copier le lien de vérification d'une attestation
**Afin de** le transmettre par email ou messagerie à un tiers sans partager le PDF

**Critères d'acceptance**

| # | Critère |
|---|---------|
| 1 | Le bouton "⧉ Lien de vérification" copie l'URL dans le presse-papier |
| 2 | Un toast "⧉ Lien copié !" confirme la copie |
| 3 | Si aucune attestation n'a été générée, un toast d'avertissement s'affiche |

---

### US-11 · Vérifier l'authenticité d'une attestation 🔴

**En tant que** employeur, administration ou tout tiers
**Je veux** vérifier qu'une attestation d'inscription ESGCI est authentique
**Afin de** m'assurer que le document n'a pas été falsifié

**Critères d'acceptance**

| # | Critère |
|---|---------|
| 1 | La page de vérification est accessible sans mot de passe |
| 2 | Scanner le QR code ou ouvrir le lien affiche les informations de l'étudiant |
| 3 | Si le document est authentique : badge vert "✓ Attestation authentique" |
| 4 | Les informations affichées (nom, n° étudiant, programme, année) correspondent au PDF |
| 5 | L'empreinte numérique (hash) est visible pour audit |
| 6 | Si le document est falsifié : badge rouge "✗ Attestation non vérifiée" |
| 7 | Si le lien est invalide ou corrompu : message d'erreur explicite |
| 8 | Un bouton "Imprimer" permet d'archiver le résultat de la vérification |

---

## Epic 6 — Historique de session

### US-12 · Consulter l'historique des attestations générées 🟡

**En tant que** responsable scolarité
**Je veux** voir la liste des attestations générées pendant ma session
**Afin de** suivre ce que j'ai produit et éviter les doublons

**Critères d'acceptance**

| # | Critère |
|---|---------|
| 1 | Chaque nouvelle génération ajoute une entrée dans l'historique |
| 2 | L'historique s'ouvre automatiquement après la première génération |
| 3 | Chaque entrée affiche : référence, civilité + prénom + nom, programme, année |
| 4 | L'historique est limité aux 10 dernières attestations |
| 5 | L'historique est vidé à la fermeture de l'onglet (pas de persistance) |

---

### US-13 · Retrouver et afficher une attestation passée 🟡

**En tant que** responsable scolarité
**Je veux** retrouver une attestation générée précédemment dans la même session
**Afin de** la réafficher ou la retélécharger sans ressaisir les données

**Critères d'acceptance**

| # | Critère |
|---|---------|
| 1 | Le bouton "👁 Voir" réaffiche le certificat dans la zone de prévisualisation |
| 2 | Le bouton "↓ PDF" retélécharge le PDF correspondant |
| 3 | L'écran défile vers la prévisualisation lors du clic sur "👁 Voir" |

---

## Epic 7 — Interface utilisateur

### US-14 · Utiliser l'interface en anglais 🟡

**En tant que** responsable scolarité non-francophone
**Je veux** basculer l'interface du formulaire en anglais
**Afin de** utiliser l'application dans ma langue de travail

**Critères d'acceptance**

| # | Critère |
|---|---------|
| 1 | Un bouton "EN" / "FR" est visible dans le header |
| 2 | Cliquer "EN" traduit tous les labels, placeholders, boutons et messages |
| 3 | Cliquer "FR" revient à l'interface française |
| 4 | Le contenu du certificat (toujours bilingue) n'est pas affecté par ce toggle |
| 5 | Les messages d'erreur et toasts suivent également la langue sélectionnée |

---

### US-15 · Utiliser le mode sombre 🟢

**En tant que** responsable scolarité
**Je veux** activer un mode sombre sur l'interface
**Afin de** réduire la fatigue visuelle lors d'une utilisation prolongée

**Critères d'acceptance**

| # | Critère |
|---|---------|
| 1 | Un bouton toggle est disponible dans le header |
| 2 | Cliquer le toggle bascule entre mode clair et mode sombre |
| 3 | Toute l'interface (formulaire, prévisualisation, historique) change de thème |
| 4 | La transition est animée (0,3s) |

---

### US-16 · Utiliser l'application sur mobile 🟢

**En tant que** responsable scolarité en déplacement
**Je veux** accéder au générateur depuis mon téléphone
**Afin de** émettre une attestation sans être à mon poste fixe

**Critères d'acceptance**

| # | Critère |
|---|---------|
| 1 | Sur un écran ≤ 860px, le formulaire s'affiche en colonne unique |
| 2 | La prévisualisation s'affiche sous le formulaire |
| 3 | Tous les boutons et champs sont utilisables au toucher |
| 4 | Le certificat s'adapte à la largeur de l'écran |

---

## Récapitulatif

| Epic | User Stories | Must Have | Should Have | Could Have |
|------|-------------|-----------|-------------|------------|
| Accès sécurisé | 1 | 1 | 0 | 0 |
| Saisie données | 2 | 2 | 0 | 0 |
| Prévisualisation | 2 | 1 | 1 | 0 |
| Génération PDF | 3 | 2 | 1 | 0 |
| QR & Vérification | 3 | 2 | 1 | 0 |
| Historique | 2 | 0 | 2 | 0 |
| Interface | 3 | 0 | 1 | 2 |
| **Total** | **16** | **8** | **6** | **2** |
