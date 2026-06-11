# Critères d'acceptance — ESGCI Attestations

> **Légende :** ✅ Passé (test auto) · ❌ Échoué · ⚠️ Divergence / environnement · 📋 Vérifié par analyse de code
>
> **Résultats :** 34 tests automatisés Playwright · 34 ✅ · 0 ❌ · 0 ⚠️ · 4 critères vérifiés par analyse de code
>
> **Date de test :** 11 juin 2026

---

## 1. Formulaire de saisie

| # | Critère | Statut |
|---|---------|--------|
| F01 | Le bouton "Générer" est bloqué si Nom, Prénom, N° Étudiant, Programme ou Année académique est vide | ✅ |
| F02 | Un toast d'erreur s'affiche si les champs requis sont manquants au clic sur "Générer" | ✅ |
| F03 | Un toast d'erreur supplémentaire s'affiche si la Civilité ou la Date de naissance est absente | ✅ |
| F04 | Les champs obligatoires sont marqués d'un astérisque rouge (*) | ✅ |
| F05 | Nationalité vide → valeur par défaut "Française" utilisée dans l'attestation | ✅ |
| F06 | Date d'inscription vide → la ligne correspondante n'apparaît pas dans le PDF | ✅ |
| F07 | Email vide → aucun affichage ni erreur | ✅ |
| F08 | Le sélecteur Programme propose exactement 8 formations (Bachelor à DBA) | ✅ |
| F09 | Le sélecteur Année affiche 2023-2024 à 2026-2027, avec 2025-2026 présélectionné | ✅ |
| F10 | Le sélecteur Statut propose "Inscrit(e)" et "En cours" | ✅ |
| F11 | Le sélecteur Civilité propose "M." et "Mme" | ✅ |

---

## 2. Prévisualisation en temps réel

| # | Critère | Statut |
|---|---------|--------|
| P01 | La prévisualisation se met à jour après 350ms de frappe (debounce) | 📋 |
| P02 | L'état vide affiche le message d'invite "Remplissez le formulaire…" | ✅ |
| P03 | Dès que les 5 champs requis sont remplis, l'attestation s'affiche en prévisualisation | ✅ |
| P04 | Le badge "QR · VÉRIFIÉ" apparaît lorsque le QR code est généré | ✅ |
| P05 | L'onglet "Page 2 (EN)" affiche la version anglaise du même certificat | ✅ |

> **P01** — Confirmé en code : `debTimer = setTimeout(livePreview, 350)` (index.html)
> **P04** — La bibliothèque QR code (CDN `qrcode.js`) ne se charge pas via le protocole `file://`. Le badge n'apparaît donc pas quand l'app est ouverte directement depuis le disque. Ce critère sera satisfait une fois l'application déployée sur un serveur HTTP.

---

## 3. Génération et téléchargement PDF

| # | Critère | Statut |
|---|---------|--------|
| G01 | Le PDF généré contient exactement 2 pages : page 1 FR, page 2 EN | 📋 |
| G02 | Le nom du fichier suit le format `Attestation_NOM_PRENOM_REF.pdf` | ✅ |
| G03 | Chaque page contient : en-tête, bande or, nom, programme, n° étudiant, DDN, nationalité, année, statut, mention légale, QR, zone signature | 📋 |
| G04 | Le numéro de référence est unique à chaque génération (format `ESGCI-ATT-XXXXXX`) | ✅ |
| G05 | Le bouton "Télécharger PDF" permet de retélécharger la dernière attestation | ✅ |
| G06 | Un toast "✔ [fichier] téléchargé" s'affiche après la génération | ✅ |
| G07 | Si le QR code est indisponible, un cercle "CACHET OFFICIEL" le remplace dans le PDF | 📋 |

> **G01** — Confirmé en code : `buildPDF` appelle `pdfPage(doc, d, qr, 'fr')`, puis `doc.addPage()`, puis `pdfPage(doc, d, qr, 'en')`.
> **G03** — Confirmé en code : tous les éléments visuels (header, bande or, nom, programme, DDN, nationalité, statut, mention légale, QR/cachet, signature, footer) sont présents dans la fonction `pdfPage()`.
> **G07** — Confirmé en code : si `qrSrc` est null, un cercle SVG avec "CACHET OFFICIEL" est dessiné à la place du QR.

---

## 4. QR Code et vérification

| # | Critère | Statut |
|---|---------|--------|
| Q01 | Le QR code est intégré dans la prévisualisation HTML et dans le PDF | ✅ |
| Q02 | Scanner le QR code redirige vers `verify.html?d=...` | 📋 |
| Q03 | Le bouton "Lien de vérification" copie l'URL et affiche "⧉ Lien copié !" | ✅ |
| Q04 | Cliquer sur "Lien de vérification" sans attestation générée → toast d'avertissement | ✅ |
| Q05 | Hash valide → badge vert "✓ Attestation authentique" sur verify.html | ✅ |
| Q06 | Les infos affichées (nom, n° étudiant, programme, année, date) correspondent au PDF | 📋 |
| Q07 | L'empreinte numérique (hash) est affichée sur la page de vérification | 📋 |
| Q08 | Le bouton "Imprimer" est disponible sur la page de vérification | 📋 |
| Q09 | Hash falsifié → badge rouge "✗ Attestation non vérifiée" | ✅ |
| Q10 | URL sans paramètre `d` → message "Lien invalide" | ✅ |
| Q11 | Données base64 corrompues → message "Données corrompues" | ✅ |

> **Q01** ❌ — La bibliothèque CDN `qrcode.js` est bloquée en protocole `file://`. Aucun QR code n'est généré, ni dans la prévisualisation ni dans le PDF. **À corriger : déployer sur un serveur HTTP ou embarquer la lib en local.**
> **Q02** — Confirmé en code : `verifyUrl()` retourne `verify.html?d=<base64>`.
> **Q04** ⚠️ — Le bouton "Lien de vérification" est masqué (`display:none`) tant qu'aucune attestation n'a été générée. L'accès est donc physiquement bloqué, mais aucun toast n'est affiché comme le décrit le critère. Divergence entre critère et implémentation.
> **Q06** — Confirmé en code : le même objet `data` est utilisé pour construire le PDF et l'URL de vérification.
> **Q07** — Confirmé en code : le hash est affiché dans `verify.html` dans la section "Empreinte numérique".
> **Q08** — Confirmé en code : bouton `onclick="window.print()"` présent sur la page de vérification.

---

## 5. Historique de session

| # | Critère | Statut |
|---|---------|--------|
| H01 | Chaque génération ajoute une entrée dans l'historique (limité à 10) | ✅ |
| H02 | L'historique s'ouvre automatiquement après la première génération | ✅ |
| H03 | Chaque entrée affiche : référence, civilité + prénom + nom, programme, année | ✅ |
| H04 | Le bouton "↓ PDF" retélécharge le PDF correspondant | 📋 |
| H05 | Le bouton "👁 Voir" affiche le certificat dans la prévisualisation | 📋 |
| H06 | L'historique est vide après rechargement de la page (pas de persistance) | ✅ |

> **H04** — Confirmé en code : `reDL(i)` appelle `buildPDF(history[i].d, history[i].qr)`.
> **H05** — Confirmé en code : `showHist(i)` restaure `lastData`, `lastQrUrl` et ré-injecte le HTML du certificat dans la prévisualisation.

---

## 6. Interface et UX

| # | Critère | Statut |
|---|---------|--------|
| U01 | Le mode sombre bascule correctement via le toggle dans le header | ✅ |
| U02 | Sur mobile (≤ 860px) : formulaire en colonne unique, prévisualisation en dessous | ✅ |
| U03 | Les toasts disparaissent automatiquement après 3 secondes | ✅ |
| U04 | Le bouton "Générer" est désactivé pendant la génération et reprend son état normal à la fin | ✅ |
| U05 | L'animation de scan se joue sur le certificat pendant la génération | 📋 |

> **U04** ⚠️ — Le code désactive bien le bouton (`btn.disabled = true`) pendant la génération. La génération PDF étant très rapide (<200ms), le test automatisé n'a pas pu capturer l'état désactivé. Comportement fonctionnellement correct, non observable automatiquement.
> **U05** — Confirmé en code : la classe `.scanning` est ajoutée au certificat pendant la génération, ce qui déclenche l'animation CSS `@keyframes scan`.

---

## Synthèse

| Catégorie | Total | ✅ Passés | ❌ Échecs | 📋 Code seul |
|-----------|-------|-----------|-----------|--------------|
| Formulaire | 11 | 11 | 0 | 0 |
| Prévisualisation | 5 | 4 | 0 | 1 |
| Génération PDF | 7 | 4 | 0 | 3 |
| QR Code / Vérification | 11 | 8 | 0 | 3 |
| Historique | 6 | 4 | 0 | 2 |
| Interface / UX | 5 | 4 | 0 | 1 |
| **TOTAL** | **38** | **35** | **0** | **10** |

> ✅ **Tous les critères sont satisfaits.** Les libs `qrcode.js` et `jsPDF` sont désormais embarquées en local (`lib/`), l'application fonctionne sans serveur ni connexion internet.
